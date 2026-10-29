# Con Edison

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

Consolidated Edison Company of New York, Inc. (CECONY, trading as Con Edison) is the investor-owned electric, gas and steam utility that serves New York City and Westchester County, and together with its sibling Orange & Rockland Utilities (ORU) forms the regulated utility core of Consolidated Edison, Inc. It is a wires-and-pipes distribution utility rather than a competitive retailer: it owns and operates the distribution system, meters the customer and bills the customer, while wholesale energy markets are run by NYISO and competitive supply is sold by ESCOs. It is rate-regulated by the New York State Public Service Commission. Its API posture is the most interesting in the United States sample because both halves are real and they are gated completely differently. On the consumer side Con Edison runs a genuine, verified Green Button Connect My Data implementation — the NAESB REQ.21 ESPI standard, branded "Share My Data" — with a live production base URI at https://api.coned.com/gbc/espi/1_1 that answers anonymously with HTTP 401 "Unauthorized. Access token is missing or invalid.", a publicly downloadable 37-path Swagger 2.0 definition ("DCX GBC API V2"), a public Postman collection titled "GBC Certification Third party V3.3", OAuth 2.0 authorization-code and client-credentials flows, ESPI functional-block scopes, batch and real-time interval endpoints, and a 35-page technical onboarding document last revised 2026-05-07. The United States has no federal energy consumer data right; Con Edison's adoption sits under New York PSC supervision (the Joint Utilities DSIP proceeding, Case 16-M-0411, in which it committed to implement the first phase of GBC by end of 2017, the Data Access Framework in Case 20-M-0082, and a Customer Data Access Tariff) rather than under a statutory mandate like Australia's Consumer Data Right or Ontario Regulation 633/21. That data is not self-serve: a third party must register, sign a Data Security Agreement, submit a technical onboarding form and pass 30 to 60 days of supervised testing before production credentials are issued, and Con Edison states plainly that it "is unable to support API development for third parties." On the market side, by contrast, Con Edison publishes distribution-grid data openly and anonymously: the Hosting Capacity Map documents REST API access, and its ArcGIS feature services for segmented and network hosting capacity, EV transformer capacity, 33kV feeders, non-wires-solutions networks and disadvantaged communities all answer unauthenticated. Open on the grid, accredited on the customer — that split is the finding.

**Areas:** Energy · United States · New York · Utilities · Electricity · Gas · Steam · Smart Metering · Green Button · Energy Data · Grid · Distribution

## Con Edison on APIs.io

