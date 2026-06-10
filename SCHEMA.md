# Supabase Schema — `partners` Table

**Project:** flytbase-partners  
**URL:** https://dyehigwvpirujdiksese.supabase.co  
**Table:** `public.partners`

RLS is enabled. Anon key = SELECT only. Service role key = full access.

---

## Columns

| Column | Type | Description |
|---|---|---|
| `id` | BIGSERIAL | Auto-incrementing primary key |
| `partner_name` | TEXT UNIQUE NOT NULL | Canonical partner name — must match exactly |
| `last_enriched` | TIMESTAMPTZ | When the enrichment was last written |
| `updated_at` | TIMESTAMPTZ | Auto-set on every write |

### Block 1 — Identity
| Column | Type | Description |
|---|---|---|
| `hq` | TEXT | City, Country |
| `region` | TEXT | EU / Asia Pacific / North America / Africa / Latin America / Middle East / Oceania |
| `partner_type` | TEXT | VAR / Solution Partner / Drone Service Provider / Reseller |
| `tier` | TEXT | T1 · Lighthouse / T2 · Gold / T3 · Bronze / — |
| `market_approach` | TEXT | Vertical specialist / Horizontal / Geographic specialist |
| `website` | TEXT | Domain only |

### Block 2 — Strengths
| Column | Type | Description |
|---|---|---|
| `signature_capability` | TEXT | One sentence — what they are best known for |
| `market_position` | TEXT | One sentence — where they sit in the market |
| `key_differentiators` | TEXT[] | Array of differentiator strings |
| `partner_voice` | TEXT | One sentence in the partner's own voice |

### Block 3 — Commercial
| Column | Type | Default | Description |
|---|---|---|---|
| `arr` | NUMERIC | 0 | Partner's own FlytBase subscription ARR |
| `docks` | INTEGER | 0 | Number of live docking stations |
| `pipeline_value` | NUMERIC | 0 | Total open pipeline value |
| `pipeline_deals` | JSONB | [] | Array of deal objects — see ENRICHMENT-SPEC.md |

### Block 4 — Proof
| Column | Type | Default | Description |
|---|---|---|---|
| `proof_status` | TEXT | 'none' | none / in_progress / verified |
| `star_story` | TEXT | | 2-3 sentences, specific numbers and named customers only |
| `customers` | JSONB | [] | Array of deployed customer objects — see ENRICHMENT-SPEC.md |

### Block 5 — DNA
| Column | Type | Description |
|---|---|---|
| `primary_verticals` | TEXT[] | From the 12-vertical fixed list only |
| `secondary_verticals` | TEXT[] | From the 12-vertical fixed list only |
| `proven_use_cases` | TEXT[] | Specific use cases evidenced in data |
| `proven_geographies` | TEXT[] | Countries where partner has active deployments |

### Block 6 — Captain
| Column | Type | Description |
|---|---|---|
| `key_contact_name` | TEXT | Primary contact name |
| `key_contact_role` | TEXT | Primary contact title/role |
| `additional_contacts` | TEXT[] | Additional contacts as "Name · Role" strings |
| `comm_frequency` | TEXT | Weekly / Monthly / Quarterly / Unknown |
| `captain_signal` | TEXT | 2-3 sentences on engagement quality |
| `publicly_advocated` | TEXT | Yes / No / Not confirmed + context |

### Block 7 — Competitive
| Column | Type | Description |
|---|---|---|
| `exclusivity` | TEXT | Exclusive / Unknown |
| `sells_competing_platforms` | TEXT | Yes / No / Unknown |
| `competitors_encountered` | TEXT | Competitor names and context |

### Block 8 — Capability
| Column | Type | Description |
|---|---|---|
| `technical_depth` | TEXT | Low / Medium / High |
| `technical_depth_evidence` | TEXT | One sentence of evidence |
| `runs_own_marketing` | TEXT | Yes / No / Unknown |
| `needs_co_marketing` | TEXT | Yes / No / Cold + context |
| `languages` | TEXT[] | Evidence-only — never assumed from country |

### Block 9 — Enablement
| Column | Type | Description |
|---|---|---|
| `activation` | TEXT | Investigate / Onboard / Activate / Grow |
| `pillars_needed` | TEXT[] | CAPS TAGS for areas where FlytBase can add value |
| `vertical_guides` | TEXT[] | Relevant vertical guide names |
| `decks_to_build` | TEXT[] | Specific deck titles |
| `localization` | TEXT[] | Languages for localization |
| `notes` | TEXT | Flags, risks, entity changes, context |

---

## REST API Reference

**Base URL:** `https://dyehigwvpirujdiksese.supabase.co/rest/v1`

**Headers:**
```
apikey: <anon_key>           # for reads
apikey: <service_role_key>   # for writes
Authorization: Bearer <key>
Content-Type: application/json
```

**Read all partners (alphabetical):**
```
GET /partners?select=*&order=partner_name.asc
```

**Read one partner:**
```
GET /partners?select=*&partner_name=eq.Aerodyne
```

**Write (upsert) a partner enrichment:**
```
PATCH /partners?partner_name=eq.Aerodyne
Body: { "arr": 45000, "activation": "Grow", ... }
```

**Filter by activation stage:**
```
GET /partners?activation=eq.Grow&select=partner_name,arr,docks
```

**Filter enriched partners only:**
```
GET /partners?last_enriched=not.is.null&select=*
```

---

## RLS Policies

| Policy | Role | Permission |
|---|---|---|
| `public_read` | anon | SELECT — anyone with the anon key can read |
| `service_write` | service_role | ALL — full read/write/delete |
