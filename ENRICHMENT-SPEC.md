# Partner Enrichment Specification

This document defines how FlytBase partners are enriched, how customers are identified, and how the 9-block output maps to fields in the Supabase `partners` table.

---

## How Enrichment Works

1. A Cowork session is opened and the partner enrichment skill is invoked with a partner name.
2. The skill pulls data from four sources in parallel: FlytForce, Argus, Circleback (embedded in FlytForce transcripts), and Gmail.
3. The skill identifies deployed customers using the 3-pattern method (see below).
4. The skill produces a 9-block profile.
5. Cowork writes the enrichment directly to Supabase using the service role key.
6. The dashboard reflects the update on next load — no manual steps.

**Before writing any enrichment, read `GUARDRAILS.md`.**

---

## The 4 Data Sources

| Source | What to pull | Tool |
|---|---|---|
| **FlytForce** | `search_accounts`, `get_account`, `get_account_meetings` (with `include_transcript: true`), `get_account_highlights` | FlytForce MCP |
| **Argus** | `search_everything` for the partner name, `get_deal` for any relevant deals | Argus MCP |
| **Circleback** | Embedded inside FlytForce meeting notes (source: circleback). Always pull full transcripts — summaries miss customer names and key context. | Via FlytForce |
| **Gmail** | `search_threads` for partner name and known contact email domains | Gmail MCP |

Never skip a source. Run all four in parallel.

---

## Customer Identification — The 3 Patterns

This is the most error-prone step. Apply all three patterns.

### Pattern 1 — Separate FlytForce org with ARR > 0
A customer-type account in FlytForce with its own `org_id` AND `ARR > 0`, representing a company that is clearly different from the partner itself.
**Result: Confirmed deployed customer.**

### Pattern 2 — Argus closed_won deal where end_customer ≠ partner name
An Argus deal in `closed_won` stage where the `end_customer` field names a company other than the partner.
**Result: Confirmed deployed customer.**

### Pattern 3 — Customer named in transcript only
A company mentioned in a meeting transcript as a site or client the partner is actively operating for, with no FlytForce org and no Argus closed deal.
**Result: Inferred deployed customer. Flag as unverified in the customers JSON.**

### Hard Rules

- Never list SPP or VAR deals as customers. These are partner program fees paid to FlytBase.
- Never list pipeline deals (contact_made, discovery, qualified) as deployed customers.
- Never trust account naming conventions alone. "Aerodyne_BPHB" or "Skyports_MPA" prove nothing — always cross-check org_id, ARR, and transcripts.
- Always read full transcripts, not just summaries. Pattern 3 customers only appear in transcript text.
- Sub-operator structures: if a partner governs sub-operators who deploy at end sites, the sub-operator's end customers are also the partner's deployed customers. Only surfaces in transcripts.
- On-prem deployments show $0 ARR in FlytForce — check Argus for closed_won deal value instead.

---

## The 9-Block Profile

### Block 1 · Identity
Factual background — who they are, where they operate.

| Field | Supabase column | Notes |
|---|---|---|
| HQ | `hq` | City, Country |
| Region | `region` | EU / Asia Pacific / North America / Africa / Latin America / Middle East / Oceania |
| Partner type | `partner_type` | VAR / Solution Partner / Drone Service Provider / Reseller |
| Tier | `tier` | T1 · Lighthouse / T2 · Gold / T3 · Bronze / — |
| Market approach | `market_approach` | Vertical specialist / Horizontal / Geographic specialist |
| Website | `website` | Domain only, no https:// |

Flag acquisitions, government transitions, and entity changes in the `notes` field.

---

### Block 2 · Strengths
What makes this partner distinctive.

| Field | Supabase column |
|---|---|
| Signature capability | `signature_capability` |
| Market position | `market_position` |
| Key differentiators | `key_differentiators` (array) |
| Partner voice | `partner_voice` |

---

### Block 3 · Commercial
Revenue and pipeline.

| Field | Supabase column | Notes |
|---|---|---|
| ARR | `arr` | Numeric only. This is the partner's own FlytBase subscription ARR — not customer revenue. |
| Docks | `docks` | Number of live docking stations |
| Pipeline value | `pipeline_value` | Sum of open pipeline deal values |
| Pipeline deals | `pipeline_deals` | JSONB array — see format below |

