---
name: act-log-interactions
description: Write an interaction back into Act! — log a completed call, meeting or email as a History record, add a Note, schedule a follow-up Task, and attach a file, associating each with the right contacts, companies, groups and opportunities in one object.
api: act:web-api
generated: '2026-08-13'
method: generated
source: >-
  Grounded in openapi/act-history-api-openapi.yml,
  openapi/act-notes-api-openapi.yml, openapi/act-tasks-api-openapi.yml and
  openapi/act-supplemental-files-api-openapi.yml.
operations:
  - History_Post_E2B6CABD
  - History_Get_F9FA0741
  - History_GetByContact_6664D670
  - History_Patch_B2B2DD81
  - HistoryTypes_GetHistoryTypes_2691B741
  - Notes_Post_DE6D7616
  - Notes_GetByContact_0ECBDD59
  - Notes_Patch_5F3A0F8F
  - Tasks_PostTasks_87E31868
  - Tasks_GetTasks_82532732
  - TaskTypes_Get_CA70DD31
  - SupplementalFiles_PostHistoryAttachments_B509AF9D
  - SupplementalFiles_PostNoteAttachments_ACC89468
  - SupplementalFiles_GetUploadStatus_A24AF9D4
---

# Log an interaction into Act!

Run `act-connect-and-discover` first.

## Pick the right object

| What happened | Object | Endpoint |
|---|---|---|
| Something completed (a call, a meeting, an email that went out) | **History** | `POST /api/History` |
| A free-text observation with no time semantics | **Note** | `POST /api/notes` |
| Something to do later | **Task** | `POST /api/tasks` |

Do not log a completed call as a Task; Act!'s reporting and its Analytics operations read History for "what actually happened".

## The association trick that saves you three calls

History, Note and Opportunity objects each carry **arrays** of embedded records:

```json
{
  "regarding": "Discovery call",
  "details": "Walked through pricing. Sending the proposal Friday.",
  "historyTypeID": 2,
  "startTime": "2026-08-13T15:00:00Z",
  "endTime": "2026-08-13T15:30:00Z",
  "contacts":      [{ "id": "..." }, { "id": "..." }],
  "companies":     [{ "id": "..." }],
  "opportunities": [{ "id": "..." }]
}
```

One `POST /api/History` associates that record with three contacts, a company and a deal. You do not need separate association calls.

## History

- `History_Post_E2B6CABD` — `POST /api/History`
- `HistoryTypes_GetHistoryTypes_2691B741` — `GET /api/history-types` — **read this first.** `historyTypeID` is an integer that means different things in different databases. Never hardcode it.
  (Note: `/api/historytypes` also exists but its operationId is `HistoryTypes_GetDeprecatedHistoryTypes_76F493AB` — use the hyphenated route.)
- `History_GetByContact_6664D670` — `GET /api/contacts/{id}/history` — the interaction timeline for one contact, OData-filterable.
- `History_Get_F9FA0741` — `GET /api/History` — all history, OData-filterable.
- `History_Patch_B2B2DD81` — `PATCH /api/History/{id}`

## Notes

- `Notes_Post_DE6D7616` — `POST /api/notes`
- `Notes_GetByContact_0ECBDD59` — `GET /api/contacts/{id}/notes`
- `Notes_GetByGroup_032215FF` — `GET /api/groups/{id}/notes`
  (Not `/api/group/{id}/notes` — that route's operationId is `Notes_GetByGroupDep_998BE8AB`.)
- `Notes_GetByOpportunity_DB8CCF05` — `GET /api/opportunities/{id}/notes`
- `Notes_Patch_5F3A0F8F` — `PATCH /api/notes/{id}`

Notes carry the same `contacts[]` / `companies[]` / `groups[]` / `opportunities[]` arrays, plus `noteTypeID` and `isPrivate`.

## Follow-up tasks

- `Tasks_PostTasks_87E31868` — `POST /api/tasks`. The summary warns that the Recur Spec is ignored on create — schedule recurring work through ActivitySeries instead (though note all six ActivitySeries operations are named `*Deprecated*`; see `lifecycle/act-lifecycle.yml`).
- `Tasks_GetTasks_82532732` — `GET /api/tasks` — OData-filterable, plus `userIds`, `typeIds`, `start`/`end` query parameters.
- `Tasks_GetAssociatedTasksFromContact_416BB7A5` — `GET /api/contacts/{contactId}/associated-tasks`
- `Tasks_PutClearTaskAlarms_A1BC070F` — `PUT /api/tasks/{id}/clear-alarms`

## Attachments

Attach a file to what you just logged:

- `SupplementalFiles_PostHistoryAttachments_B509AF9D` — `POST /api/attachments/history/{id}`
- `SupplementalFiles_PostNoteAttachments_ACC89468` — `POST /api/attachments/notes/{id}`
- `SupplementalFiles_PostActivityAttachment_1E201089` — `POST /api/attachments/activities/{id}`
- `SupplementalFiles_GetUploadStatus_A24AF9D4` — `GET /api/attachments/{id}/upload-status`

Uploads are chunked. `206` means a chunk was accepted and more are expected. **`423 Locked` means an upload is already in progress for that resource** — wait and retry rather than starting a second upload.

## Privacy

Set `isPrivate` deliberately. Records also carry `recordManagerID` (ownership) — a history record created by an integration user is owned by that user, and other users may not see it depending on the database's access settings.

## No idempotency — the risk here is real

Duplicated history is the most visible failure mode in a CRM: the same call appears twice on the customer timeline. There is no idempotency key. Before retrying an ambiguous `POST /api/History`, query `GET /api/contacts/{id}/history?$filter=(created ge {your request time})` and check whether it landed.
