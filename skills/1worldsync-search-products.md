---
name: Search 1WorldSync products
description: Query the 1WorldSync Content1 Read API for product master data — count matches, then fetch a page of products for a search criteria.
api: openapi/1worldsync-content1-openapi-original.json
operations: [fetchItemCountByCriteria, fetchItemByCriteria]
---

# Search 1WorldSync products

Use the Content1 Read API (base URL `https://content1-api.1worldsync.com`) to find
product master data. Every request is HMAC-signed — see
`authentication/1worldsync-authentication.yml`.

## Authentication (required on every call)

Send the `appid` and `hashcode` request headers and the `timestamp` query
parameter. Compute `hashcode` as the HMAC-SHA256 of the canonical request string
using your 32-character secret key. Your 8-character `appid` and secret key are
issued at registration. Use the Pre-Production host
(`https://content1-api.preprod.1worldsync.com`) with your test credentials first.

## Steps

1. **Count matches** — `POST /V1/product/count` (`fetchItemCountByCriteria`) with
   your search criteria in the JSON body. The response is an `ItemCount`. Use it to
   decide paging.
2. **Fetch a page** — `POST /V1/product/fetch` (`fetchItemByCriteria`) with the
   `pageSize` query parameter (max 1000) and the same criteria. The response is an
   `ItemFetchResult` wrapping a list of `Item`. A `204` means no products matched.

## Rules

- `pageSize` must not exceed 1000 or the API returns `500`.
- Errors return `{requestId, code, reason}` (see `errors/1worldsync-error-codes.yml`);
  a `401` indicates an authentication/validation problem — recheck the signature.
- Conventions (pagination, tracing) are in `conventions/1worldsync-conventions.yml`.
