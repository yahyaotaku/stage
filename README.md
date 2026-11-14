# Internship Manager — stage

Personal internship-search assistant for **Ennaouiti Yahya**, ING 2 Computer
Engineering student at **EILCO (Calais, France)**.

Target: **Stage Assistant Ingénieur en Informatique — ING 2**, around
**June 7 – September 8–10, 2027**, preferred duration **8 weeks**
(longer offers up to ~14 weeks kept as possible matches).

## Current contents

| Path | Description |
|---|---|
| `data/french_companies.csv` | Seed dataset of **61 real French companies** ordered by search priority (see below). UTF-8 with BOM, opens directly in Excel. |
| `data/generate_companies_csv.py` | Reproducible generator for the CSV (distances computed, not guessed). |
| `docs/PHASE1_AUDIT.md` | Audit of the original starter project (`generer.py` + `candidatures.xlsx` + LaTeX templates). |

## Search priorities

1. **Calais area** — Calais, Coquelles, Marck, Boulogne-sur-Mer,
   Dunkerque/Gravelines, Saint-Omer area, plus the wider Pas-de-Calais /
   Nord littoral (Lestrem) and the Lille metropole.
2. **Ris-Orangis area** — Essonne and Île-de-France.
3. **Other France** — representative large technical employers.

## Data honesty rules (enforced in the dataset)

- Only **real companies**; no fabricated contact details.
  `contact_email` and `contact_phone` are **empty everywhere** — they must be
  verified by the research engine or by you before any use.
- Website URLs are filled only where they are well-known public facts.
- Distances are **approximate straight-line km** computed from city
  coordinates (great-circle). Travel times are intentionally absent.
- Every row is marked `Unverified - public general knowledge, verify before
  use`. Nothing here is a confirmed internship offer.
- `internship_type` distinguishes *spontaneous application candidates* from
  *companies known to run internship programs* (current offers still to be
  verified).

## Regenerate the CSV

```bash
python3 data/generate_companies_csv.py
```

## Roadmap

1. ~~Phase 1 — audit of the starter project~~ (see `docs/PHASE1_AUDIT.md`)
2. Phase 2 — architecture & implementation plan
3. Phase 3 — core application (SQLite database, profile, company management,
   application tracking, dashboard; works offline, no AI required)
4. Phase 4 — document generation (French CV, lettre de motivation, email,
   LaTeX → PDF)
5. Phase 5 — research engine (published offers + spontaneous candidates,
   source verification, duplicate detection, transparent scoring)
6. Phase 6 — modular AI integration (optional, free fallback included)
7. Phase 7 — email: manual sending by default, optional guarded auto-send
8. Phase 8 — tests and QA
