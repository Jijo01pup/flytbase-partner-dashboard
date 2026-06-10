# FlytBase Partner Intelligence Dashboard

A live dashboard showing the enrichment status and 9-block profile for all 96 FlytBase partners. Data lives in Supabase and is fetched fresh on every page load — no manual steps, no hosting required.

---

## How to open the dashboard

Double-click `index.html`. That's it. It fetches live data from Supabase automatically.

To share with the team: send them the `index.html` file. Anyone who opens it sees the same live data.

---

## How data gets in

When a partner is enriched in Cowork (using the partner enrichment skill), the enrichment is written directly to Supabase via the service role key. The next time anyone opens the dashboard, they see the updated profile.

No CSV. No Google Sheets. No manual steps.

---

## Files in this folder

| File | What it does |
|---|---|
| `index.html` | The live dashboard — open this to see all 96 partners |
| `CLAUDE.md` | Instructions for Claude — when anyone attaches this folder in Cowork, Claude automatically reads GUARDRAILS.md, writes to Supabase, and produces consistent 9-block enrichments |
| `GUARDRAILS.md` | FlytBase product names, terminology rules, and what NOT to write |
| `ENRICHMENT-SPEC.md` | The full 9-block enrichment spec, Supabase field mappings, and customer identification logic |
| `SCHEMA.md` | The Supabase `partners` table — every column, its type, and what goes in it |

---

## Supabase project

- **Project:** `flytbase-partners`
- **URL:** `https://dyehigwvpirujdiksese.supabase.co`
- **Table:** `partners`
- **Anon key** (read-only, safe to embed in HTML): in `index.html`
- **Service role key** (write access, Cowork only — never put in HTML): stored in `CLAUDE.md` for use in Cowork sessions

---

## Extending this system

The `partners` table in Supabase is the single source of truth for all partner enrichment data. Any tool that can make an authenticated HTTP request to the Supabase REST API can read or write partner data.

**To build an MCP on top of this:**
- Read: `GET /rest/v1/partners?select=*` with the anon key
- Write: `PATCH /rest/v1/partners?partner_name=eq.{name}` with the service role key
- Full schema: see `SCHEMA.md`

**To add a new field:**
- Add the column to the Supabase table (SQL Editor)
- Add it to the enrichment spec in `ENRICHMENT-SPEC.md`
- Update the dashboard render logic in `index.html`
