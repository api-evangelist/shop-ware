---
name: Manage Shop-Ware repair orders
description: List, retrieve, start, and share repair orders for a shop (tenant) via the Shop-Ware Partner API.
api: openapi/shop-ware-v2-openapi-original.yml
operations:
  - "GET /api/v1/tenants/{tenant_id}/repair_orders"
  - "GET /api/v1/tenants/{tenant_id}/repair_orders/{id}"
  - "POST /api/v1/tenants/{tenant_id}/repair_orders/{id}/start"
  - "POST /api/v1/tenants/{tenant_id}/repair_orders/{id}/share"
  - "GET /api/v2/tenants/{tenant_id}/repair_orders"
---

# Manage Shop-Ware repair orders

Use this skill to work with repair orders (ROs) for a single Shop-Ware shop.

## Authentication
Every request requires two header credentials (see `authentication/shop-ware-authentication.yml`):
- `X-Api-Partner-Id: <your partner id>`
- `X-Api-Secret: <your partner secret>`

Base URL: `https://api.shop-ware.com`

## Steps
1. **Resolve the tenant.** A shop is a *tenant*. Confirm you are authorized for it with
   `GET /api/v1/partners/{partner_id}/authorizations`, which lists accessible tenants.
2. **List repair orders.** Call `GET /api/v2/tenants/{tenant_id}/repair_orders`.
   V2 uses opaque **cursor** pagination — pass `per_page` and the `cursor` returned by the
   previous page (empty cursor = first page). V1 (`GET /api/v1/tenants/{tenant_id}/repair_orders`)
   uses `page`/`per_page` offset paging instead.
3. **Retrieve one RO.** `GET /api/v1/tenants/{tenant_id}/repair_orders/{id}`.
4. **Start work on an RO.** `POST /api/v1/tenants/{tenant_id}/repair_orders/{id}/start`.
5. **Share an RO with the customer.** `POST /api/v1/tenants/{tenant_id}/repair_orders/{id}/share`.

## Conventions & cautions
- No idempotency key is supported (`conventions/shop-ware-conventions.yml`); do not blindly retry the
  `start`/`share` write actions on timeout — re-check state first.
- Prefer **V2** endpoints where available; V1 and V2 run concurrently.
- To react to RO changes in near-real-time, subscribe to `repair_order.created|updated|deleted`
  webhooks (see the separate webhooks skill).
