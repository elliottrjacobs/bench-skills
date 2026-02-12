---
title: Authenticate Realtime Channel Subscriptions
impact: MEDIUM
impactDescription: Unprotected channels leak data to any connected client
tags: realtime, channels, subscriptions, authentication, broadcast
---

## Authenticate Realtime Channel Subscriptions

Supabase Realtime supports three channel types: Broadcast (client-to-client), Presence (online status), and Postgres Changes (database events). Postgres Changes respect RLS, but Broadcast and Presence channels are open by default.

**Incorrect (broadcast channel accessible to anyone):**

```typescript
// Any client can subscribe to this channel and receive all messages
const channel = supabase.channel('room-123')
  .on('broadcast', { event: 'message' }, (payload) => {
    console.log(payload) // receives all messages
  })
  .subscribe()

// Any client can also send messages
channel.send({
  type: 'broadcast',
  event: 'message',
  payload: { text: 'I can impersonate anyone' },
})
```

**Correct (use RLS-protected Postgres Changes instead of raw broadcast):**

```typescript
// Postgres Changes automatically respect RLS policies
// Only changes the user is allowed to see will be delivered
const channel = supabase
  .channel('my-messages')
  .on(
    'postgres_changes',
    {
      event: 'INSERT',
      schema: 'public',
      table: 'messages',
      filter: `room_id=eq.${roomId}`,
    },
    (payload) => {
      // Only receives messages allowed by RLS policy
      console.log(payload.new)
    }
  )
  .subscribe()
```

**For Broadcast/Presence — use Realtime Authorization:**

```sql
-- Enable Realtime Authorization in Supabase Dashboard
-- Then create policies on the realtime schema

-- Only room members can subscribe to room channels
create policy "Room members can subscribe"
  on realtime.messages for select
  using (
    exists (
      select 1 from public.room_members
      where room_id = (realtime.messages.extension::jsonb ->> 'room_id')::uuid
        and user_id = auth.uid()
    )
  );
```

**Key rule:** Prefer Postgres Changes (RLS-protected) over Broadcast (open) for sensitive data. Use Broadcast only for ephemeral, non-sensitive data like typing indicators or cursor positions.

Reference: https://supabase.com/docs/guides/realtime/authorization
