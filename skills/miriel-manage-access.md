---
name: Manage users and document access
description: Provision users, set per-document access grants, and manage user policies in Miriel.
api: openapi/miriel-openapi.yml
operations: [createUser, setDocumentAccess, getAllDocuments, addUserPolicy]
---

# Manage users and document access (Miriel)

Miriel manages permissions at the token level — each user has an ID and documents
are shared through grant IDs and policies.

## Auth
`POST` to `https://api.prod.miriel.ai/api/v2/*` with the API key in the
`x-access-token` header.

## Steps
1. **Create a user** — call `createUser` to provision an end-user identity.
2. **List their documents** — call `getAllDocuments` with the `user_id` (optionally
   filtered by `project` or `metadata_query`).
3. **Grant access** — call `setDocumentAccess` with `user_id`, `document_id`, and
   the `grant_ids` array that should be able to see the document.
4. **Apply a policy** — call `addUserPolicy` with a `policy` object (and optional
   `project_id`) to enforce broader access rules.

## Notes
- Removing access is the inverse: `deleteUserPolicy` and re-`setDocumentAccess`
  with a reduced `grant_ids` set.
- Every interaction is encrypted; treat grant IDs as sensitive.
