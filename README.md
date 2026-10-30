# Nasuni

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

Nasuni is an enterprise hybrid-cloud file data platform that consolidates distributed NAS and file servers into UniFS, a cloud-native global file system backed by customer-owned object storage (Azure Blob, Amazon S3, Google Cloud Storage and S3-compatible on-prem targets). Edge appliances cache hot data at each site while every version is written immutably to the cloud, delivering multi-site collaboration with Global File Lock and Global File Acceleration, continuous versioning, rapid ransomware recovery, and file data services for analytics and AI. Nasuni exposes a substantial programmable surface: the REST Nasuni Management Console (NMC) API for fleet, volume, share, snapshot and appliance management; the multi-region Nasuni Portal API for account, IAM, Ops IQ telemetry, UaaS and job orchestration; the Global File Acceleration Telemetry API; Nasuni Data Service (NDS) endpoints that present Nasuni volumes through Azure Blob- and Amazon S3-compatible interfaces; the Nasuni Data API for appliance filesystem access; and a hosted, OAuth 2.0 Portal MCP Server plus open-source local MCP servers that make the platform callable by AI agents.

**Areas:** Company · File Storage · Hybrid Cloud · Object Storage · Enterprise Storage · Data Management · Backup and Recovery · Ransomware Protection · Infrastructure · Observability · MCP · Agent-Native

## Nasuni on APIs.io

