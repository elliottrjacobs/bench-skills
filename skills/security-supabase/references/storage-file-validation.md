---
title: Validate File Types and Sizes
impact: HIGH
impactDescription: Without validation, users can upload malicious files or exhaust storage
tags: storage, validation, file-types, mime-types, size-limits, uploads
---

## Validate File Types and Sizes

Without restrictions, users can upload executable files, oversized files, or files disguised with wrong extensions. Validate both client-side (UX) and server-side (security).

**Incorrect (no file validation):**

```typescript
// Accepts any file type and size
const { data, error } = await supabase.storage
  .from('uploads')
  .upload(`${userId}/${file.name}`, file)

// Users can upload .exe, .sh, 500MB files, etc.
```

**Correct (bucket-level + client-level validation):**

```sql
-- Bucket configuration with allowed MIME types and size limit
update storage.buckets
set
  allowed_mime_types = array['image/jpeg', 'image/png', 'image/webp'],
  file_size_limit = 5242880  -- 5MB in bytes
where id = 'avatars';
```

```typescript
// Client-side validation (better UX — fail fast)
const MAX_SIZE = 5 * 1024 * 1024 // 5MB
const ALLOWED_TYPES = ['image/jpeg', 'image/png', 'image/webp']

function validateFile(file: File): string | null {
  if (!ALLOWED_TYPES.includes(file.type)) {
    return 'Only JPEG, PNG, and WebP images are allowed'
  }
  if (file.size > MAX_SIZE) {
    return 'File must be under 5MB'
  }
  return null
}

// Upload with validation
const error = validateFile(file)
if (error) {
  // Show error to user
  return
}

const { data, error: uploadError } = await supabase.storage
  .from('avatars')
  .upload(`${userId}/avatar.${ext}`, file, {
    contentType: file.type,
    upsert: true,
  })
```

**Important:** Client-side validation is for UX only. The bucket-level `allowed_mime_types` and `file_size_limit` are the actual security boundary — they're enforced server-side by Supabase.

Reference: https://supabase.com/docs/guides/storage/uploads/standard-uploads
