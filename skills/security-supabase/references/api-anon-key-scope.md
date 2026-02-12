---
title: Restrict What the Anon Key Can Access
impact: HIGH
impactDescription: Overly permissive anon access exposes data to unauthenticated users
tags: api, anon-key, public-access, unauthenticated, roles
---

## Restrict What the Anon Key Can Access

The `anon` key maps to the `anon` Postgres role. RLS policies using `auth.uid() is not null` block anon access, but policies using `true` or no restrictive clause expose data to anyone with the anon key (which is public).

**Incorrect (policy allows anon access to sensitive data):**

```sql
-- This policy lets ANYONE read all posts, including unauthenticated users
create policy "Anyone can read posts"
  on public.posts for select
  using (true);

-- Even worse: this lets anon users read user profiles
create policy "Public profiles"
  on public.profiles for select
  using (true);  -- email, full_name exposed to the world
```

**Correct (restrict anon access, require authentication for sensitive data):**

```sql
-- Public content: allow anon read, but only for published items
create policy "Anyone can read published posts"
  on public.posts for select
  using (status = 'published' and visibility = 'public');

-- Authenticated-only: require login for profiles
create policy "Authenticated users can read profiles"
  on public.profiles for select
  using (auth.role() = 'authenticated');

-- Sensitive data: restrict to own data only
create policy "Users can read own settings"
  on public.user_settings for select
  using (auth.uid() = user_id);
```

**Column-level protection (don't return sensitive columns):**

```sql
-- Instead of exposing the full profiles table, create a view
create view public.public_profiles as
  select id, display_name, avatar_url
  from public.profiles;

-- Apply RLS to the original table, grant anon access to the view
grant select on public.public_profiles to anon;
```

**Audit question:** For every table with `using (true)` policies, ask: "Would I be comfortable if anyone on the internet could read every row in this table?"

Reference: https://supabase.com/docs/guides/api/api-keys
