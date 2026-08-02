---
name: Onboard and price a Zalando article
description: Create or update a product on the Zalando zDirect Platform, set its attributes, publish a price, and set stock so the article becomes sellable.
api: openapi/zalando-products-openapi.yml
operations:
  - "POST /merchants/{merchant-id}/products"        # zalando-products-openapi.yml
  - "GET /merchants/{merchant-id}/product-attributes" # zalando-product-attributes-openapi.yml
  - "GET /merchants/{merchant-id}/article-requirements" # zalando-article-requirements-openapi.yml
  - "POST /merchants/{merchant-id}/prices"          # zalando-prices-openapi.yml
  - "POST /merchants/{merchant-id}/stocks"          # zalando-stocks-openapi.yml
generated: '2026-07-21'
method: generated
---

# Onboard and price a Zalando article

Operating instructions for an agent using the Zalando zDirect Merchant Platform. All requests are JSON:API (`application/vnd.api+json`); errors come back as `application/problem+json` or JSON:API error objects (`errors/zalando-problem-types.yml`).

## 1. Authenticate
Request an OAuth 2.0 client-credentials token from `https://api.merchants.zalando.com/auth/token` (sandbox: `https://api-sandbox.merchants.zalando.com/auth/token`) using your zDirect app `client_id`/`client_secret`. Send it as `Authorization: Bearer <token>` on every call. Required scopes: `products/write`, `products/attributes/read`, `products/price/write`, `products/stock/write`. See `authentication/zalando-authentication.yml` and `scopes/zalando-scopes.yml`.

## 2. Discover the article requirements
`GET /merchants/{merchant-id}/article-requirements` (Article Requirements API) and `GET /merchants/{merchant-id}/product-attributes` (Product Attributes API) to learn the UAF outline and valid attribute values for the target category before submitting.

## 3. Submit the product
`POST /merchants/{merchant-id}/products` (Products API) with the attributes assembled in step 2. Include an `X-Flow-Id` header for tracing.

## 4. Set the price
`POST /merchants/{merchant-id}/prices` (Merchant Price Service). Prices are configured at country level. Poll the Price Reporting API (`GET .../price-reporting`) to confirm the price update was accepted.

## 5. Set stock
`POST /merchants/{merchant-id}/stocks` (Merchant Stock Service) so the article has availability.

## Rules
- Respect rate limits: 429 responses carry `Retry-After` and `X-Rate-Limit`; back off accordingly (Product Attributes 1000/min, Product Submissions 25/sec).
- There is no `Idempotency-Key`; guard against duplicate submits on the client side.
- Pagination on collections is cursor-based (`cursor`, `limit`, `sort`).
