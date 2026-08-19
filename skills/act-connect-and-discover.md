---
name: act-connect-and-discover
description: Connect to an Act! Web API instance — find the host, mint a JWT, confirm the deployed build, and read the tenant's real field/entity schema before doing anything else. Every other Act! skill assumes this ran first.
api: act:web-api
generated: '2026-08-13'
method: generated
source: >-
  Grounded in openapi/act-system-api-openapi.yml and
  openapi/act-metadata-info-api-openapi.yml; auth flow and error codes from
  https://apimta.act.com/act.web.api/.
operations:
  - System_GetVersion_2833248C
  - MetadataInfo_GetField_76332063
  - MetadataInfo_GetEntities_76EDEF77
  - MetadataInfo_GetFields_387CA408
  - Database_GetOpenSessions_E10D6976
---

# Connect to an Act! Web API instance

Act! is not one API on one host. Do these four steps in order; skipping any of them is the usual cause of a failed Act! integration.

## 1. Get the host from the user — never guess it

There are three deployment shapes:

| Shape | Base URL |
|---|---|
| Act! Premium Cloud tenant | `https://{server}/{customer}-api/act.web.api` |
| Self-hosted (Act! Premium for Web / Windows) | `https://{server}/act.web.api` |
| Act!'s public reference instance (US region) | `https://apimta.act.com/act.web.api` |

Ask the user for their base URL and their **database name**. Do not assume `apimta.act.com` — that is Act!'s documentation instance, not the user's data.

## 2. Confirm the build before assuming an operation exists

`System_GetVersion_2833248C` — `GET /api/system` — is **anonymous**. Call it first.

```
GET {base}/api/system
```

Returns `apiVersion` and `sdkVersion`. Self-hosted customers run whatever build they last installed, so two Act! instances legitimately expose different operations. If this call fails, the base URL is wrong; stop and re-ask rather than trying credentials.

## 3. Mint a token

```
GET {base}/authorize
Authorization: Basic base64(username:password)
Act-Database-Name: {database name}
```

The response body is a JWT. Send it on every subsequent request:

```
Authorization: Bearer {token}
Act-Database-Name: {database name}
```

You can also present an existing bearer token to `/authorize` to get a fresh one.

**These are the end user's real Act! credentials.** There is no OAuth, no API key, and no scoped token — whatever the human can see and change, this integration can see and change. Handle them accordingly and never log them.

Failure codes, per Act!'s own documentation:

| Code | Meaning | What to do |
|---|---|---|
| 401 | Unauthorized | Re-authorize; the token expired or the credentials are wrong. |
| 403 | Forbidden | The user lacks permission for that resource. |
| 4030 | Incompatibility issue with Act! | Build mismatch — compare `GET /api/system`. |
| 4031 | Subscription required | Billing wall. The subscription does not include API access. Stop; no retry will fix it. |
| 4032 | API access permission required | An Act! administrator must grant the user API access. Stop and tell the user exactly that. |

Do not retry on 4031 or 4032. They are entitlement walls, not transient errors.

## 4. Read the tenant's schema before composing any query

Act! databases are user-extensible. Custom fields and custom entities differ per tenant, so the shipped schema in `openapi/` is a floor, not the truth for this database.

- `MetadataInfo_GetField_76332063` — `GET /api/metadata/fields` — every field in the database.
- `MetadataInfo_GetFields_387CA408` — `GET /api/metadata/{recordType}/fields` — fields for one record type.
- `MetadataInfo_GetEntities_76EDEF77` — `GET /api/metadata/entities` — custom entities (tenant-defined tables).

Cache the result for the session and use it to validate every `$filter` and `$select` you build. A field name that does not exist here will come back as a bare `400` with no indication of which field was wrong.

## Before anything destructive

`Database_GetOpenSessions_E10D6976` — `GET /api/opened/sessions` — lists open connections to the database. Check it before a bulk write or anything under `/api/database/`.

## Conventions that apply to every call

- Collections are **bare JSON arrays** — no envelope, no total, no next link. Page with `$top`/`$skip` until a short page comes back.
- **Nothing is idempotent.** There is no idempotency key. A retried POST creates a duplicate record. Before retrying a failed write, read back with a `$filter` to see whether it actually landed.
- Errors are ASP.NET exception objects (`message`, `exceptionMessage`, `exceptionType`, `stackTrace`), not RFC 9457 problem+json. Do not build logic on `exceptionType`; it is not a contract.

See `conventions/act-conventions.yml` and `errors/act-problem-types.yml`.
