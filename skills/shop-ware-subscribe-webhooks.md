---
name: Subscribe to Shop-Ware webhooks
description: Register, list, update, and delete HTTPS webhook subscriptions and choose which object_type.event_action events to receive.
api: openapi/shop-ware-v1-openapi-original.yml
operations:
  - "POST /api/v1/webhooks"
  - "GET /api/v1/webhooks"
  - "GET /api/v1/webhooks/{id}"
  - "PUT /api/v1/webhooks/{id}"
  - "DELETE /api/v1/webhooks/{id}"
---

# Subscribe to Shop-Ware webhooks

Use this skill to receive event notifications from Shop-Ware instead of polling.

## Authentication
Send both partner headers on every management call:
- `X-Api-Partner-Id`
- `X-Api-Secret`

## Steps
1. **Choose events.** Events follow `object_type.event_action`. Object types:
   appointment, assignment, canned_job, category, customer, gp_exception, inventory,
   past_recommendation, payment_transaction, purchase_record, repair_order, shop, staff,
   status, vehicle. Actions: `created`, `updated`, `deleted` (plus `customer.merged`).
   The full catalog is in `asyncapi/shop-ware-webhooks.yml`.
2. **Create the subscription.** `POST /api/v1/webhooks` with a JSON body:
   ```json
   { "url": "https://your-endpoint.example.com/hook",
     "events": ["repair_order.created", "repair_order.updated"] }
   ```
   The `url` **must be HTTPS and must not redirect**. Optionally set `format` to `zapier`.
3. **List / inspect.** `GET /api/v1/webhooks` and `GET /api/v1/webhooks/{id}`.
4. **Update events or URL.** `PUT /api/v1/webhooks/{id}`.
5. **Remove.** `DELETE /api/v1/webhooks/{id}`.

## Receiving deliveries
Deliveries arrive as HTTPS `POST` to your `url`, one per subscribed event. Match on the
`event` / `object_type` + `action` fields, then fetch the full resource via its REST endpoint
if you need more than the identifier. See the generated AsyncAPI at
`asyncapi/shop-ware-webhooks-asyncapi.yml`.
