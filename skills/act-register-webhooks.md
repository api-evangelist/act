---
name: act-register-webhooks
description: Register, shape, rotate and suspend Act! Web API webhooks — and understand before you build that Act! webhooks are polled on a 15-minute default interval, are not available on Act! Premium Cloud, and are authenticated with a replayed shared token rather than a signature.
api: act:web-api
generated: '2026-08-13'
method: generated
source: >-
  Grounded in openapi/act-webhooks-api-openapi.yml and
  https://apimta.act.com/act.web.api/ActHooks/Index. Catalog in
  asyncapi/act-webhooks.yml.
operations:
  - Webhooks_Post_EE3CCFA8
  - Webhooks_Get_3F017BC6
  - Webhooks_Get_8857FC56
  - Webhooks_Put_20B03A3C
  - Webhooks_Patch_B8C243E8
  - Webhooks_Delete_649F8293
  - Webhooks_Suspend_A12B132D
  - Webhooks_Continue_511537A0
  - Webhooks_PutTokenReset_C1A718D3
---

# Register Act! webhooks

Run `act-connect-and-discover` first.

## Read this before you design around them

Three facts change the architecture:

1. **They are polled, not pushed.** The `Act.Webhook.Notifications` service polls on `PollingIntervalSeconds`, default **900 seconds (15 minutes)**, and then fires callbacks. Worst-case latency is the interval, not milliseconds. If you need real-time, poll `GET /api/contacts?$filter=(edited ge {watermark})` yourself instead — you will not do worse.
2. **They are not available on Act! Premium Cloud.** Act! states "This is not available in the cloud as of now, but coming soon." The webhook registration database is created by the Act! installer, so today this is a self-hosted capability.
3. **There is no signature.** The `callbackToken` you register is replayed verbatim in the `Authorization` header of every delivery. There is no HMAC, no timestamp, no replay protection — and Act! documents registering plaintext `http://` callback URLs when the receiver has no valid certificate. Treat the token as a shared secret in transit and terminate TLS on your receiver.

## Register

`Webhooks_Post_EE3CCFA8` — `POST /api/webhooks`

```json
{
  "monitor": "Contacts",
  "triggerEvent": "Created",
  "queryOption": null,
  "callbackUrl": "https://your-receiver.example.com/act/contacts/created",
  "callbackToken": "{a secret you generate}",
  "description": "Capture newly created contacts."
}
```

**Monitors:** `Activities`, `Companies`, `Contacts`, `Fields`, `Groups`, `History`, `Opportunities`, `Products`.

**Triggers:** `Created`, `Updated`, `Alarms`.

That is 8 × 3 in principle; `Alarms` is meaningful only on `Activities`.

## Shape the payload with queryOption

With `queryOption: null` the notification carries only the default properties: `id`, `created`, `edited`. For `Alarms`: `id`, `startTime`, `leadMinutes`.

Pass an OData query in `queryOption` to get more:

```json
{ "queryOption": "$select=fullName,company,businessAddress" }
```

**Do not repeat the default properties inside `queryOption`.** Act! warns this "may cause the webhook from firing correctly" — including `id`, `created` or `edited` there can silently break delivery.

Design intent: shape the payload so your receiver does not have to call back for the record. Every avoided callback is one fewer request against the rate limit.

## Two traps

- **Fields webhooks miss out-of-band changes.** Act! caches metadata, so a `Fields` webhook does not notify when a field is created or updated *outside* the API. It only sees schema changes made through the Web API.
- **Alarms queue, then fire.** Alarms are pulled into a queue on the polling cycle but are not broadcast until the alarm actually sounds — so an alarm callback arrives at alarm time, not at poll time.

## Retry and suspension

| Setting | Default |
|---|---|
| `PollingIntervalSeconds` | 900 (15 min) |
| `PollingRetryIntervalSeconds` | 120 (2 min) |
| `RetryAttempts` | 3 |

After the retries are exhausted the webhook is **suspended**. It does not resume by itself.

- `Webhooks_Get_3F017BC6` — `GET /api/webhooks` — list registrations (OData-filterable). Poll this to detect a suspended hook.
- `Webhooks_Continue_511537A0` — `PUT /api/webhooks/{id}/continue` — resume it.
- `Webhooks_Suspend_A12B132D` — `PUT /api/webhooks/{id}/suspend` — pause deliveries during your own maintenance.

Build the suspension check into your health monitoring. A silently suspended webhook looks exactly like a quiet CRM.

## Manage

- `Webhooks_Get_8857FC56` — `GET /api/webhooks/{id}`
- `Webhooks_Patch_B8C243E8` — `PATCH /api/webhooks/{id}` — change the callback URL or query option.
- `Webhooks_Put_20B03A3C` — `PUT /api/webhooks/{id}` — full replace.
- `Webhooks_Delete_649F8293` — `DELETE /api/webhooks/{id}`

## Rotate the token

`Webhooks_PutTokenReset_C1A718D3` — `PUT /api/webhooks/reset-token`

Because the token is replayed on every delivery rather than used to sign one, rotate it on a schedule and on any suspected receiver compromise.

## Receiver checklist

- Compare the inbound `Authorization` header to the token you registered, in constant time.
- Serve HTTPS with a valid certificate so you never need Act!'s documented `http://` fallback.
- Be idempotent on your side. Act! retries, and a retry is indistinguishable from a new event — dedupe on the record `id` plus `edited`.
- Respond fast and do work asynchronously; a slow receiver burns retry attempts and ends in suspension.
