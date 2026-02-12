---
title: Handle Session Refresh and Expiry Properly
impact: CRITICAL
impactDescription: Stale sessions cause silent auth failures; improper refresh creates race conditions
tags: auth, sessions, refresh-token, expiry, onAuthStateChange
---

## Handle Session Refresh and Expiry Properly

Supabase JWT access tokens expire (default: 1 hour). If you don't handle refresh properly, users get silent 401 errors. If you set up multiple listeners or refresh in the wrong place, you get race conditions.

**Incorrect (no session refresh handling):**

```typescript
// Creating client without listening for auth changes
// Session expires after 1 hour — all subsequent API calls fail silently
const supabase = createClient(url, anonKey)

// User logs in...
// 1 hour later, supabase.from('posts').select() returns empty or errors
// No indication to user that session expired
```

**Incorrect (multiple auth listeners causing race conditions):**

```typescript
// Layout.tsx
useEffect(() => {
  supabase.auth.onAuthStateChange((event, session) => { /* refresh */ })
}, [])

// AuthProvider.tsx
useEffect(() => {
  supabase.auth.onAuthStateChange((event, session) => { /* also refresh */ })
}, [])

// Two listeners competing to refresh the same token = race condition
```

**Correct (single auth listener at app root):**

```typescript
// Single Supabase client instance (singleton)
// lib/supabase.ts
import { createClient } from '@supabase/supabase-js'

export const supabase = createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
)

// Single auth listener at app root
// app/providers.tsx or App.tsx
useEffect(() => {
  const { data: { subscription } } = supabase.auth.onAuthStateChange(
    (event, session) => {
      if (event === 'TOKEN_REFRESHED') {
        // Session refreshed automatically — update app state if needed
      }
      if (event === 'SIGNED_OUT') {
        // Clear local state, redirect to login
      }
    }
  )

  return () => subscription.unsubscribe()
}, [])
```

**For Next.js with SSR (middleware-based refresh):**

```typescript
// middleware.ts — refresh session on every request
import { createServerClient } from '@supabase/ssr'
import { NextResponse } from 'next/server'

export async function middleware(request: NextRequest) {
  const response = NextResponse.next()
  const supabase = createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    { cookies: { /* cookie handlers */ } }
  )

  // This refreshes the session if expired
  await supabase.auth.getUser()

  return response
}
```

Reference: https://supabase.com/docs/guides/auth/server-side/creating-a-client
