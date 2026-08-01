---
name: Search the Upwind inventory asset graph
description: Query cloud assets by relationship criteria (graph traversal) with the Inventory API v2 and paginate with cursors.
api: openapi/upwind-management-v2-openapi.yml
operations: [assetsSearch, getAsset, getAssetExample]
generated: '2026-07-21'
method: generated
---

# Search the Upwind inventory asset graph

## Auth

Same OAuth 2.0 client-credentials flow as all Upwind Management APIs (see `authentication/upwind-authentication.yml`): token from `https://auth.upwind.io/oauth/token` with the regional `audience`, sent as `Authorization: Bearer <token>`.

## Steps

1. **Search by relationships** — `assetsSearch` (`POST /v2/organizations/{organization-id}/inventory/assets/search`). Build `conditions` in the JSON body to traverse relationships, e.g. all Virtual Machines "Connected To" a Subnet or "Part Of" an EKS cluster.
2. **Paginate** — pass `limit` (1–100, default 20) and follow `metadata.next_cursor` via the `cursor` query parameter until `next_cursor` is `null`.
3. **Fetch one asset** — `getAsset` for full asset metadata; `getAssetExample` shows the schema shape for an asset kind.

## Rules

- Rate limit on search: 100 requests/minute per organization — throttle bulk traversals and honor `429`.
- Cursors are opaque; never construct them manually.
- Results depend on the asset schema per kind; inspect `getAssetExample` before mapping fields.
