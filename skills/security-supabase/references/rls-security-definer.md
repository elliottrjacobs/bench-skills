---
title: Avoid SECURITY DEFINER Bypasses
impact: CRITICAL
impactDescription: SECURITY DEFINER functions bypass RLS entirely, creating hidden backdoors
tags: rls, security-definer, functions, bypass, escalation
---

## Avoid SECURITY DEFINER Bypasses

Functions marked `SECURITY DEFINER` execute with the permissions of the function owner (typically the `postgres` superuser), bypassing all RLS policies. This is the most common way RLS gets accidentally circumvented.

**Incorrect (function bypasses RLS):**

```sql
-- This function runs as the postgres superuser
-- It can read ALL rows regardless of RLS policies
create or replace function public.get_all_user_emails()
returns setof text
language sql
security definer
as $$
  select email from public.profiles;
$$;

-- Any authenticated user can now call this and get every email
-- even if the RLS policy restricts profiles to own data only
```

**Correct (function respects RLS):**

```sql
-- SECURITY INVOKER (the default) runs with the calling user's permissions
-- RLS policies are enforced normally
create or replace function public.get_user_emails()
returns setof text
language sql
security invoker  -- explicit, though this is the default
as $$
  select email from public.profiles;
$$;
```

**When SECURITY DEFINER is actually needed:**

Use it only for operations that genuinely require elevated privileges, and always restrict access:

```sql
-- Example: let users claim a username (needs to check uniqueness across all rows)
create or replace function public.claim_username(desired_username text)
returns boolean
language plpgsql
security definer
set search_path = ''  -- prevent search_path injection
as $$
begin
  -- Only modify the calling user's own row
  update public.profiles
  set username = desired_username
  where id = auth.uid()
    and not exists (
      select 1 from public.profiles where username = desired_username
    );
  return found;
end;
$$;

-- Restrict who can call it
revoke execute on function public.claim_username from public;
grant execute on function public.claim_username to authenticated;
```

**Audit query — find SECURITY DEFINER functions in public schema:**

```sql
select proname, prosecdef
from pg_proc
join pg_namespace on pg_proc.pronamespace = pg_namespace.oid
where nspname = 'public' and prosecdef = true;
```

Reference: https://supabase.com/docs/guides/database/functions#security-definer-vs-invoker