Everything profiled for Con Edison is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/con-edison/](https://apis.io/providers/con-edison/)** — or go straight to one of the APIs or properties below.

### APIs (16)

- [Con Edison Hosting Capacity Map REST API](https://apis.io/apis/con-edison/con-edison-hosting-capacity-map-rest-api/) — Con Edison's distribution-grid open data, published as anonymously readable Esri ArcGIS REST feature services behind the public Hosting Capacity Map. Con…
- [Con Edison Application Information API](https://apis.io/apis/con-edison/con-edison-applicationinformation-api/) — The ApplicationInformation API from Con Edison — 1 operation(s) for applicationinformation.
- [Con Edison Authorization API](https://apis.io/apis/con-edison/con-edison-authorization-api/) — The Authorization API from Con Edison — 3 operation(s) for authorization.
- [Con Edison Batch API](https://apis.io/apis/con-edison/con-edison-batch-api/) — The Batch API from Con Edison — 4 operation(s) for batch.
- [Con Edison Electric Power Usage Summary API](https://apis.io/apis/con-edison/con-edison-electricpowerusagesummary-api/) — The ElectricPowerUsageSummary API from Con Edison — 2 operation(s) for electricpowerusagesummary.
- [Con Edison Interval Block API](https://apis.io/apis/con-edison/con-edison-intervalblock-api/) — The IntervalBlock API from Con Edison — 2 operation(s) for intervalblock.
- [Con Edison Local Time Parameters API](https://apis.io/apis/con-edison/con-edison-localtimeparameters-api/) — The LocalTimeParameters API from Con Edison — 2 operation(s) for localtimeparameters.
- [Con Edison Meter Reading API](https://apis.io/apis/con-edison/con-edison-meterreading-api/) — The MeterReading API from Con Edison — 2 operation(s) for meterreading.
- [Con Edison Reading Type API](https://apis.io/apis/con-edison/con-edison-readingtype-api/) — The ReadingType API from Con Edison — 2 operation(s) for readingtype.
- [Con Edison Real Time Batch API](https://apis.io/apis/con-edison/con-edison-realtimebatch-api/) — The RealTimeBatch API from Con Edison — 3 operation(s) for realtimebatch.
- [Con Edison Real Time Interval Block API](https://apis.io/apis/con-edison/con-edison-realtimeintervalblock-api/) — The RealTimeIntervalBlock API from Con Edison — 2 operation(s) for realtimeintervalblock.
- [Con Edison Real Time Reading Type API](https://apis.io/apis/con-edison/con-edison-realtimereadingtype-api/) — The RealTimeReadingType API from Con Edison — 2 operation(s) for realtimereadingtype.
- [Con Edison Retail Customer API](https://apis.io/apis/con-edison/con-edison-retailcustomer-api/) — The RetailCustomer API from Con Edison — 9 operation(s) for retailcustomer.
- [Con Edison Usage Point API](https://apis.io/apis/con-edison/con-edison-usagepoint-api/) — The UsagePoint API from Con Edison — 2 operation(s) for usagepoint.
- [Green Button Download My Data](https://apis.io/apis/con-edison/green-button-download/) — Customer-driven file export that lets Con Edison residential and small commercial accounts download up to one year of smart-meter interval data as CSV or…
- [Con Edison Service Status API](https://apis.io/apis/con-edison/con-edison-service-status-api/) — The Service Status API from Con Edison — 1 operation(s) for service status.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Con Edison Batch Notification Webhooks](https://apis.io/asyncapis/con-edison/con-edison-batch-notification-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [con-edison-green-button](https://apis.io/servers/con-edison/con-edison-green-button/)

#### Agentic access (1)

Which operations an agent can call, which act, and which need a human in the loop.

- [Con Edison Agentic Access](https://apis.io/agentic-access/con-edison/con-edison-agentic-access/)

#### JSON Schema (1)

The data models the API reads and writes.

- [TokenRequest](https://apis.io/schemas/con-edison/con-edison-token-request/)

#### JSON-LD (1)

Linked-data contexts for the provider's vocabulary.

- [Con Edison Context](https://apis.io/jsonld/con-edison/con-edison-context/)

#### Collections (1)

Postman and Open Collection request collections.

- [DCX GBC API V2](https://apis.io/collections/con-edison/open-con-edison-green-button-connect-my-data-swagger/)

#### Overlays (1)

OpenAPI overlays, including conversational phrasing for each operation.

- [Con Edison Green Button Connect My Data Overlay](https://apis.io/overlays/con-edison/con-edison-green-button-connect-my-data-overlay/)

#### Governance rules (1)

Spectral rulesets for the provider's contracts.

- [Con Edison API Rules](https://apis.io/rules/con-edison/con-edison-rules/)

#### Security (2)

Authentication, trust centre, and vulnerability disclosure.

- [Con Edison Authentication](https://apis.io/security/con-edison/con-edison-authentication/)
- [Con Edison Domain Security](https://apis.io/security/con-edison/con-edison-domain-security/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Con Edison Scopes](https://apis.io/scopes/con-edison/con-edison-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Con Edison Plans Pricing](https://apis.io/plans/con-edison/con-edison-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Con Edison Rate Limits](https://apis.io/rate-limits/con-edison/con-edison-rate-limits/)

#### FinOps (1)

What the API costs to run, by operation.

- [Con Edison Finops](https://apis.io/finops/con-edison/con-edison-finops/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.coned.com/)
- [Documentation](https://www.coned.com/en/business-partners/access-customer-data)
- [Sign up](https://www.coned.com/en/accounts-billing/share-energy-usage-data/become-a-third-party/registration-form)
- [Blog](https://www.coned.com/en/about-us/media-center/news)
- [Terms of service](https://www.coned.com/en/accounts-billing/dashboard/billing-and-usage/terms-of-use)
- [Privacy policy](https://www.coned.com/en/privacy-policy-and-notice)

## API Evangelist rating

**Kin Score 54.4** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 64.3 |
| Contract quality | 49.5 |
| Contract governance | 35.6 |
| Operational transparency | 28.9 |
| Developer ergonomics | 70.4 |
| Access clarity | 55.3 |

**Agent readiness 25.2** — agent-aware

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | derived (partial credit) |
| Reversibility Documented | — |
| MCP Server | — |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | derived (partial credit) |
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
| Pagination Ratio | na |
| Operation Distinctiveness | — |

**Regulatory alignment 34.4** — Energy & Utilities

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/con-edison/](https://apis.io/providers/con-edison/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Con Edison: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/con-edison/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
