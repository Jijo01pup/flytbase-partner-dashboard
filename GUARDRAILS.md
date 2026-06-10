# FlytBase Enrichment Guardrails

Rules and terminology that apply to every partner enrichment, every piece of content, and every dashboard entry. Read this before writing anything about FlytBase or its partners.

---

## FlytBase Product Names — Canonical List

Use only these names. Never invent alternatives.

| Correct name | What it is | Never write |
|---|---|---|
| **FlytBase** | The core enterprise drone autonomy platform | Flytbase, flytbase, FlyBase |
| **Verkos** | FlytBase's AI agent framework — "Detect Anything" agents that analyze live drone video and telemetry to detect events and trigger automated actions | WorkOS, Workhorse, Workhorse AI, Verkos AI (just "Verkos" is fine) |
| **AI-R** | FlytBase's edge AI hardware device — sits next to the dock, enables real-time on-prem AI analytics | AIR, A.I.R, ai-r |
| **Flinks** | FlytBase's library of add-ons and integrations. Comes in two flavors: (1) **Add-ons** — pre-built FlytBase apps covering fleet maintenance, site intelligence, scheduling, automation, and incident response; (2) **Integrations** — connectors to third-party tools like PIX4D, DroneDeploy, Genetec, Milestone, Esri, and 38+ others. Named add-ons include: Akara (photogrammetry/3D mapping), Forensic Search (AI visual search for footage), Pilot Shift Scheduler, Aero-Flow Control (workflow automation), Live Incidence Response, FlytBase Prediq (predictive fleet maintenance), Flyt-AIS (maritime vessel tracking), Battery Analytics, Coverage Intelligence, FlytBase Insights. | Flink (singular is wrong for the product), plugins |
| **FlytBase One** | Unified Fleet Management (UFM) system — connects drones, docks, robots, sensors, and cameras into one operating layer. Launched Feb 2026. | FlytBase 1 |
| **FlytBase Academy** | Partner and customer training and certification platform | |
| **FlytBase Pro** | Enterprise-grade software license tier | |

---

## Internal Tool Names

These are FlytBase's internal systems. Use them correctly in enrichment notes — they are not customer-facing product names.

| Name | What it is |
|---|---|
| **FlytForce** | FlytBase's customer success and account management CRM |
| **Argus** | FlytBase's sales pipeline and deal tracking CRM |
| **Circleback** | Meeting transcription tool — its notes are embedded inside FlytForce meeting records |
| **NestGen** | FlytBase's partner community event series (NestGen Retreat, NestGen Detroit, etc.) |

---

## Partner Program Terminology

| Term | Meaning |
|---|---|
| **SPP** | Standard Partner Program — the standard tier for FlytBase solution partners |
| **VAR** | Value Added Reseller — a reseller model |
| **T1 / Lighthouse** | Top partner tier |
| **T2 / Gold** | Second partner tier |
| **T3 / Bronze** | Entry partner tier |
| **LUC** | Light UAS Operator Certificate — EU regulatory authorization enabling BVLOS operations |
| **BVLOS** | Beyond Visual Line of Sight — drone operations beyond the pilot's direct line of sight |
| **DaaS** | Drone as a Service — partners who sell drone operations as a managed service |
| **Dock** | The hardware unit (DJI Dock 2, Dock 3, etc.) that houses, charges, and deploys the drone autonomously |

---

## The 12 Industry Verticals — Exact Spelling

Use only these words when writing primary or secondary verticals. No variations.

1. Security Services
2. Mining Operations
3. Electric Utilities
4. Public Safety
5. Solar Operations
6. Oil and Gas
7. Maritime Ports
8. Railroad Operations
9. Corrections & Detention
10. Data Centers
11. Transportation & Highways
12. Construction

---

## ARR Rules

- **Never count a partner's own FlytBase subscription ARR as customer revenue.** If the only ARR in FlytForce belongs to the partner's own org, that is what they pay FlytBase — not what their customers pay.
- **Never count an SPP or VAR deal as a deployed customer.** These are partner program fees paid to FlytBase.
- **Only count a company as a deployed customer if it meets Pattern 1, 2, or 3** (see `ENRICHMENT-SPEC.md`).
- On-prem deployments (like MOI Qatar) show $0 ARR in FlytForce — revenue is tracked as project payments in Argus. This does not mean the customer has no value.

---

## Writing Rules

- Never use em dashes (—) in content written for the marketing team or partners
- Never suggest MDF (Marketing Development Funds) management as an activity
- Partner marketing scope is: market-entry materials and event presence only. Never include email sequences, LinkedIn targeting, or demand-gen campaigns.
- Languages: only fill in languages that are explicitly evidenced in calls or emails. Never assume based on country.
- Star Stories: specific numbers and named customers only. No vague claims like "significant growth" or "strong results."
- Proof Status: only mark as "verified" if there are named, live deployments with operational evidence (Pattern 1 or Pattern 2). Transcript-only customers (Pattern 3) = "in_progress."

---

## Common Errors to Avoid

| Wrong | Right |
|---|---|
| WorkOS | Verkos |
| Workhorse | Verkos |
| Workhorse AI | Verkos |
| "docks" transcribed as "dogs" in speech-to-text | always interpret "dogs" as "docks" |
| Listing SPP/VAR renewal deals as customers | These are partner fees, not customer deployments |
| Listing pipeline deals as deployed customers | Pipeline = prospect. Only closed deployments count. |
| Assuming languages from country | Only write what's evidenced in calls/emails |
