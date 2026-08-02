---
name: Retrieve and fulfill Zalando orders
description: Poll merchant orders on the Zalando zDirect Platform, drill into order items and lines, and reconcile ZFS stock movements for fulfilled inventory.
api: openapi/zalando-orders-openapi.yml
operations:
  - "GET /merchants/{merchant-id}/orders"                       # zalando-orders-openapi.yml
  - "GET /merchants/{merchant-id}/orders/{order-id}"            # zalando-orders-openapi.yml
  - "GET /merchants/{merchant-id}/orders/{order-id}/order-items" # zalando-orders-openapi.yml
  - "GET /zfs/stock-movements"                                  # zalando-zfs-stock-movements-openapi.yml
generated: '2026-07-21'
method: generated
---

# Retrieve and fulfill Zalando orders

Operating instructions for an agent reading merchant orders from the Zalando zDirect Orders API and reconciling Zalando Fulfillment Solutions (ZFS) movements.

## 1. Authenticate
OAuth 2.0 client-credentials Bearer token (see `authentication/zalando-authentication.yml`). Scopes: `orders/read`, plus `zfs/received-item/read` / `zfs.icm-reports.read` for ZFS reconciliation.

## 2. List orders
`GET /merchants/{merchant-id}/orders` with `created_after` / `modified_after` filters (ISO 8601). Page with `cursor` + `limit`; sort with `sort`. Retention is 1 year — orders older than that are not returned.

## 3. Drill into an order
`GET /merchants/{merchant-id}/orders/{order-id}` then `GET .../order-items` and the order-lines relationship to get item-level detail and status transitions (`Order` has_many `OrderItem` has_many `OrderLine` — see `data-model/zalando-data-model.yml`).

## 4. Reconcile fulfillment
For ZFS merchants, `GET /zfs/stock-movements` (base `https://api.merchants.zalando.com/zfs/stock-movements`) to reconcile inbound/outbound stock against orders, and the Cross-Border Movements report for inter-warehouse transfers.

## Rules
- Send `X-Flow-Id` for request tracing.
- Handle 429 with `Retry-After`; handle 503 with truncated exponential backoff.
- Errors: `application/problem+json` or JSON:API error objects (`errors/zalando-problem-types.yml`).
