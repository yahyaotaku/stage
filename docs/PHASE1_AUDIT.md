# Phase 1 — Audit of the starter project

Files inspected: `generer.py`, `candidatures.xlsx` (3 sheets, 200 data rows),
`modeles/cv.tex`, `modeles/lettre.tex`, `modeles/mail.txt`,
`modeles/relance1.txt`, `modeles/relance2.txt`, and `Yahya Ennaouiti CV.pdf`.
The original `.rar` was provided as loose files; nothing was modified.

## What the starter project is

A single-script, Excel-driven application generator built for **Vadim
Guignard**, a second-year EILCO student targeting **German internships**
(`Pflichtpraktikum`, 12–14 weeks, from May 2027), with documents written in
**English**.

Pipeline:

```
candidatures.xlsx (Candidatures sheet, status = "À préparer")
        │  generer.py
        ├─ writes vars.tex per company (LaTeX \def variables)
        ├─ pdflatex → cv.pdf + lettre.pdf (per-company folder in sorties/)
        ├─ pypdf merge → Candidature_<company>.pdf
        ├─ Mail_<company>.txt from modeles/mail.txt (string.Template)
        └─ updates Excel status → "Prêt"
```

Extras: `--liste` (text dashboard), `--relances` (follow-up due dates from
`Statut` + `Date d'envoi`, configurable delays in the `Paramètres` sheet:
7 and 14 days), `--entreprise` (regenerate one company).

## Existing features

- Per-company output folders with CV, letter, merged PDF, mail file
- LaTeX escaping (`& % $ # _ { } ~ ^ \`) and filename slugging
- `pdflatex` detection with graceful fallback (keeps `.tex` sources and
  points to MiKTeX/TeX Live/Overleaf)
- Follow-up scheduling with status transitions
  (Envoyé → Relancé 1 → Relancé 2) and late-day reporting
- Data-quality guardrail: refuses to generate a letter with an empty
  "Accroche" (personalized hook) in batch mode
- Status guardrail: regenerating an already-sent application does not reset
  its status
- Excel formulas for follow-up dates and "Action du jour"

## Bugs, limitations and risks

1. **Wrong person / wrong target.** All templates hardcode Vadim Guignard's
   name, phone, email, LinkedIn, GitHub, TOEIC score, German plans and the
   12–14-week May-2027 `Pflichtpraktikum` frame. None of it applies to
   Yahya Ennaouiti.
2. **Wrong language and country.** Documents are English; target is Germany.
   The new application must produce **French** documents for **French
   companies**.
3. **Excel as the only database.** No relational integrity, no duplicate
   detection, no history, no backups, no concurrent-safe writes, and the
   script fails when the workbook is open in Excel (correctly detected, but
   still a dead end).
4. **No research engine.** The 200 German companies were compiled manually;
   nothing discovers or verifies companies or offers.
5. **No scoring.** Compatibility is a single human-written "Accroche" field.
6. **No UI, no tests, no requirements.txt, no packaging.**
7. Template logic (`\Profile` web/data switch) is specific to Vadim's CV and
   does not generalize.
8. Minor: `write_vars` silently defaults City to "Germany"; dates are
   formatted in English inside (future) French documents; merged-PDF step
   silently skipped when `pypdf` is missing (documented only in console).

## What is reusable (design ideas, not code-as-is)

- The **vars.tex injection pattern** (template + generated variable file) is
  a clean way to keep LaTeX templates stable — kept for the new CV/letter
  engine.
- **latex_escape / slug / date parsing** utilities — reimplemented with type
  hints and tests.
- **Follow-up due-date logic** and status guardrails — carried into the
  follow-up service with configurable intervals.
- **pdflatex-missing fallback** — carried into the LaTeX service.
- The **"no empty hook" rule** — generalized: no document is generated from
  incomplete data without an explicit warning.
- The Excel workbook structure (status list, parameter sheet, how-to sheet)
  — superseded by SQLite, but preserved as an **export/reporting** format.

## Verdict

The starter is a good personal automation script but not a foundation for
the requested application. Its pipeline concepts are reused; its hardcoded
personal data, German targeting, Excel-as-database design and lack of
research/scoring/UI are all replaced.
