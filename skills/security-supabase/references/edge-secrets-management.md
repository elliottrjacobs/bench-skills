---
title: Use Secrets for Sensitive Values in Edge Functions
impact: MEDIUM
impactDescription: Hardcoded secrets in function code are exposed in version control and logs
tags: edge-functions, secrets, environment-variables, api-keys, hardcoded
---

## Use Secrets for Sensitive Values in Edge Functions

Edge functions should never contain hardcoded API keys, webhook secrets, or other credentials. Use Supabase Secrets (environment variables) which are encrypted at rest and injected at runtime.

**Incorrect (hardcoded secrets in function code):**

```typescript
// supabase/functions/send-email/index.ts
Deno.serve(async (req) => {
  const RESEND_API_KEY = 're_abc123...'  // hardcoded in source code!
  const STRIPE_SECRET = 'sk_live_...'    // committed to git!

  const res = await fetch('https://api.resend.com/emails', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${RESEND_API_KEY}`,
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ /* email data */ }),
  })
})
```

**Correct (use Supabase Secrets):**

```bash
# Set secrets via CLI (encrypted, not in source code)
supabase secrets set RESEND_API_KEY=re_abc123...
supabase secrets set STRIPE_WEBHOOK_SECRET=whsec_...

# List current secrets (values are hidden)
supabase secrets list
```

```typescript
// supabase/functions/send-email/index.ts
Deno.serve(async (req) => {
  // Access secrets via environment variables
  const resendKey = Deno.env.get('RESEND_API_KEY')
  if (!resendKey) {
    return new Response('Server configuration error', { status: 500 })
  }

  const res = await fetch('https://api.resend.com/emails', {
    method: 'POST',
    headers: {
      'Authorization': `Bearer ${resendKey}`,
      'Content-Type': 'application/json',
    },
    body: JSON.stringify({ /* email data */ }),
  })
})
```

**Built-in secrets** (automatically available in edge functions):
- `SUPABASE_URL` — project URL
- `SUPABASE_ANON_KEY` — anon key
- `SUPABASE_SERVICE_ROLE_KEY` — service role key
- `SUPABASE_DB_URL` — direct database connection string

**Never log secrets:**

```typescript
// WRONG
console.log('Using key:', Deno.env.get('STRIPE_SECRET'))

// RIGHT — log that the secret exists, not its value
console.log('Stripe configured:', !!Deno.env.get('STRIPE_SECRET'))
```

Reference: https://supabase.com/docs/guides/functions/secrets
