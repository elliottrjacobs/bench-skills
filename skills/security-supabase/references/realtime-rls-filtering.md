---
title: Ensure RLS Applies to Realtime Events
impact: MEDIUM
impactDescription: Postgres Changes can leak data if RLS is misconfigured on the source table
tags: realtime, rls, postgres-changes, filtering, publications
---

## Ensure RLS Applies to Realtime Events

Postgres Changes events are filtered by RLS, but only if the source table has RLS enabled AND the Realtime publication is configured correctly. A common mistake is enabling Realtime on a table that lacks proper RLS.

**Incorrect (Realtime on table without adequate RLS):**

```sql
-- Table has RLS but with an overly permissive policy
create table public.orders (
  id uuid primary key,
  user_id uuid references auth.users,
  amount numeric,
  status text
);

alter table public.orders enable row level security;

-- This policy lets ALL authenticated users read ALL orders
create policy "Authenticated can read orders"
  on public.orders for select
  using (auth.role() = 'authenticated');

-- Now if Realtime is enabled on orders,
-- every user receives INSERT/UPDATE events for ALL orders
```

**Correct (Realtime with properly scoped RLS):**

```sql
-- Proper RLS: users only see their own orders
create policy "Users read own orders"
  on public.orders for select
  using (auth.uid() = user_id);

-- Now Realtime events are filtered per user
-- User A only receives events for their own orders
```

```typescript
// Client subscription — RLS automatically filters
const channel = supabase
  .channel('my-orders')
  .on(
    'postgres_changes',
    {
      event: '*',
      schema: 'public',
      table: 'orders',
    },
    (payload) => {
      // Only receives events for rows this user can SELECT
    }
  )
  .subscribe()
```

**Enable Realtime on specific tables only:**

```sql
-- Don't use "supabase_realtime" publication for all tables
-- Add only tables that need it
alter publication supabase_realtime add table public.messages;
alter publication supabase_realtime add table public.orders;

-- Check which tables are in the publication
select * from pg_publication_tables
where pubname = 'supabase_realtime';
```

Reference: https://supabase.com/docs/guides/realtime/postgres-changes
