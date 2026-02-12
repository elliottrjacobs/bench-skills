---
name: security-supabase
description: Supabase security best practices and common vulnerability patterns. Use this skill when writing RLS policies, configuring Supabase Auth, reviewing storage bucket policies, securing realtime subscriptions, or auditing Supabase projects for security issues.
license: MIT
metadata:
  author: elliottjacobs
  version: "1.0.0"
  organization: bench-skills
---

# Supabase Security Best Practices

Comprehensive security guide for Supabase projects. Contains rules across 6 categories, prioritized by impact to guide secure configuration, RLS policy design, and authentication patterns.

## When to Apply

Reference these guidelines when:
- Writing or reviewing RLS policies
- Configuring Supabase Auth (OAuth, email, sessions)
- Setting up storage bucket policies
- Securing realtime channel subscriptions
- Writing or reviewing edge functions
- Auditing a Supabase project before launch
- Reviewing API exposure and anon key usage

## Rule Categories by Priority

| Priority | Category | Impact | Prefix |
|----------|----------|--------|--------|
| 1 | Row Level Security | CRITICAL | `rls-` |
| 2 | Authentication | CRITICAL | `auth-` |
| 3 | API Exposure | HIGH | `api-` |
| 4 | Storage Security | HIGH | `storage-` |
| 5 | Realtime Security | MEDIUM | `realtime-` |
| 6 | Edge Functions | MEDIUM | `edge-` |

## How to Use

Read individual rule files for detailed explanations and code examples:

```
references/rls-enable-all-tables.md
references/auth-pkce-flow.md
references/api-anon-key-scope.md
```

Each rule file contains:
- Brief explanation of why it matters
- Incorrect code example with explanation
- Correct code example with explanation
- Supabase-specific context and gotchas
- Additional references

## References

- https://supabase.com/docs/guides/auth
- https://supabase.com/docs/guides/auth/row-level-security
- https://supabase.com/docs/guides/storage
- https://supabase.com/docs/guides/realtime
- https://supabase.com/docs/guides/functions
