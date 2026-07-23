---
name: Fetch 1WorldSync product hierarchies
description: Retrieve item hierarchy details (parent/child packaging levels) from the 1WorldSync Content1 Read API for a search criteria.
api: openapi/1worldsync-content1-openapi-original.json
operations: [fetchHierarchiesByCriteria]
---

# Fetch 1WorldSync product hierarchies

Use the Content1 Read API (base URL `https://content1-api.1worldsync.com`) to
retrieve product hierarchies — the parent/child packaging levels that link items
(e.g. case → inner pack → each). Every request is HMAC-signed; see
`authentication/1worldsync-authentication.yml`.

## Authentication (required on every call)

Send the `appid` and `hashcode` request headers and the `timestamp` query
parameter. `hashcode` is the HMAC-SHA256 of the canonical request string using your
secret key. Test against the Pre-Production host first
(`https://content1-api.preprod.1worldsync.com`).

## Steps

1. **Fetch hierarchies** — `POST /V1/product/hierarchy`
   (`fetchHierarchiesByCriteria`) with your search criteria in the JSON body and the
   `pageSize` query parameter (max 1000). The response is a `HierarchyFetchResult`
   wrapping `HierarchyDetails`, each holding a tree of `ItemFetchHierarchy` nodes
   whose `children` are nested recursively.

## Rules

- `pageSize` must not exceed 1000 or the API returns `500`.
- Errors return `{requestId, code, reason}`; a `401` indicates an
  authentication/validation problem (see `errors/1worldsync-error-codes.yml`).
- The hierarchy entity graph is in `data-model/1worldsync-data-model.yml`.
