# Tech Spec: [Feature Name]

**Date:** YYYY-MM-DD
**PRD:** [link to PRD]
**Status:** Draft / In Review / Approved

## Overview

[1-2 sentences: what this spec covers and its relationship to the PRD]

## Architecture

### System Design

[High-level architecture description. How components interact, data flows, and where this feature fits in the existing system.]

### Component Hierarchy

```
[Component tree showing parent-child relationships and state ownership]

PageComponent (server component)
  ├── DataProvider (server component, fetches data)
  │   ├── FeatureList (client component, manages selection state)
  │   │   └── FeatureItem (client component)
  │   └── FeatureDetail (client component)
  └── ActionBar (client component)
```

## Data Models

### Database Schema

```sql
-- New tables or modifications
create table public.[table_name] (
  id uuid primary key default gen_random_uuid(),
  -- columns
  created_at timestamptz default now(),
  updated_at timestamptz default now()
);

-- RLS policies
alter table public.[table_name] enable row level security;

create policy "[description]"
  on public.[table_name] for [operation]
  using ([condition]);
```

### TypeScript Types

```typescript
// Derived from database schema or API contracts
type FeatureName = {
  id: string
  // fields
}
```

## API Design

### Endpoints / Server Actions

| Method | Path / Action | Description | Auth |
|--------|--------------|-------------|------|
| GET | `/api/[resource]` | [description] | Required |
| POST | `createResource()` | [description] | Required |

### Request/Response Shapes

```typescript
// Example request/response for key endpoints
```

## Security Considerations

- [ ] RLS policies cover all CRUD operations
- [ ] Input validation with Zod schemas
- [ ] Auth checks at API/action boundary
- [ ] No sensitive data exposed to client
- [ ] [Feature-specific security consideration]

## Migration Plan

### Steps
1. [Database migration]
2. [API/action implementation]
3. [UI implementation]
4. [Data migration if needed]

### Rollback
[How to revert if issues are found]

## Risks & Open Questions

| Item | Type | Impact | Status |
|------|------|--------|--------|
| [item] | Risk / Question | High/Med/Low | Open / Resolved |
