---
title: Secure Database Functions Exposed via API
impact: HIGH
impactDescription: Public functions are callable by anyone with the anon key unless explicitly restricted
tags: api, functions, rpc, grants, permissions, search-path
---

## Secure Database Functions Exposed via API

Every function in the `public` schema is callable via the Supabase REST API (`/rest/v1/rpc/function_name`). By default, functions are executable by all roles including `anon`. You must explicitly restrict access.

**Incorrect (function callable by anyone):**

```sql
create or replace function public.delete_user_data(target_user_id uuid)
returns void
language plpgsql
as $$
begin
  delete from public.profiles where id = target_user_id;
  delete from public.posts where user_id = target_user_id;
end;
$$;

-- By default, anon and authenticated roles can call this
-- Anyone can delete anyone's data via: POST /rest/v1/rpc/delete_user_data
```

**Correct (restrict function access with grants):**

```sql
create or replace function public.delete_my_data()
returns void
language plpgsql
security invoker  -- respects RLS
set search_path = ''  -- prevent search_path injection
as $$
begin
  -- Only deletes the calling user's data (auth.uid() from RLS)
  delete from public.profiles where id = auth.uid();
  delete from public.posts where user_id = auth.uid();
end;
$$;

-- Remove default public access
revoke execute on function public.delete_my_data from public;
revoke execute on function public.delete_my_data from anon;

-- Only authenticated users can call it
grant execute on function public.delete_my_data to authenticated;
```

**Always set `search_path`** on any function to prevent injection:

```sql
-- Without search_path restriction, a malicious schema could shadow
-- your tables and intercept queries
create function public.my_function()
returns void
language plpgsql
set search_path = ''  -- forces fully qualified names
as $$
begin
  -- Must use public.profiles instead of just profiles
  select * from public.profiles;
end;
$$;
```

Reference: https://supabase.com/docs/guides/database/functions
