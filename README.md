# Act! CRM

<!-- API-EVANGELIST-PROVENANCE:BEGIN -->
> ### About this repository
>
> **This is not our API.** This repository is an independent, third-party profile of a company's
> **publicly available** API surface, maintained by [API Evangelist](https://apievangelist.com).
> API Evangelist does not operate, host, resell, or support this company's APIs, and is not
> affiliated with or endorsed by the company unless stated on the profile.
>
> **Where the information came from.** Everything here is assembled from material a member of the
> public can reach with a browser and no credentials — the company's own website, developer portal
> and documentation, the specifications it publishes for public use (OpenAPI, AsyncAPI, JSON Schema,
> `apis.json`, `llms.txt` and similar), its public repositories, and its public status, pricing and
> changelog pages. **Nothing here is obtained by breaching a system, defeating an access control, or
> using credentials of any kind.**
>
> **The rating is an independent assessment.** The Kin Score and Agent Readiness rating are
> independently calculated scores of a company's *public* API artifacts, produced by API Evangelist
> against a published rubric. They are not certifications, endorsements, security assessments, or
> audits, and they score published artifacts — not the quality, safety, or security of the software.
>
> **Corrections, re-scores, and removal are free.** No partnership, contract, or purchase is
> required, and you do not need to justify the request.
>
> - **Something wrong?** Open an issue on this repository, or email
>   [info@apievangelist.com](mailto:info@apievangelist.com).
> - **Published something new?** Ask for a re-score and we will re-run the rating.
> - **Want the listing taken down?** Say so and we will honor it. The profile is reduced to your
>   company name, a factual description, and a link to your own site, and the company is recorded as
>   **unrated** — never scored zero for having asked.
>
> **Response times.** Acknowledgement within **one business day**; removal or restriction within
> **two business days**; corrections and re-scores within **five business days**.
>
> **Not from the company, and here with a question?** You are welcome here — we would rather be the
> front line and point you the right way than have a good report go nowhere. What this repository
> can answer is narrow, though, so it is worth knowing who you are actually looking for:
>
> - **A question about how the API works, an account, billing, or a bug in the service** — that is
>   the company's own support, not us. We profile this API; we do not operate it and cannot see
>   your account.
> - **A bug in an open-source project we only catalog** — file it on that project's own repository.
>   This has happened with a real and correct bug report that reached us instead of the people who
>   could fix it, which helped nobody.
> - **Anything about this listing itself** — the description, the tags, the rating, a missing or
>   wrong artifact — is ours. Open an issue here.
> - **Not sure, or something general about API Evangelist or APIs.io** — open an issue on the
>   [APIs.io Inbox](https://github.com/api-search/inbox) and we will route it.
>
> **This repository contains no software, and we will never ask you to download anything.** There is
> no build, release, installer, or binary here — only text and machine-readable API descriptions, so
> there is nothing here that can be "corrupt" or need "repairing". Any issue, comment, or email
> claiming otherwise and offering a download link is not from us and is hostile. Do not follow the
> link; it is a lure. Report it to GitHub and, if you like, tell us at
> [info@apievangelist.com](mailto:info@apievangelist.com) so we can take it down.
>
> **On a security or compliance team?** Email
> [info@apievangelist.com](mailto:info@apievangelist.com) with *security* in the subject line and
> you will get a person, not a form. We will tell you exactly which public URLs this profile was
> built from so your team can see the same surface we did, and we will take the listing down on
> request while you work through it.
>
> Full detail: **[Where this data comes from](https://apievangelist.com/about/where-our-data-comes-from)**
<!-- API-EVANGELIST-PROVENANCE:END -->

Act! is a CRM and marketing automation platform built for small and mid-sized businesses, providing contact and activity management, opportunity tracking, email marketing, and pipeline reporting in cloud (Act! Advantage) or on-premises (Act! Premium Desktop) editions. The Act! Web API is a JSON-based REST API that exposes contacts, companies, groups, opportunities, activities, tasks, notes, history, documents and tenant-defined custom entities across 410 operations, with OData v4 query options ($filter, $orderby, $top, $skip, $select, $expand) on collection reads, multipart batching at POST /api/$batch, a webhook registration API, and a published Swagger 2.0 specification. Authentication is a JWT bearer token minted at GET /authorize from HTTP Basic credentials plus an Act-Database-Name header. The API is deployed per database — either on Act! Premium Cloud or on the customer's own IIS server — so the base URL is per-tenant.

**Areas:** CRM · Marketing Automation · Contact Management · Sales · Opportunity Management · OData · Small Business

## Act! CRM on APIs.io

Everything profiled for Act! CRM is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/act/](https://apis.io/providers/act/)** — or go straight to one of the APIs or properties below.

### APIs (11)

- [Act! Web API](https://apis.io/apis/act/web-api/) — JSON-based REST API for the Act! CRM database exposing contacts, companies, groups, opportunities, tasks, activity series, calendar, notes, history,…
- [Act! CRM Activity Series API](https://apis.io/apis/act/act-activityseries-api/) — The ActivitySeries API from Act! CRM — 2 operation(s) for activityseries.
- [Act! CRM History Types API](https://apis.io/apis/act/act-historytypes-api/) — The HistoryTypes API from Act! CRM — 5 operation(s) for historytypes.
- [Act! CRM Marketing Automations API](https://apis.io/apis/act/act-marketingautomations-api/) — The MarketingAutomations API from Act! CRM — 3 operation(s) for marketingautomations.
- [Act! CRM Metadata Info API](https://apis.io/apis/act/act-metadatainfo-api/) — The MetadataInfo API from Act! CRM — 14 operation(s) for metadatainfo.
- [Act! CRM Secondary Contacts API](https://apis.io/apis/act/act-secondarycontacts-api/) — The SecondaryContacts API from Act! CRM — 3 operation(s) for secondarycontacts.
- [Act! CRM Supplemental Files API](https://apis.io/apis/act/act-supplementalfiles-api/) — The SupplementalFiles API from Act! CRM — 8 operation(s) for supplementalfiles.
- [Act! CRM Custom Entities API](https://apis.io/apis/act/act-custom-entities-api/) — The Custom Entities API from Act! CRM — 2 operation(s) for custom entities.
- [Act! CRM Document Types API](https://apis.io/apis/act/act-document-types-api/) — The Document Types API from Act! CRM — 4 operation(s) for document types.
- [Act! CRM Sync Data API](https://apis.io/apis/act/act-sync-data-api/) — The Sync Data API from Act! CRM — 4 operation(s) for sync data.
- [Act! CRM Task Types API](https://apis.io/apis/act/act-task-types-api/) — The Task Types API from Act! CRM — 4 operation(s) for task types.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Act Webhooks](https://apis.io/asyncapis/act/act-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [act-mcp.yml](https://apis.io/servers/act/act-mcp-yml/)

#### Collections (31)

Postman and Open Collection request collections.

- [Act! Web API — ActivitySeries](https://apis.io/collections/act/open-act-activity-series-api/)
- [Act! Web API — Analytics](https://apis.io/collections/act/open-act-analytics-api/)
- [Act! Web API — Calendar](https://apis.io/collections/act/open-act-calendar-api/)
- [Act! Web API — Companies](https://apis.io/collections/act/open-act-companies-api/)
- [Act! Web API — Configurations](https://apis.io/collections/act/open-act-configurations-api/)
- [Act! Web API — Contacts](https://apis.io/collections/act/open-act-contacts-api/)
- [Act! Web API — Cors](https://apis.io/collections/act/open-act-cors-api/)
- [Act! Web API — CustomEntities](https://apis.io/collections/act/open-act-custom-entities-api/)
- [Act! Web API — Database](https://apis.io/collections/act/open-act-database-api/)
- [Act! Web API — DocumentTypes](https://apis.io/collections/act/open-act-document-types-api/)
- [Act! Web API — Documents](https://apis.io/collections/act/open-act-documents-api/)
- [Act! Web API — Geographics](https://apis.io/collections/act/open-act-geographics-api/)
- [Act! Web API — Groups](https://apis.io/collections/act/open-act-groups-api/)
- [Act! Web API — History](https://apis.io/collections/act/open-act-history-api/)
- [Act! Web API — HistoryTypes](https://apis.io/collections/act/open-act-history-types-api/)
- [Act! Web API — Import](https://apis.io/collections/act/open-act-import-api/)
- [Act! Web API — MarketingAutomations](https://apis.io/collections/act/open-act-marketing-automations-api/)
- [Act! Web API — MetadataInfo](https://apis.io/collections/act/open-act-metadata-info-api/)
- [Act! Web API — Notes](https://apis.io/collections/act/open-act-notes-api/)
- [Act! Web API — Opportunities](https://apis.io/collections/act/open-act-opportunities-api/)
- [Act! Web API — Preferences](https://apis.io/collections/act/open-act-preferences-api/)
- [Act! Web API — Products](https://apis.io/collections/act/open-act-products-api/)
- [Act! Web API — SecondaryContacts](https://apis.io/collections/act/open-act-secondary-contacts-api/)
- [Act! Web API — SupplementalFiles](https://apis.io/collections/act/open-act-supplemental-files-api/)
- [Act! Web API — SyncData](https://apis.io/collections/act/open-act-sync-data-api/)
- …and 6 more published on APIs.io

#### Overlays (32)

OpenAPI overlays, including conversational phrasing for each operation.

- [Act Activityseries Api Phrasing Overlay](https://apis.io/overlays/act/act-activityseries-api-phrasing-overlay/)
- [Act Analytics Api Phrasing Overlay](https://apis.io/overlays/act/act-analytics-api-phrasing-overlay/)
- [Act Calendar Api Phrasing Overlay](https://apis.io/overlays/act/act-calendar-api-phrasing-overlay/)
- [Act Companies Api Phrasing Overlay](https://apis.io/overlays/act/act-companies-api-phrasing-overlay/)
- [Act Configurations Api Phrasing Overlay](https://apis.io/overlays/act/act-configurations-api-phrasing-overlay/)
- [Act Contacts Api Phrasing Overlay](https://apis.io/overlays/act/act-contacts-api-phrasing-overlay/)
- [Act Cors Api Phrasing Overlay](https://apis.io/overlays/act/act-cors-api-phrasing-overlay/)
- [Act Custom Entities Api Phrasing Overlay](https://apis.io/overlays/act/act-custom-entities-api-phrasing-overlay/)
- [Act Database Api Phrasing Overlay](https://apis.io/overlays/act/act-database-api-phrasing-overlay/)
- [Act Document Types Api Phrasing Overlay](https://apis.io/overlays/act/act-document-types-api-phrasing-overlay/)
- [Act Documents Api Phrasing Overlay](https://apis.io/overlays/act/act-documents-api-phrasing-overlay/)
- [Act Geographics Api Phrasing Overlay](https://apis.io/overlays/act/act-geographics-api-phrasing-overlay/)
- [Act Groups Api Phrasing Overlay](https://apis.io/overlays/act/act-groups-api-phrasing-overlay/)
- [Act History Api Phrasing Overlay](https://apis.io/overlays/act/act-history-api-phrasing-overlay/)
- [Act Historytypes Api Phrasing Overlay](https://apis.io/overlays/act/act-historytypes-api-phrasing-overlay/)
- [Act Import Api Phrasing Overlay](https://apis.io/overlays/act/act-import-api-phrasing-overlay/)
- [Act Marketingautomations Api Phrasing Overlay](https://apis.io/overlays/act/act-marketingautomations-api-phrasing-overlay/)
- [Act Metadatainfo Api Phrasing Overlay](https://apis.io/overlays/act/act-metadatainfo-api-phrasing-overlay/)
- [Act Notes Api Phrasing Overlay](https://apis.io/overlays/act/act-notes-api-phrasing-overlay/)
- [Act Opportunities Api Phrasing Overlay](https://apis.io/overlays/act/act-opportunities-api-phrasing-overlay/)
- [Act Preferences Api Phrasing Overlay](https://apis.io/overlays/act/act-preferences-api-phrasing-overlay/)
- [Act Products Api Phrasing Overlay](https://apis.io/overlays/act/act-products-api-phrasing-overlay/)
- [Act Secondarycontacts Api Phrasing Overlay](https://apis.io/overlays/act/act-secondarycontacts-api-phrasing-overlay/)
- [Act Supplementalfiles Api Phrasing Overlay](https://apis.io/overlays/act/act-supplementalfiles-api-phrasing-overlay/)
- [Act Sync Data Api Phrasing Overlay](https://apis.io/overlays/act/act-sync-data-api-phrasing-overlay/)
- …and 7 more published on APIs.io

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Act Authentication](https://apis.io/security/act/act-authentication/)
- [Act Domain Security](https://apis.io/security/act/act-domain-security/)
- [Act Trust Center](https://apis.io/security/act/act-trust-center/)
- [Act Vulnerability Disclosure](https://apis.io/security/act/act-vulnerability-disclosure/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Act Plans Pricing](https://apis.io/plans/act/act-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Act Rate Limits](https://apis.io/rate-limits/act/act-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.act.com)
- [Documentation](https://www.act.com/developer/)
- [Getting started](https://www.act.com/resources/getting-started/act-cloud/)
- [API reference](https://apimta.act.com/act.web.api/)
- [Pricing](https://www.act.com/pricing/)
- [Sign up](https://www.act.com/trial/act/)
- [Support](https://support.act.com/)
- [Blog](https://www.act.com/blog/)
- [Terms of service](https://www.act.com/legal/terms-of-service/)
- [Privacy policy](https://www.act.com/legal/privacy-policy/)
- [GitHub](https://github.com/Swiftpage)
- [Status](https://status.act.com/)

## API Evangelist rating

**Kin Score 70.0** — exemplar · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 71.4 |
| Contract quality | 55.3 |
| Contract governance | 18.2 |
| Operational transparency | 60.5 |
| Developer ergonomics | 66.1 |
| Access clarity | 92.1 |

**Agent readiness 25.7** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | documented (partial credit) |
| OpenAPI Examples | — |
| Rate Limit Signal | documented (partial credit) |
| Event Surface Described | yes |
| Agent Skills | derived (partial credit) |
| Well Known Catalog | — |
| Consent Identity | — |
| Agent Card | — |
| Dry Run Mode | — |
| Delegated Identity | — |
| Protected Resource Metadata | — |
| Dynamic Client Registration | — |
| Agentic Commerce | — |
| Pagination Ratio | — |
| Operation Distinctiveness | mostly |

**Regulatory alignment 35.3** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/act/](https://apis.io/providers/act/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Act! CRM: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/act/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