Everything profiled for Nasuni is published on APIs.io rather than in this repository. Start at the provider page — **[apis.io/providers/nasuni/](https://apis.io/providers/nasuni/)** — or go straight to one of the APIs or properties below.

### APIs (29)

- [Nasuni Data API](https://apis.io/apis/nasuni/nasuni-data-api/) — HTTP API providing programmatic access to the filesystem of a Nasuni Edge Appliance — create, read, update and delete files and folders, list shares, and…
- [Nasuni Portal MCP Server](https://apis.io/apis/nasuni/nasuni-portal-mcp-server/) — Hosted, OAuth 2.0-authenticated Model Context Protocol server that exposes the Nasuni Portal to AI clients (Claude, ChatGPT, Microsoft Copilot Studio) as…
- [Nasuni Account API](https://apis.io/apis/nasuni/nasuni-account-api/) — The Account API from Nasuni — 4 operation(s) for account.
- [Nasuni Account Management API](https://apis.io/apis/nasuni/nasuni-account-management-api/) — The Account Management API from Nasuni — 3 operation(s) for account management.
- [Nasuni Activity Logs API](https://apis.io/apis/nasuni/nasuni-activity-logs-api/) — The Activity Logs API from Nasuni — 2 operation(s) for activity logs.
- [Nasuni AI Activate API](https://apis.io/apis/nasuni/nasuni-ai-activate-api/) — The AI Activate API from Nasuni — 5 operation(s) for ai activate.
- [Nasuni Auth API](https://apis.io/apis/nasuni/nasuni-auth-api/) — The Auth API from Nasuni — 5 operation(s) for auth.
- [Nasuni Cloud Credentials API](https://apis.io/apis/nasuni/nasuni-cloud-credentials-api/) — The Cloud Credentials API from Nasuni — 5 operation(s) for cloud credentials.
- [Nasuni Configuration API](https://apis.io/apis/nasuni/nasuni-configuration-api/) — The Configuration API from Nasuni — 5 operation(s) for configuration.
- [Nasuni Data API](https://apis.io/apis/nasuni/nasuni-data-api/) — The Data API from Nasuni — 1 operation(s) for data.
- [Nasuni Edge Appliances (Edge) API](https://apis.io/apis/nasuni/nasuni-edge-appliances-edge-api/) — The Edge Appliances (Edge) API from Nasuni — 4 operation(s) for edge appliances (edge).
- [Nasuni File IQ Appliances (File IQ) API](https://apis.io/apis/nasuni/nasuni-file-iq-appliances-file-iq-api/) — The File IQ Appliances (File IQ) API from Nasuni — 2 operation(s) for file iq appliances (file iq).
- [Nasuni Filers API](https://apis.io/apis/nasuni/nasuni-filers-api/) — The Filers API from Nasuni — 30 operation(s) for filers.
- [Nasuni Global File Accelerator (GFA) API](https://apis.io/apis/nasuni/nasuni-global-file-accelerator-gfa-api/) — The Global File Accelerator (GFA) API from Nasuni — 2 operation(s) for global file accelerator (gfa).
- [Nasuni Health API](https://apis.io/apis/nasuni/nasuni-health-api/) — The Health API from Nasuni — 1 operation(s) for health.
- [Nasuni IAM API](https://apis.io/apis/nasuni/nasuni-iam-api/) — The IAM API from Nasuni — 13 operation(s) for iam.
- [Nasuni Index API](https://apis.io/apis/nasuni/nasuni-index-api/) — The Index API from Nasuni — 1 operation(s) for index.
- [Nasuni Jobs API](https://apis.io/apis/nasuni/nasuni-jobs-api/) — The Jobs API from Nasuni — 3 operation(s) for jobs.
- [Nasuni Messages API](https://apis.io/apis/nasuni/nasuni-messages-api/) — The Messages API from Nasuni — 2 operation(s) for messages.
- [Nasuni Data Service (NDS) API](https://apis.io/apis/nasuni/nasuni-nasuni-data-service-nds-api-api/) — The Nasuni Data Service (NDS) API API from Nasuni — 3 operation(s) for nasuni data service (nds) api.
- [Nasuni Data Service (NDS) AWS API](https://apis.io/apis/nasuni/nasuni-nasuni-data-service-nds-aws-api-api/) — The Nasuni Data Service (NDS) AWS API API from Nasuni — 2 operation(s) for nasuni data service (nds) aws api.
- [Nasuni Notifications API](https://apis.io/apis/nasuni/nasuni-notifications-api/) — The Notifications API from Nasuni — 3 operation(s) for notifications.
- [Nasuni Ops IQ API](https://apis.io/apis/nasuni/nasuni-ops-iq-api/) — **Requires Premium Ops IQ license** (`ops-iq-premium`). The Ops IQ Telemetry API is available only to accounts with the Premium Ops IQ license bit enabled.…
- [Nasuni Portal API](https://apis.io/apis/nasuni/nasuni-portal-api-api/) — The Portal API API from Nasuni — 1 operation(s) for portal api.
- [Nasuni Serials API](https://apis.io/apis/nasuni/nasuni-serials-api/) — The Serials API from Nasuni — 3 operation(s) for serials.
- [Nasuni Software API](https://apis.io/apis/nasuni/nasuni-software-api/) — The Software API from Nasuni — 2 operation(s) for software.
- [Nasuni UniFS As A Service (UaaS) API](https://apis.io/apis/nasuni/nasuni-unifs-as-a-service-uaas-api/) — The UniFS As A Service (UaaS) API from Nasuni — 10 operation(s) for unifs as a service (uaas).
- [Nasuni Volume Connections API](https://apis.io/apis/nasuni/nasuni-volume-connections-api/) — The Volume Connections API from Nasuni — 1 operation(s) for volume connections.
- [Nasuni Volumes API](https://apis.io/apis/nasuni/nasuni-volumes-api/) — The Volumes API from Nasuni — 52 operation(s) for volumes.

### Published properties

Each property below is a page on APIs.io carrying the machine-readable artifact and what it says.

#### AsyncAPI (1)

Event-driven and webhook contracts.

- [Nasuni File Iq Alerts Webhooks](https://apis.io/asyncapis/nasuni/nasuni-file-iq-alerts-webhooks/)

#### MCP servers (1)

Model Context Protocol servers that expose the API to agents.

- [Nasuni MCP servers](https://apis.io/servers/nasuni/nasuni-mcp-servers/)

#### JSON Schema (1)

The data models the API reads and writes.

- [Nasuni File IQ Alert Webhook Payload](https://apis.io/schemas/nasuni/nasuni-file-iq-alert-webhook-payload.schema/)

#### Overlays (7)

OpenAPI overlays, including conversational phrasing for each operation.

- [Nasuni Global File Acceleration Telemetry Overlay](https://apis.io/overlays/nasuni/nasuni-global-file-acceleration-telemetry-overlay/)
- [Nasuni Nasuni Data Service Aws Overlay](https://apis.io/overlays/nasuni/nasuni-nasuni-data-service-aws-overlay/)
- [Nasuni Nasuni Data Service Azure Overlay](https://apis.io/overlays/nasuni/nasuni-nasuni-data-service-azure-overlay/)
- [Nasuni Nmc V1 0 Overlay](https://apis.io/overlays/nasuni/nasuni-nmc-v1-0-overlay/)
- [Nasuni Nmc V1 1 Overlay](https://apis.io/overlays/nasuni/nasuni-nmc-v1-1-overlay/)
- [Nasuni Nmc V1 2 Overlay](https://apis.io/overlays/nasuni/nasuni-nmc-v1-2-overlay/)
- [Nasuni Portal V0 Overlay](https://apis.io/overlays/nasuni/nasuni-portal-v0-overlay/)

#### Security (4)

Authentication, trust centre, and vulnerability disclosure.

- [Nasuni Authentication](https://apis.io/security/nasuni/nasuni-authentication/)
- [Nasuni Domain Security](https://apis.io/security/nasuni/nasuni-domain-security/)
- [Nasuni Trust Center](https://apis.io/security/nasuni/nasuni-trust-center/)
- [Nasuni Vulnerability Disclosure](https://apis.io/security/nasuni/nasuni-vulnerability-disclosure/)

#### OAuth scopes (1)

The permissions the API asks for.

- [Nasuni Scopes](https://apis.io/scopes/nasuni/nasuni-scopes/)

#### Plans and pricing (1)

Published plans, tiers and prices.

- [Nasuni Plans Pricing](https://apis.io/plans/nasuni/nasuni-plans-pricing/)

#### Rate limits (1)

Published request limits.

- [Nasuni Rate Limits](https://apis.io/rate-limits/nasuni/nasuni-rate-limits/)

## Where to find this API

Links to the company's own pages. This repository publishes no copy of their specifications — the canonical document is theirs and lives at the URLs below.

- [Website](https://www.nasuni.com/)
- [Documentation](https://docs.nasuni.com/)
- [Getting started](https://docs.nasuni.com/docs/nasuni-quick-start-guide)
- [API reference](https://docs.api.nasuni.com/api/nmc/v120/reference/overview/)
- [Pricing](https://www.nasuni.com/pricing/)
- [Sign up](https://portal.nasuni.com/)
- [Support](https://www.nasuni.com/customers/support/)
- [Blog](https://www.nasuni.com/blog/)
- [Trust centre](https://trust.nasuni.com/)
- [Terms of service](https://www.nasuni.com/legal/master-subscription-and-services-agreement-2026/)
- [Privacy policy](https://www.nasuni.com/legal/privacy-notice-2026/)
- [GitHub](https://github.com/nasuni-labs)

## API Evangelist rating

**Kin Score 64.9** — strong · rubric 0.25.0 · scored 2026-10-23

| Facet | Score |
|---|---|
| Discoverability | 76.7 |
| Contract quality | 56.6 |
| Contract governance | 18.2 |
| Operational transparency | 57.9 |
| Developer ergonomics | 66.1 |
| Access clarity | 84.2 |

**Agent readiness 36.7** — agent-ready

| Agent-readiness dimension | Result |
|---|---|
| Spec Presence | yes |
| Agentic Access | — |
| Reversibility Documented | documented (partial credit) |
| MCP Server | documented (partial credit) |
| Auth Clarity | bearer (partial credit) |
| Idempotency | — |
| Error Semantics | verified |
| OpenAPI Examples | partial |
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
| Operation Distinctiveness | distinct |

**Regulatory alignment 34.3** — Horizontal (data, software, accessibility, platform)

The rubric is published at [github.com/api-evangelist/kin-score](https://github.com/api-evangelist/kin-score). The per-check breakdown behind these numbers — which checks were earned, partial or missed — along with score history and how this provider compares to its peers, is available at [apis.io/providers/nasuni/](https://apis.io/providers/nasuni/).

## How this profile is made

API Evangelist profiles a company's public API surface the way a developer would find it: the website, developer portal and documentation, the specifications it publishes, its public repositories, and its pricing, status and changelog pages. From that it derives the properties above — contracts split per API, workflows, schemas, collections, governance rules, plans and rate limits — and scores them against the published [Kin Score rubric](https://github.com/api-evangelist/kin-score).

The artifacts are published on [APIs.io](https://apis.io), not in this repository. This repository is the public record for Nasuni: the place to say something is wrong, missing or out of date, and to ask for a re-score or a removal.

## Open an issue

- **Something about this listing** — a wrong description, a missing API, a stale artifact or a question about the rating: [open an issue on this repository](https://github.com/api-evangelist/nasuni/issues/new).
- **A new API, or one that is not in the catalog yet** — [submit it on APIs.io](https://apis.io/add/).
- **Anything else** — [the APIs.io inbox](https://github.com/api-search/inbox/issues/new) or [info@apievangelist.com](mailto:info@apievangelist.com).
