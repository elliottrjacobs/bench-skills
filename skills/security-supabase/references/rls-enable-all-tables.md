---
title: Enable RLS on Every Public Table
impact: CRITICAL
impactDescription: Without RLS, any user with the anon key can read/write all data
tags: rls, tables, public-schema, enable
---

## Enable RLS on Every Public Table

Every table in the `public` schema is accessible via the Supabase API by default. Without RLS enabled, anyone with the anon key has full read/write access to that table.

**Incorrect (table without RLS — fully exposed via API):**

```sql
create table public.profiles (
  id uuid references auth.users primary key,
  email text,
  full_name text,
  role text
);

-- No RLS enabled. Any client with the anon key can:
-- SELECT * FROM profiles (read all user data)
-- DELETE FROM profiles (delete all profiles)
```

**Correct (RLS enabled with explicit policies):**

```sql
create table public.profiles (
  id uuid references auth.users primary key,
  email text,
  full_name text,
  role text
);

alter table public.profiles enable row level security;

-- Users can only read their own profile
create policy "Users can read own profile"
  on public.profiles for select
  using (auth.uid() = id);

-- Users can only update their own profile
create policy "Users can update own profile"
  on public.profiles for update
  using (auth.uid() = id);
```

**Audit query — find tables without RLS:**

```sql
select schemaname, tablename, rowsecurity
from pg_tables
where schemaname = 'public'
  and rowsecurity = false;
```

If this query returns any rows, those tables are fully exposed to the API.

Reference: https://supabase.com/docs/guides/auth/row-level-security
