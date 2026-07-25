---
name: Inventory and scope the CyCognito attack surface
description: Query discovered assets, inspect one, annotate it, and manage attack-surface scope and organization attribution.
api: openapi/cycognito-v1-openapi-original.json
operations:
- POST /v1/assets
- POST /v1/assets/{asset_type}
- GET /v1/assets/{asset_type}/{asset_id}
- PUT /v1/assets/{asset_type}/{asset_id}/investigation-status
- PUT /v1/assets/actions/add/comment
- PUT /v1/assets/actions/{action}/tags
- POST /v1/include-assets
- GET /v1/realm/asset-summary
---

# Inventory and scope the CyCognito attack surface

Use the CyCognito API V1 to explore your discovered external assets and manage what is in scope.

## Auth
API-key authenticated: send your key in the `Authorization` header. Base URL: `https://api.platform.cycognito.com`.

## Steps
1. **Summarize the realm** — `GET /v1/realm/asset-summary` for an aggregate view of the attack surface.
2. **List assets** — `POST /v1/assets` (all types) or `POST /v1/assets/{asset_type}` for a specific type. Page with `count`/`offset`, filter with `q` / `advanced-search`, project with `fields`, order with `sort-by` / `sort-order`.
3. **Inspect one asset** — `GET /v1/assets/{asset_type}/{asset_id}`.
4. **Set investigation status** — `PUT /v1/assets/{asset_type}/{asset_id}/investigation-status`.
5. **Annotate** — `PUT /v1/assets/actions/add/comment` and `PUT /v1/assets/actions/{action}/tags` (action = add/remove) to label assets.
6. **Manage scope** — `POST /v1/include-assets` to bring assets into scope for testing/attribution.

## Conventions & errors
- Offset pagination (`count`/`offset`); no documented idempotency key — do not blindly retry mutating PUT/POST-action calls.
- Expect 400 / 403 / 404 / 405 / 415 / 5XX per `errors/cycognito-problem-types.yml`; 403 usually means a bad or missing API key.
