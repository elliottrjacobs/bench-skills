---
title: Enforce Email Confirmation Before Access
impact: CRITICAL
impactDescription: Without confirmation, anyone can sign up with any email and access data
tags: auth, email, confirmation, verification, signup
---

## Enforce Email Confirmation Before Access

If email confirmation is disabled, anyone can sign up with any email address (including someone else's) and immediately access the app. This also enables account enumeration and spam signups.

**Incorrect (no email confirmation, RLS trusts auth.uid() blindly):**

```sql
-- Dashboard: Authentication > Settings > "Enable email confirmations" is OFF

-- RLS policy trusts any authenticated user
create policy "Authenticated users can read"
  on public.workspaces for select
  using (auth.role() = 'authenticated');

-- Problem: user signs up with victim@company.com (no confirmation required)
-- They're now "authenticated" and can access workspace data
```

**Correct (email confirmation enabled + RLS checks confirmation status):**

```sql
-- Dashboard: Authentication > Settings > "Enable email confirmations" is ON

-- For extra safety, check email confirmation in RLS
create policy "Confirmed users can read"
  on public.workspaces for select
  using (
    auth.uid() is not null
    and (auth.jwt() ->> 'email_confirmed_at') is not null
  );
```

**Application-level check (in addition to RLS):**

```typescript
// After login, check if email is confirmed
const { data: { user } } = await supabase.auth.getUser()

if (user && !user.email_confirmed_at) {
  // Redirect to "check your email" page
  // Don't grant access to the app
  redirect('/confirm-email')
}
```

**Supabase Dashboard settings:**
- Authentication > Settings > Enable email confirmations: **ON**
- Authentication > Email Templates > Customize confirmation email
- Consider setting `MAILER_AUTOCONFIRM=false` in self-hosted setups

Reference: https://supabase.com/docs/guides/auth/auth-email
