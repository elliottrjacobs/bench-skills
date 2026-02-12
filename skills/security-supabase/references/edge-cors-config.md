---
title: Configure CORS for Edge Functions
impact: MEDIUM
impactDescription: Missing CORS blocks legitimate clients; wildcard CORS allows cross-site abuse
tags: edge-functions, cors, headers, origin, security
---

## Configure CORS for Edge Functions

Edge functions need CORS headers to be callable from browsers. Missing CORS blocks your frontend; wildcard (`*`) CORS lets any website call your function on behalf of a logged-in user.

**Incorrect (wildcard CORS allows any origin):**

```typescript
Deno.serve(async (req) => {
  return new Response(JSON.stringify({ data: 'sensitive' }), {
    headers: {
      'Content-Type': 'application/json',
      'Access-Control-Allow-Origin': '*',  // any website can call this
    },
  })
})
```

**Correct (restrict to your domains):**

```typescript
const ALLOWED_ORIGINS = [
  'https://yourapp.com',
  'https://staging.yourapp.com',
  ...(Deno.env.get('ENVIRONMENT') === 'development'
    ? ['http://localhost:3000', 'http://localhost:8081']
    : []),
]

function corsHeaders(req: Request) {
  const origin = req.headers.get('Origin') ?? ''
  const allowedOrigin = ALLOWED_ORIGINS.includes(origin) ? origin : ''

  return {
    'Access-Control-Allow-Origin': allowedOrigin,
    'Access-Control-Allow-Headers': 'authorization, x-client-info, apikey, content-type',
    'Access-Control-Allow-Methods': 'POST, OPTIONS',
  }
}

Deno.serve(async (req) => {
  // Handle preflight
  if (req.method === 'OPTIONS') {
    return new Response('ok', { headers: corsHeaders(req) })
  }

  // Your function logic...
  return new Response(JSON.stringify({ data: 'sensitive' }), {
    headers: {
      'Content-Type': 'application/json',
      ...corsHeaders(req),
    },
  })
})
```

**Note:** For mobile apps (Expo/React Native), CORS is not enforced — it's a browser-only mechanism. Mobile apps call edge functions directly without CORS restrictions.

Reference: https://supabase.com/docs/guides/functions/cors
