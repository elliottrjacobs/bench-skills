# Solution Document Template

Use this template when writing solution documents for `docs/solutions/`.

## Template

```markdown
---
title: "[Clear, searchable title]"
date: YYYY-MM-DD
category: [one of: auth, database, api, ui, state, navigation, performance, testing, deployment, typescript, tooling, architecture, mobile, general]
tags: [2-8 relevant tags]
difficulty: [low|medium|high]
applicability: [specific|moderate|broad]
---

# [Title]

## Problem

[What went wrong or what needed to be decided. Include error messages, symptoms, or the triggering context.]

## Root Cause

[Why it happened. The underlying issue, not just the symptom.]

## Solution

[What fixed it or what was decided.]

### Key Changes

[Specific code changes, configuration changes, or architectural decisions. Include file paths and relevant code snippets.]

## What Didn't Work

[Approaches tried that failed. This saves future-you from repeating dead ends.]

## Prevention

[How to avoid this in the future. Could be a lint rule, a pattern to follow, a check to add.]

## Related

[Links to related docs, PRDs, tech specs, or other solution docs.]
```

## Frontmatter Field Guide

### category
Choose the most specific category that matches the root cause:

| Category | When to Use |
|----------|------------|
| `auth` | Authentication, authorization, sessions, tokens, RLS |
| `database` | Schema, queries, migrations, indexes, data modeling |
| `api` | API routes, server actions, endpoints, request/response |
| `ui` | Components, styling, layout, responsive design |
| `state` | React Query, Zustand, form state, URL state |
| `navigation` | Routing, deep links, navigation patterns |
| `performance` | Speed, bundle size, rendering, caching |
| `testing` | Tests, mocking, test infrastructure |
| `deployment` | CI/CD, builds, environments, hosting |
| `typescript` | Types, generics, Zod, type errors |
| `tooling` | Dev tools, linting, IDE configuration |
| `architecture` | System design, patterns, module structure |
| `mobile` | Platform-specific, native modules, mobile UX |
| `general` | Doesn't fit elsewhere |

### tags
- Minimum 2, maximum 8
- Use specific technology names: `supabase`, `nextjs`, `expo`, `react-query`
- Use problem domain terms: `n-plus-one`, `race-condition`, `hydration`
- Use pattern names: `optimistic-update`, `server-action`, `rls-policy`

### difficulty
- `low` — Quick fix, straightforward once you know the answer
- `medium` — Required investigation or understanding of multiple systems
- `high` — Deep debugging, architectural implications, or multi-day effort

### applicability
- `specific` — Only applies to this exact scenario
- `moderate` — Applies to similar problems in this tech stack
- `broad` — General principle applicable across projects
