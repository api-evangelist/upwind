---
name: Triage Upwind threat detections
description: List, inspect, and update threat detections in the Upwind platform via the Management REST API v1.
api: openapi/upwind-management-v1-openapi.yml
operations: [list-threat-detections, get-threat-detection, update-threat-detection]
generated: '2026-07-21'
method: generated
---

# Triage Upwind threat detections

## Auth

1. Exchange client credentials for a JWT: `POST https://auth.upwind.io/oauth/token` with `client_id`, `client_secret`, `grant_type=client_credentials`, and `audience` set to your regional API host (`https://api.upwind.io`, `https://api.eu.upwind.io`, or `https://api.me.upwind.io`).
2. Send `Authorization: Bearer <token>` on every request. Tokens expire after 24 hours — re-fetch rather than cache long-term.

## Steps

1. **List detections** — `list-threat-detections` (`GET /v1/organizations/{organization-id}/threat-detections`). Filter with query parameters (e.g. severity) and paginate: this API uses page-based (`page`, `per-page`) or token-based (`page-token`, `per-page`) pagination depending on the endpoint; follow the RFC 5988 `Link` response header when present.
2. **Inspect one detection** — `get-threat-detection` (`GET .../threat-detections/{detection-id}`) for the full Threat Detection object.
3. **Update its state** — `update-threat-detection` to change the detection as triage proceeds.

## Rules

- Every path is organization-scoped: `/v1/organizations/{organization-id}/...`.
- Expect `401`/`403` on auth problems, `429` on rate limiting (back off before retrying — no idempotency-key contract exists, but these reads/updates are safe to retry after a 429).
- Errors are plain `application/json`; see `errors/upwind-problem-types.yml` and `conventions/upwind-conventions.yml`.
