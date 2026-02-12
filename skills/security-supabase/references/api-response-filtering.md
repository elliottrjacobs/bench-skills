---
title: Don't Return Sensitive Columns via API
impact: HIGH
impactDescription: Client can select any column on any table exposed through the API
tags: api, columns, select, views, data-exposure, pii
---

## Don't Return Sensitive Columns via API

The Supabase REST API allows clients to select any column: `?select=email,password_hash,ssn`. Even with RLS restricting rows, all columns on accessible rows are readable. Use views or column-level grants to hide sensitive fields.

**Incorrect (sensitive columns accessible via API):**

```sql
create table public.profiles (
  id uuid primary key,
  email text,
  full_name text,
  phone text,
  stripe_customer_id text,  -- sensitive
  internal_notes text,       -- sensitive
  created_at timestamptz
);

-- Even with RLS restricting to own row, the user can query:
-- GET /rest/v1/profiles?select=stripe_customer_id,internal_notes
```

**Correct (use a view to expose only safe columns):**

```sql
-- Keep the full table for server-side use
create table public.profiles (
  id uuid primary key,
  email text,
  full_name text,
  phone text,
  stripe_customer_id text,
  internal_notes text,
  created_at timestamptz
);

-- Create a view with only client-safe columns
create view public.user_profiles as
  select id, email, full_name, avatar_url, created_at
  from public.profiles;

-- RLS on the base table
alter table public.profiles enable row level security;
create policy "Users read own profile"
  on public.profiles for select
  using (auth.uid() = id);
```

**Alternative: column-level grants (more granular):**

```sql
-- Revoke default access
revoke all on public.profiles from anon, authenticated;

-- Grant only specific columns
grant select (id, email, full_name, avatar_url, created_at)
  on public.profiles to authenticated;

-- Stripe ID only accessible server-side via service role
```

Reference: https://supabase.com/docs/guides/api/securing-your-api
