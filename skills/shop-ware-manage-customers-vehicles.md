---
name: Manage Shop-Ware customers and vehicles
description: List and manage a shop's customers, their contacts, and vehicles via the Shop-Ware Partner API.
api: openapi/shop-ware-v1-openapi-original.yml
operations:
  - "GET /api/v1/tenants/{tenant_id}/customers"
  - "GET /api/v1/tenants/{tenant_id}/customers/{id}"
  - "GET /api/v1/tenants/{tenant_id}/customers/{customer_id}/contacts"
  - "POST /api/v1/tenants/{tenant_id}/customers/{customer_id}/contacts"
  - "GET /api/v1/tenants/{tenant_id}/vehicles"
  - "GET /api/v1/tenants/{tenant_id}/vehicles/{id}"
---

# Manage Shop-Ware customers and vehicles

Use this skill to read and maintain the customer and vehicle records for a shop (tenant).

## Authentication
Both headers required on every request: `X-Api-Partner-Id`, `X-Api-Secret`.
Base URL: `https://api.shop-ware.com`.

## Steps
1. **List customers.** `GET /api/v1/tenants/{tenant_id}/customers`
   (offset paging: `page`, `per_page`).
2. **Retrieve one customer.** `GET /api/v1/tenants/{tenant_id}/customers/{id}`.
3. **List a customer's contacts.** `GET /api/v1/tenants/{tenant_id}/customers/{customer_id}/contacts`.
4. **Add a contact.** `POST /api/v1/tenants/{tenant_id}/customers/{customer_id}/contacts`.
5. **List / retrieve vehicles.** `GET /api/v1/tenants/{tenant_id}/vehicles` and
   `GET /api/v1/tenants/{tenant_id}/vehicles/{id}`.

## Conventions
- A customer belongs to a tenant and owns many contacts
  (see `data-model/shop-ware-data-model.yml`).
- Watch `customer.created|updated|deleted|merged` webhooks to keep a downstream copy in sync;
  note that customers can be **merged**, which fires `customer.merged`.
- No idempotency key is supported — check for an existing record before creating a duplicate.
