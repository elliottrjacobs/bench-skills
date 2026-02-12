---
title: Verify JWT in Edge Functions
impact: MEDIUM
impactDescription: Unverified requests allow anyone to call edge functions as any user
tags: edge-functions, jwt, auth, verification, middleware
---

## Verify JWT in Edge Functions

Edge Functions receive the Authorization header from the client, but you must explicitly verify the JWT and extract the user. Without verification, anyone can call the function with a forged or expired token.

**Incorrect (trusting the JWT without verification):**

```typescript
// supabase/functions/process-payment/index.ts
Deno.serve(async (req) => {
  // WRONG: parsing JWT without verification
  const token = req.headers.get('Authorization')?.replace('Bearer ', '')
  const payload = JSON.parse(atob(token!.split('.')[1]))
  const userId = payload.sub // could be forged!

  // Processing payment for potentially forged user ID...
  await chargeUser(userId)
})
```

**Correct (verify JWT using Supabase client):**

```typescript
import { createClient } from 'https://esm.sh/@supabase/supabase-js@2'

Deno.serve(async (req) => {
  const supabase = createClient(
    Deno.env.get('SUPABASE_URL')!,
    Deno.env.get('SUPABASE_ANON_KEY')!,
    {
      global: {
        headers: { Authorization: req.headers.get('Authorization')! },
      },
    }
  )

  // This verifies the JWT and returns the authenticated user
  const { data: { user }, error } = await supabase.auth.getUser()

  if (error || !user) {
    return new Response(JSON.stringify({ error: 'Unauthorized' }), {
      status: 401,
      headers: { 'Content-Type': 'application/json' },
    })
  }

  // user.id is now verified — safe to use
  await chargeUser(user.id)

  return new Response(JSON.stringify({ success: true }), {
    headers: { 'Content-Type': 'application/json' },
  })
})
```

**For admin operations in edge functions:**

```typescript
// Use service role client for operations that need to bypass RLS
const supabaseAdmin = createClient(
  Deno.env.get('SUPABASE_URL')!,
  Deno.env.get('SUPABASE_SERVICE_ROLE_KEY')!  // available in edge function env
)

// But ALWAYS verify the user first with the anon client
// before doing admin operations on their behalf
```

Reference: https://supabase.com/docs/guides/functions/auth
