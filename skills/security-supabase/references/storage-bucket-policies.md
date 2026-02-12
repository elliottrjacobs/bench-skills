---
title: Set RLS Policies on Storage Buckets
impact: HIGH
impactDescription: Without storage policies, any user can read/upload/delete files in public buckets
tags: storage, buckets, rls, policies, uploads, files
---

## Set RLS Policies on Storage Buckets

Supabase Storage uses RLS on the `storage.objects` table. A "public" bucket means files are accessible via public URL — but upload/delete permissions are still controlled by RLS policies. Without policies, authenticated users can upload or delete any file.

**Incorrect (bucket with no restrictive policies):**

```sql
-- Creating a public bucket without upload restrictions
insert into storage.buckets (id, name, public)
values ('avatars', 'avatars', true);

-- No policies = authenticated users can:
-- Upload files to any path (including other users' directories)
-- Delete any file in the bucket
-- Overwrite existing files
```

**Correct (bucket with path-scoped policies):**

```sql
insert into storage.buckets (id, name, public)
values ('avatars', 'avatars', true);

-- Users can only upload to their own folder
create policy "Users upload to own folder"
  on storage.objects for insert
  with check (
    bucket_id = 'avatars'
    and (storage.foldername(name))[1] = auth.uid()::text
  );

-- Users can only update their own files
create policy "Users update own files"
  on storage.objects for update
  using (
    bucket_id = 'avatars'
    and (storage.foldername(name))[1] = auth.uid()::text
  );

-- Users can only delete their own files
create policy "Users delete own files"
  on storage.objects for delete
  using (
    bucket_id = 'avatars'
    and (storage.foldername(name))[1] = auth.uid()::text
  );

-- Public read for avatar bucket (since bucket is public)
create policy "Public read for avatars"
  on storage.objects for select
  using (bucket_id = 'avatars');
```

**File path convention:** Use `{user_id}/{filename}` paths so RLS can scope access by folder.

```typescript
// Client upload — path includes user ID
const { data, error } = await supabase.storage
  .from('avatars')
  .upload(`${user.id}/avatar.png`, file)
```

**Private buckets** (public = false) require signed URLs for read access:

```typescript
// Server-side: generate signed URL (expires)
const { data } = await supabaseAdmin.storage
  .from('documents')
  .createSignedUrl('user-id/contract.pdf', 3600) // 1 hour
```

Reference: https://supabase.com/docs/guides/storage/security/access-control
