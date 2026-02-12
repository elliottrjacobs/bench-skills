---
title: Never Expose the Service Role Key Client-Side
impact: CRITICAL
impactDescription: Service role key bypasses all RLS — full database access if leaked
tags: rls, service-role, anon-key, environment-variables, client-side
---

## Never Expose the Service Role Key Client-Side

Supabase provides two keys: `anon` (public, respects RLS) and `service_role` (secret, bypasses all RLS). The service role key must never appear in client-side code, environment variables prefixed with `NEXT_PUBLIC_` or `EXPO_PUBLIC_`, or any code shipped to the browser/device.

**Incorrect (service role key exposed to client):**

```typescript
// .env.local — WRONG: NEXT_PUBLIC_ prefix exposes to browser
NEXT_PUBLIC_SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOiJIUzI1NiIs...

// app/page.tsx — WRONG: using service role in client component
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.NEXT_PUBLIC_SUPABASE_SERVICE_ROLE_KEY!  // bypasses ALL RLS
)
```

**Correct (service role only in server-side code):**

```typescript
// .env.local — service role key without public prefix
SUPABASE_SERVICE_ROLE_KEY=eyJhbGciOiJIUzI1NiIs...

// lib/supabase-admin.ts — server-only admin client
import { createClient } from '@supabase/supabase-js'

// This file should only be imported in server components,
// API routes, or server actions
export const supabaseAdmin = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.SUPABASE_SERVICE_ROLE_KEY!,
  { auth: { autoRefreshToken: false, persistSession: false } }
)
```

**For Expo/React Native:**

```typescript
// NEVER ship the service role key in mobile apps
// Mobile apps should ONLY use the anon key
// All admin operations go through your own API/edge functions

// Correct: anon key only
const supabase = createClient(
  process.env.EXPO_PUBLIC_SUPABASE_URL!,
  process.env.EXPO_PUBLIC_SUPABASE_ANON_KEY!  // respects RLS
)
```

**Audit checklist:**
- Search codebase for `service_role` — should only appear in server-side files
- Check `.env*` files — no `NEXT_PUBLIC_*SERVICE*` or `EXPO_PUBLIC_*SERVICE*` variables
- Check CI/CD — service role key should be in server environment only

Reference: https://supabase.com/docs/guides/api/api-keys
