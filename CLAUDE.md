# FlytBase Partner Intelligence — Instructions for Claude

This folder contains the FlytBase partner enrichment system. When someone asks you to enrich a partner, follow these instructions exactly. These instructions override any default behaviour.

---

## When asked to enrich a partner

1. Read `GUARDRAILS.md` in this folder before writing anything. Apply every rule in it — especially product names (Verkos, not WorkOS or Workhorse; Flinks, not Flink; etc.) and the ARR rules.
2. Read `ENRICHMENT-SPEC.md` for the full 9-block format, the 3-pattern customer identification method, and the Supabase field mappings.
3. Run the enrichment using all four data sources in parallel: FlytForce, Argus, Circleback (inside FlytForce transcripts), Gmail.
4. Write the completed enrichment to Supabase using the credentials below.
5. The dashboard (`index.html`) fetches from Supabase on every load — no further action needed.

---

## Supabase credentials

**Project URL:** `https://dyehigwvpirujdiksese.supabase.co`
**Anon key (read-only):** `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImR5ZWhpZ3d2cGlydWpkaWtzZXNlIiwicm9sZSI6ImFub24iLCJpYXQiOjE3ODEwODAxNTIsImV4cCI6MjA5NjY1NjE1Mn0.yrdF7gAgURzQ12OrnbYFdC4lJ5oBiH29vkxr9tRxIFA`
**Service role key (write access):** `eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJzdXBhYmFzZSIsInJlZiI6ImR5ZWhpZ3d2cGlydWpkaWtzZXNlIiwicm9sZSI6InNlcnZpY2Vfcm9sZSIsImlhdCI6MTc4MTA4MDE1MiwiZXhwIjoyMDk2NjU2MTUyfQ.HkPF8T-vJTh4Vr2vnoIZUvvhPg0fMojVD-K6RSa70u4`

Use the service role key for all writes. Use the Supabase MCP `execute_sql` tool with project ID `dyehigwvpirujdiksese`.

---

## How to write an enrichment to Supabase

After completing the 9-block profile, run an UPDATE on the `partners` table. Match on `partner_name` exactly (case-sensitive — use the name as it appears in the table).

```sql
UPDATE partners SET
  last_enriched = NOW(),
  hq = '...',
  region = '...',
  partner_type = '...',
  tier = '...',
  market_approach = '...',
  website = '...',
  revenue = 0,
  docks = 0,
  pipeline_value = 0,
  activation = '...',
  primary_verticals = ARRAY['...'],
  secondary_verticals = ARRAY['...'],
  proven_use_cases = ARRAY['...'],
  proven_geographies = ARRAY['...'],
  key_contact_name = '...',
  key_contact_role = '...',
  additional_contacts = ARRAY['Name · Role'],
  comm_frequency = '...',
  captain_signal = '...',
  publicly_advocated = '...',
  exclusivity = '...',
  sells_competing_platforms = '...',
  competitors_encountered = '...',
  technical_depth = '...',
  technical_depth_evidence = '...',
  runs_own_marketing = '...',
  needs_co_marketing = '...',
  languages = ARRAY['English'],
  signature_capability = '...',
  market_position = '...',
  key_differentiators = ARRAY['...'],
  partner_voice = '...',
  star_story = '...',
  proof_status = 'none',
  customers = '[{"name":"...","vertical":"...","context":"...","source":"Pattern 2 — Argus closed_won"}]',
  pipeline_deals = '[{"name":"...","value":0,"stage":"...","end_customer":"..."}]',
  pillars_needed = ARRAY['...'],
  vertical_guides = ARRAY['...'],
  decks_to_build = ARRAY['...'],
  localization = ARRAY['English'],
  notes = '...',
  updated_at = NOW()
WHERE partner_name = 'Exact Partner Name Here';
```

For any field where no data was found, use `NULL` (not an empty string). For array fields with no data, use `NULL` or omit the field.

---

## Non-negotiable rules (always apply)

- Never use em dashes in any output
- Never count a partner's own FlytBase subscription ARR as customer revenue
- Never count SPP/VAR deals as deployed customers
- Never count pipeline deals as deployed customers
- Never assume languages from a country — only write what is evidenced in calls or emails
- Always use the exact vertical names from GUARDRAILS.md — no variations
- Always read full Circleback transcripts in FlytForce meetings, not just summaries
- Product names: Verkos (never WorkOS, Workhorse), Flinks (never Flink), AI-R, FlytBase One
- In speech-to-text transcripts, "dogs" always means "docks"

---

## Files in this folder

| File | Purpose |
|---|---|
| `GUARDRAILS.md` | Product names, terminology, ARR rules, writing rules |
| `ENRICHMENT-SPEC.md` | 9-block spec, field mappings, customer identification logic |
| `SCHEMA.md` | Full Supabase table schema and REST API reference |
| `README.md` | Project overview and how to extend the system |
| `index.html` | The live dashboard — open to view all 96 partners |
