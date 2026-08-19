---
name: act-sync-contacts
description: Search, read, create and update Act! contacts and their company/group associations using OData queries, with safe handling for the missing idempotency and duplicate-detection behaviour.
api: act:web-api
generated: '2026-08-13'
method: generated
source: >-
  Grounded in openapi/act-contacts-api-openapi.yml,
  openapi/act-companies-api-openapi.yml and openapi/act-groups-api-openapi.yml;
  OData semantics from https://apimta.act.com/act.web.api/OData/Index.
operations:
  - Contacts_GetContacts_70C5959A
  - Contacts_GetContact_E09EB702
  - Contacts_GetContactsByIds_55F074BD
  - Contacts_PostContact_03914590
  - Contacts_PutContact_9B5285C5
  - Contacts_PatchContact_7296FF7D
  - Contacts_DeleteContact_59870332
  - Contacts_PutDuplicateContacts_5BFA5E9A
  - Contacts_GetMyRecord_A9BCA3EB
  - Companies_GetAssociatedCompaniesByContact_87388C7A
  - Companies_PutAssociatedCompaniesToContact_49B35DB0
  - Groups_GetAssociatedGroupsFromContact_584D5498
  - Groups_PutAssociatedGroupsToContact_F74551DA
---

# Sync contacts with Act!

Run `act-connect-and-discover` first. You need a bearer token, the `Act-Database-Name` header, and the tenant's field list.

## Search

`Contacts_GetContacts_70C5959A` — `GET /api/contacts` — takes an OData query.

```
GET {base}/api/contacts?$filter=(Company eq 'Pool Supplies Inc.' and FirstName eq 'John')
GET {base}/api/contacts?$filter=contains(fullname, 'mark')
GET {base}/api/contacts?$filter=startswith(lastname, 'c') or startswith(lastname, 'b')
GET {base}/api/contacts?$filter=(created ge 2026-08-01T00:00:00Z)&$orderby=created desc
GET {base}/api/contacts?$select=id,fullname,company&$expand=businessAddress($select=city,state)&$top=50&$skip=0
```

Supported operators: `eq ne lt le gt ge`, `and`/`or`, `contains()`, `startswith()`, `endswith()`. Navigation properties can be filtered (`businessAddress/State eq 'CA'`). Anything else throws — you get a bare `400`.

The `phoneFormat` query parameter (`None` | `E164`) normalises phone numbers on the way out. Ask for `E164` if you are going to dial or match them.

## Page correctly

There is no `has_more`, no total and no next link. Increment `$skip` by `$top` until a page comes back shorter than `$top`.

```
GET {base}/api/contacts?$top=100&$skip=0
GET {base}/api/contacts?$top=100&$skip=100
...
```

Use a stable `$orderby` (for example `$orderby=id`) or records can shift between pages while you walk them.

## Read one

- `Contacts_GetContact_E09EB702` — `GET /api/contacts/{contactId}`
- `Contacts_GetContactsByIds_55F074BD` — `GET /api/contacts/by-ids` — batch read; prefer this over N single reads.
- `Contacts_GetMyRecord_A9BCA3EB` — `GET /api/contacts/myrecord` — the authenticated user's own contact record.

A `404` may mean the record exists but is private to another user — Act! enforces record-level access via `recordManagerID` and `isPrivate`. Do not treat `404` as proof of absence before you try creating a duplicate.

## Create — read first, because there is no idempotency key

`Contacts_PostContact_03914590` — `POST /api/contacts` — returns `201` with the created contact.

Act! has **no** `Idempotency-Key` header. If a POST times out you cannot tell whether it landed. Always:

1. Search for the contact with a `$filter` on a field you consider unique for this integration (an email, an external id you store in a custom field).
2. If it exists, `PATCH` it.
3. If not, `POST` it.
4. If the POST errors ambiguously (a `500`, a timeout), **re-run step 1 before retrying** — do not blind-retry.

## Update

- `Contacts_PatchContact_7296FF7D` — `PATCH /api/contacts/{contactId}` — partial update. Prefer this.
- `Contacts_PutContact_9B5285C5` — `PUT /api/contacts/{contactId}` — full replace. Fields you omit are at risk; only use it when you hold the whole record.

Custom fields go in the same object, using the names from `GET /api/metadata/fields`.

## Associations

A contact has a single `companyID` field **and** association routes. Both exist.

- `Companies_GetAssociatedCompaniesByContact_87388C7A` — `GET /api/contacts/{contactId}/associated-companies`
- `Companies_PutAssociatedCompaniesToContact_49B35DB0` — `PUT /api/contacts/{contactId}/associated-companies/{companyId}`
- `Groups_GetAssociatedGroupsFromContact_584D5498` — `GET /api/contacts/{contactId}/associated-groups`
- `Groups_PutAssociatedGroupsToContact_F74551DA` — `PUT /api/contacts/{contactId}/associated-groups/{groupId}`

## Deduplicate

`Contacts_PutDuplicateContacts_5BFA5E9A` — `PUT /api/contacts/duplicate` — Act!'s own duplicate handling. Use it rather than writing merge logic; Act! knows which fields it treats as identity.

## Delete

`Contacts_DeleteContact_59870332` — `DELETE /api/contacts/{contactId}`. Irreversible, and there is no test mode — every call runs against live CRM data. Confirm with the user before calling it.

## Batch several writes into one request

```
POST {base}/api/$batch
Content-Type: multipart/mixed; boundary="{boundary}"
Authorization: Bearer {token}
Act-Database-Name: {database}
```

Each part is `Content-Type: application/http; msgtype=request` wrapping a normal request line and body. Note that batching does not make the set atomic — parts can individually fail.

## Rate limits

On Act! Premium Cloud, read `X-RateLimit-Limit`, `X-RateLimit-Remaining` and `X-RateLimit-Reset` from the first response and pace yourself against them. Act! publishes no number and no status code for exhaustion, so back off to the instant named by `X-RateLimit-Reset` rather than a fixed delay.
