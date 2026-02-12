---
title: Use Custom Claims for Role-Based Access
impact: HIGH
impactDescription: Storing roles in a regular table and checking via subquery is slow and bypassable
tags: auth, claims, roles, jwt, rbac, permissions
---

## Use Custom Claims for Role-Based Access

Checking user roles via a subquery to a `profiles` table in every RLS policy is slow (extra query per row) and fragile. Custom JWT claims embed the role directly in the token, making RLS checks fast and reliable.

**Incorrect (role check via subquery in every policy):**

```sql
-- Every RLS policy does a subquery to profiles
create policy "Admins can manage users"
  on public.users for all
  using (
    exists (
      select 1 from public.profiles
      where id = auth.uid() and role = 'admin'
    )
  );

-- Problems:
-- 1. Extra query on every single row check
-- 2. If profiles table RLS is misconfigured, role check breaks
-- 3. Role changes require re-login to take effect anyway
```

**Correct (custom claims in JWT):**

```sql
-- Step 1: Create a function to set custom claims (run once)
create or replace function public.set_claim(
  uid uuid,
  claim text,
  value jsonb
)
returns text
language plpgsql
security definer
set search_path = ''
as $$
begin
  update auth.users
  set raw_app_meta_data = raw_app_meta_data || json_build_object(claim, value)::jsonb
  where id = uid;
  return 'OK';
end;
$$;

-- Restrict to service role only
revoke execute on function public.set_claim from public;
revoke execute on function public.set_claim from authenticated;

-- Step 2: Set a user's role
select public.set_claim(
  'user-uuid-here',
  'user_role',
  '"admin"'::jsonb
);

-- Step 3: Use in RLS policies — fast, no subquery
create policy "Admins can manage users"
  on public.users for all
  using (
    (auth.jwt() -> 'app_metadata' ->> 'user_role') = 'admin'
  );

-- Step 4: Check in application code
const { data: { user } } = await supabase.auth.getUser()
const role = user?.app_metadata?.user_role
```

**Important:** After setting a claim, the user must refresh their session to get the updated JWT. Call `supabase.auth.refreshSession()` or wait for the next automatic refresh.

Reference: https://supabase.com/docs/guides/auth/custom-claims-and-role-based-access-control-rbac
