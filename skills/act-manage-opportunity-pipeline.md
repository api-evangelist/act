---
name: act-manage-opportunity-pipeline
description: Work an Act! sales pipeline — read processes and stages, create and advance opportunities, attach products, associate contacts and companies, and read the built-in pipeline analytics.
api: act:web-api
generated: '2026-08-13'
method: generated
source: >-
  Grounded in openapi/act-opportunities-api-openapi.yml,
  openapi/act-products-api-openapi.yml and openapi/act-analytics-api-openapi.yml.
operations:
  - Opportunities_GetOpportunities_DC941A41
  - Opportunities_GetOpportunity_E0756909
  - Opportunities_PostOpportunity_A5EFCD6E
  - Opportunities_PatchOpportunity_254E3997
  - Opportunities_PutOpportunity_A7A0EC30
  - Opportunities_GetOpportunityProcesses_5865E751
  - Opportunities_GetOpportunityStages_6D9C2133
  - Opportunities_GetOpportunityStagesByProcess_B86A73FE
  - Opportunities_GetOpportunityProducts_8E133CEE
  - Opportunities_PostOpportunityProduct_128DC173
  - Contacts_PutAssociatedContactsToOpportunity_098F6405
  - Companies_PutAssociatedCompaniesToOpportunity_D9F284C2
  - Analytics_GetOpportunityCountByStages_CD8840AE
  - Analytics_GetOpportunityAverageTimeInStage_DCAE7DC3
---

# Manage an Act! opportunity pipeline

Run `act-connect-and-discover` first.

## Read the pipeline definition before creating anything

Act! pipelines are **configured per database**. Stage names and ids are not universal — read them, never hardcode them.

1. `Opportunities_GetOpportunityProcesses_5865E751` — `GET /api/opportunities/processes` — the sales processes defined in this database.
2. `Opportunities_GetOpportunityStagesByProcess_B86A73FE` — `GET /api/opportunities/processes/{processId}/stages` — the ordered stages of one process.
3. `Opportunities_GetOpportunityStages_6D9C2133` — `GET /api/opportunities/stages` — all stages across processes.

Cache the process/stage map for the session.

## Query opportunities

`Opportunities_GetOpportunities_DC941A41` — `GET /api/opportunities` — accepts an OData query.

```
GET {base}/api/opportunities?$select=name,id&$expand=stage($expand=process($select=name,id);$select=name,id)&$top=50&$skip=0&$orderby=name desc
```

Filter query parameters this operation declares in addition to OData: `stageIds`, `statuses`, `amountValue` + `amountOperation`, `probabilityValue` + `probabilityOperation`, `dateType`, `valueType`, `userIds`. Use those where they exist rather than pushing everything through `$filter` — they map to Act!'s own indexed lookups.

## Create

`Opportunities_PostOpportunity_A5EFCD6E` — `POST /api/opportunities` — returns `201`.

The `stage` on the body must reference a stage id you read in step 1. There is no idempotency key, so search first with a `$filter` on the opportunity name plus the associated contact before creating.

## Advance a deal

`Opportunities_PatchOpportunity_254E3997` — `PATCH /api/opportunities/{opportunityId}` — partial update. This is how you move a deal to the next stage, change the amount, or set the status. Prefer PATCH over `Opportunities_PutOpportunity_A7A0EC30` (full replace) unless you hold the whole record.

Read the current record with `Opportunities_GetOpportunity_E0756909` first if another user may have edited it — Act! has no ETag/If-Match concurrency control, so last write wins silently.

## Associate people and companies

An opportunity carries `contacts[]`, `companies[]` and `groups[]` arrays, and there are dedicated association routes:

- `Contacts_PutAssociatedContactsToOpportunity_098F6405` — `PUT /api/opportunities/{opportunityId}/associated-contacts/{contactId}`
- `Companies_PutAssociatedCompaniesToOpportunity_D9F284C2` — `PUT /api/opportunities/{opportunityId}/associated-companies/{companyId}`
- `Groups_PutAssociatedGroupsToOpportunity_4AD8AA6A` — `PUT /api/opportunities/{opportunityId}/associated-groups/{groupId}`

## Line items

- `Opportunities_GetOpportunityProducts_8E133CEE` — `GET /api/opportunities/{opportunityId}/products`
- `Opportunities_PostOpportunityProduct_128DC173` — `POST /api/opportunities/{opportunityId}/products`
- `Opportunities_PatchOpportunityProduct_647E55BB` — `PATCH /api/opportunities/{opportunityId}/products/{productId}`

An `OpportunityProduct` is a join record carrying `opportunityID`, `productID` and the line-item pricing. Product ids come from `GET /api/products/{productID}`.

## Pipeline analytics without computing them yourself

Act! ships an Analytics tag (29 operations). Two are directly useful here:

- `Analytics_GetOpportunityCountByStages_CD8840AE` — `GET /api/stages/analytics/opportunity-count`
- `Analytics_GetOpportunityAverageTimeInStage_DCAE7DC3` — `GET /api/opportunity/analytics/stage-time`

Call these rather than paging every opportunity and aggregating client-side. They are cheaper and they use Act!'s own stage-transition history.

## Context you also have

- `Notes_GetByOpportunity_DB8CCF05` — `GET /api/opportunities/{id}/notes`
- `History_GetByOpportunity_BC00064D` — `GET /api/opportunities/{id}/history`
- `Documents_GetByOpportunity_FCF404D8` — `GET /api/opportunities/{id}/documents`
- `Tasks_GetAssociatedTasksFromOpportunity_A2697D87` — `GET /api/opportunities/{opportunityId}/associated-tasks`

## Access control

`Opportunities_GetOpportunityAccessLevel_8D67555D` and `Opportunities_PutOpportunityAccessors_9206D7E6` read and set who can see a deal. A `404` on an opportunity can mean "private to another user", not "does not exist".
