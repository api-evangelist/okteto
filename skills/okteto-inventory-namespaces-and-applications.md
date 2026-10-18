---
name: okteto-inventory-namespaces-and-applications
description: Inventory every Okteto namespace (development or preview) and the applications deployed in each, using the Okteto Public API (BETA).
api: openapi/okteto-openapi.yml
operations:
- ListNamespaces
- ListApplicationsForNamespace
generated: '2026-10-18'
method: generated
---
# Inventory Okteto namespaces and applications

**Auth:** Bearer token — an Okteto *Admin Access Token* (Dashboard → Admin → Admin Access Tokens). Base URL `https://{oktetoURL}/api/v0`, where `oktetoURL` is your own Okteto instance.

1. `ListNamespaces` — `GET /namespaces`. Optionally pass the `type` query parameter to restrict to development or preview namespaces. Collect each namespace name.
2. For each namespace, `ListApplicationsForNamespace` — `GET /namespaces/{namespaceName}/applications`.
3. Join the results into a namespace → applications map (useful for finding stale preview environments or unused dev namespaces).

**Rules**
- Both operations are read-only GETs; retrying is safe.
- `401` = missing/invalid token; `403` = token lacks admin scope; `404` on step 2 = namespace no longer exists (skip it); `400` on step 1 = invalid `type` value; `500` = retry with backoff.
- The API is BETA (version 0.1.0); write operations (deploy, destroy, sleep) are done through the Okteto CLI — see the provider-published skills in `skills/_provider/`.