**pipeline_deals format:**
```json
[
  {"name": "Deal Name", "value": 50000, "stage": "Negotiation", "end_customer": "Company Name"}
]
```

---

### Block 4 · Proof
Deployed customers and proof evidence.

| Field | Supabase column | Values |
|---|---|---|
| Proof status | `proof_status` | `none` / `in_progress` / `verified` |
| Star story | `star_story` | 2-3 sentences, specific numbers and named customers only |
| Customers | `customers` | JSONB array — see format below |

**Proof status logic:**
- `none` — no deployments, no evidence
- `in_progress` — Pattern 3 (transcript-only) customers or deployment in progress
- `verified` — Pattern 1 or Pattern 2 confirmed customers

**customers format:**
```json
[
  {
    "name": "Customer Company",
    "vertical": "Security Services",
    "context": "3 docks, perimeter surveillance since Jan 2026",
    "source": "Pattern 2 — Argus closed_won"
  },
  {
    "name": "Another Customer",
    "vertical": "Public Safety",
    "context": "1 dock, pilot deployment",
    "source": "Pattern 3 — transcript only",
    "verified": false
  }
]
```

---

### Block 5 · DNA
Industry focus and proven capabilities.

| Field | Supabase column | Notes |
|---|---|---|
| Primary verticals | `primary_verticals` | Array. Use exact vertical names from GUARDRAILS.md only. |
| Secondary verticals | `secondary_verticals` | Array. Same list. |
| Proven use cases | `proven_use_cases` | Array of specific use cases evidenced in data |
| Proven geographies | `proven_geographies` | Array of countries |

---

### Block 6 · Captain
Relationship and contacts.

| Field | Supabase column |
|---|---|
| Key contact name | `key_contact_name` |
| Key contact role | `key_contact_role` |
| Additional contacts | `additional_contacts` (array of "Name · Role" strings) |
| Comm frequency | `comm_frequency` (Weekly / Monthly / Quarterly / Unknown) |
| Captain signal | `captain_signal` |
| Publicly advocated | `publicly_advocated` |

---

### Block 7 · Competitive
Competitive positioning.

| Field | Supabase column |
|---|---|
| Exclusivity | `exclusivity` |
| Sells competing platforms | `sells_competing_platforms` |
| Competitors encountered | `competitors_encountered` |

---

### Block 8 · Capability
Technical and marketing capabilities.

| Field | Supabase column |
|---|---|
| Technical depth | `technical_depth` (Low / Medium / High) |
| Technical depth evidence | `technical_depth_evidence` |
| Runs own marketing | `runs_own_marketing` |
| Needs co-marketing | `needs_co_marketing` |
| Languages | `languages` (array — evidence-only, never assumed) |

---

### Block 9 · Enablement
What FlytBase needs to do to help this partner grow.

| Field | Supabase column | Notes |
|---|---|---|
| Activation | `activation` | Investigate / Onboard / Activate / Grow — see logic below |
| Pillars needed | `pillars_needed` | Array of CAPS TAGS — only where FlytBase can genuinely add value |
| Vertical guides | `vertical_guides` | Array of vertical names |
| Decks to build | `decks_to_build` | Array of specific deck titles |
| Localization | `localization` | Array — evidence-only |
| Notes | `notes` | Free text for flags, risks, and context |

**Activation stage logic:**
- **Investigate** — No active deployments, low engagement, no ARR
- **Onboard** — First deployment in progress, just getting started
- **Activate** — Has deployments but engagement is inconsistent or shallow
- **Grow** — Active deployments, healthy comm frequency, open pipeline, engaged contacts

**Pillars Needed rule:** Only list areas where FlytBase has a specific capability to pitch or a genuine gap to fill for this partner. Never list things the partner already does well independently. Derive from active pain points, open pipeline, and feature requests in meeting notes.

---

## Activation Timestamp

Set `last_enriched` to the current timestamp whenever an enrichment is written to Supabase. The dashboard uses this to show when each partner was last updated.
