---
name: Manage Upwind webhook integrations
description: Create, list, update, and delete Custom Webhook integrations that push threat detections and issue findings to your tools.
api: openapi/upwind-management-v1-openapi.yml
operations: [createWebhookIntegration, getWebhookIntegrations, updateWebhookIntegration, deleteWebhookIntegration]
generated: '2026-07-21'
method: generated
---

# Manage Upwind webhook integrations

## Auth

OAuth 2.0 client credentials → JWT bearer token (24h expiry), regional `audience`; see `authentication/upwind-authentication.yml`.

## Steps

1. **Create** — `createWebhookIntegration` (`POST /v1/organizations/{organization-id}/...`) with your receiver URL and authentication (API Key or Bearer Token header) for outbound deliveries.
2. **Verify** — `getWebhookIntegrations` to list configured webhooks and confirm the new one.
3. **Update** — `updateWebhookIntegration` to rotate the receiver credential or change the endpoint.
4. **Remove** — `deleteWebhookIntegration` when decommissioning.

## What you will receive

Upwind POSTs `application/json` events — including threat detections and issue findings — whose payloads mirror the corresponding API objects (e.g. the Threat Detection object from `get-threat-detection`). See `asyncapi/upwind-webhooks.yml`.

## Rules

- Webhook receiver credentials are secrets: store them in a vault and rotate via `updateWebhookIntegration`.
- Handle `429` on management calls; deliveries themselves originate from Upwind.
