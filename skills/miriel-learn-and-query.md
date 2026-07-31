---
name: Learn a source and query context
description: Ingest a data source into the Miriel context engine, then retrieve relevant context for a query.
api: openapi/miriel-openapi.yml
operations: [learn, getLearningJobs, query]
---

# Learn a source and query context (Miriel)

Use the Miriel Context Engine to teach an app about a data source, then ask for
relevant context at inference time.

## Auth
All calls are `POST` to `https://api.prod.miriel.ai/api/v2/*` with the API key in
the `x-access-token` header (issued from the Miriel dashboard).

## Steps
1. **Ingest** — call `learn` with `input` (a string or a file/URL source) plus
   `discoverable`, `grant_ids`, and `recursion_depth`. This returns a learning job.
2. **Wait** — poll `getLearningJobs` until the job for your document is complete
   (the SDK exposes `wait_for_complete` / `polling_interval` helpers).
3. **Query** — call `query` with your `query` string and the retrieval flags you
   want (`want_llm`, `want_vector`, `want_graph`) plus optional `user_id` and
   `metadata_query`. Miriel returns the relevant context.

## Notes
- Scope reads and writes with `user_id`; share documents via `grant_ids`.
- There is no idempotency key — do not blindly retry `learn` on timeout; check
  `getLearningJobs` first to avoid duplicate ingestion.
