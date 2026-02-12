---
title: Use PKCE Flow for Client-Side Authentication
impact: CRITICAL
impactDescription: Implicit flow exposes tokens in URLs, vulnerable to interception
tags: auth, pkce, oauth, implicit-flow, tokens
---

## Use PKCE Flow for Client-Side Authentication

The PKCE (Proof Key for Code Exchange) flow is the recommended OAuth flow for all client-side applications (SPAs, mobile apps). The older implicit flow passes tokens in URL fragments, which can be logged, cached, or intercepted.

**Incorrect (implicit flow — tokens in URL):**

```typescript
// Old pattern: tokens appear in URL hash after redirect
// https://yourapp.com/callback#access_token=eyJ...&refresh_token=...
// These can be logged by analytics, browser extensions, or server logs

const { data, error } = await supabase.auth.signInWithOAuth({
  provider: 'google',
  options: {
    redirectTo: 'https://yourapp.com/callback',
    // No flowType specified — defaults vary by supabase-js version
  },
})
```

**Correct (PKCE flow — tokens exchanged server-side):**

```typescript
// Step 1: Initiate PKCE flow
const { data, error } = await supabase.auth.signInWithOAuth({
  provider: 'google',
  options: {
    redirectTo: 'https://yourapp.com/auth/callback',
    queryParams: {
      access_type: 'offline',
      prompt: 'consent',
    },
  },
})

// Step 2: Handle callback — exchange code for session
// app/auth/callback/route.ts (Next.js) or equivalent
import { NextResponse } from 'next/server'
import { createServerClient } from '@supabase/ssr'

export async function GET(request: Request) {
  const { searchParams } = new URL(request.url)
  const code = searchParams.get('code')

  if (code) {
    const supabase = createServerClient(/* ... */)
    await supabase.auth.exchangeCodeForSession(code)
  }

  return NextResponse.redirect(new URL('/', request.url))
}
```

**For Expo/React Native:**

```typescript
import { makeRedirectUri } from 'expo-auth-session'
import * as WebBrowser from 'expo-web-browser'

const redirectTo = makeRedirectUri()

const { data, error } = await supabase.auth.signInWithOAuth({
  provider: 'google',
  options: { redirectTo },
})

// Open browser for auth, handle deep link callback
if (data.url) {
  const result = await WebBrowser.openAuthSessionAsync(data.url, redirectTo)
  if (result.type === 'success') {
    const url = new URL(result.url)
    const code = url.searchParams.get('code')
    if (code) {
      await supabase.auth.exchangeCodeForSession(code)
    }
  }
}
```

Reference: https://supabase.com/docs/guides/auth/server-side/oauth-with-pkce-flow-for-ssr
