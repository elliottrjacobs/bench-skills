---
title: Create Separate Policies for Each Operation
impact: CRITICAL
impactDescription: Overly broad policies grant unintended write or delete access
tags: rls, policies, select, insert, update, delete, operations
---

## Create Separate Policies for Each Operation

A single `for all` policy applies the same logic to SELECT, INSERT, UPDATE, and DELETE. This often grants more access than intended — read access logic rarely matches write access logic.

**Incorrect (single policy for all operations):**

```sql
-- This lets any authenticated user INSERT, UPDATE, and DELETE
-- any row where they can read it
create policy "Users can access own data"
  on public.posts for all
  using (auth.uid() = user_id);
```

**Correct (separate policies per operation):**

```sql
-- SELECT: users can read their own posts
create policy "Users can read own posts"
  on public.posts for select
  using (auth.uid() = user_id);

-- INSERT: users can only create posts as themselves
create policy "Users can create own posts"
  on public.posts for insert
  with check (auth.uid() = user_id);

-- UPDATE: users can only edit their own posts
create policy "Users can update own posts"
  on public.posts for update
  using (auth.uid() = user_id)
  with check (auth.uid() = user_id);

-- DELETE: users can only delete their own posts
create policy "Users can delete own posts"
  on public.posts for delete
  using (auth.uid() = user_id);
```

Note the distinction: `using` filters which existing rows the operation applies to. `with check` validates the new row data on INSERT and UPDATE. Both are needed on UPDATE to prevent a user from reassigning a post to another user.

Reference: https://supabase.com/docs/guides/auth/row-level-security#policies
