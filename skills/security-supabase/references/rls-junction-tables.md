---
title: Protect Junction and Join Tables
impact: CRITICAL
impactDescription: Unprotected junction tables allow users to grant themselves access to any resource
tags: rls, junction-tables, many-to-many, membership, access-control
---

## Protect Junction and Join Tables

Junction tables (many-to-many relationships like `team_members`, `project_collaborators`) are frequently left without RLS. This lets any user insert themselves into any team or project.

**Incorrect (junction table without RLS):**

```sql
create table public.team_members (
  team_id uuid references public.teams,
  user_id uuid references auth.users,
  role text default 'member',
  primary key (team_id, user_id)
);

-- No RLS! Any user can:
-- INSERT into team_members to join any team
-- UPDATE their role to 'admin'
-- DELETE other members from teams
```

**Correct (junction table with RLS protecting membership):**

```sql
create table public.team_members (
  team_id uuid references public.teams,
  user_id uuid references auth.users,
  role text default 'member',
  primary key (team_id, user_id)
);

alter table public.team_members enable row level security;

-- Members can see who's on their teams
create policy "Members can view team membership"
  on public.team_members for select
  using (
    team_id in (
      select team_id from public.team_members
      where user_id = auth.uid()
    )
  );

-- Only admins can add members
create policy "Admins can add members"
  on public.team_members for insert
  with check (
    exists (
      select 1 from public.team_members
      where team_id = team_members.team_id
        and user_id = auth.uid()
        and role = 'admin'
    )
  );

-- Only admins can remove members (but not themselves)
create policy "Admins can remove members"
  on public.team_members for delete
  using (
    exists (
      select 1 from public.team_members
      where team_id = team_members.team_id
        and user_id = auth.uid()
        and role = 'admin'
    )
    and user_id != auth.uid()  -- prevent self-removal
  );
```

**Common junction tables to audit:** `team_members`, `project_collaborators`, `workspace_users`, `channel_members`, `org_members`, `role_assignments`.

Reference: https://supabase.com/docs/guides/auth/row-level-security
