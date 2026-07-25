---
name: Triage validated exposures with CyCognito
description: Retrieve, investigate, comment on, and snooze/archive validated security issues (exposures) surfaced by the CyCognito platform.
api: openapi/cycognito-v1-openapi-original.json
operations:
- POST /v1/issues
- GET /v1/issues/issue/{issue_instance_id}
- PUT /v1/issues/issue/{issue_instance_id}/investigation-status
- PUT /v1/issues/actions/add/comment
- PUT /v1/issues/actions/{action}/tags
- PUT /v1/issues/actions/snooze
- PUT /v1/issues/actions/archive
---

# Triage validated exposures with CyCognito

Use the CyCognito API V1 to work a queue of validated exposures ("issues").

## Auth
All requests are API-key authenticated. Send your key in the `Authorization` header. Base URL: `https://api.platform.cycognito.com`.

## Steps
1. **List issues** — `POST /v1/issues`. Page with `count` and `offset`; narrow with the free-text `q` param or an `advanced-search` filter; pick returned attributes with `fields`; order with `sort-by` / `sort-order`.
2. **Inspect one issue** — `GET /v1/issues/issue/{issue_instance_id}` using an `issue_instance_id` from step 1.
3. **Set investigation status** — `PUT /v1/issues/issue/{issue_instance_id}/investigation-status` to move it through your workflow (e.g. investigating / remediated).
4. **Comment** — `PUT /v1/issues/actions/add/comment` to record context for the team.
5. **Tag** — `PUT /v1/issues/actions/{action}/tags` (action = add/remove) to label issues for routing.
6. **Snooze or archive** — `PUT /v1/issues/actions/snooze` to defer, or `PUT /v1/issues/actions/archive` to close out issues you have handled.

## Conventions & errors
- Pagination is offset-based (`count`/`offset`). No idempotency-key is documented, so avoid blind retries on the mutating PUT calls.
- Handle the documented status codes: 400 (malformed body), 403 (bad/missing API key or insufficient permissions), 404 (unknown issue), 405/415 (method/content-type), 5XX (server). See `errors/cycognito-problem-types.yml`.
