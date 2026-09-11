# Thesis progress tracker

Last update: Section 5.5 completed.

---

## Overall status

| Section | Status | Estimated pages | File |
|---|---|---|---|
| 1. Introduction | Not started | 2–3 | — |
| 2. Hosting company: KPMG | Not started | 2–3 | — |
| 3. Internship learning objectives | Not started | 1–2 | — |
| 4. Problem definition | **Draft v1** | ~3.5 | `section-04-problem-definition.md` |
| 5.1 Overall approach | **Draft v1** | ~0.7 | `section-05-1-overall-approach.md` |
| 5.2 Data architecture (medallion) | **Draft v1** | ~2.7 | `section-05-2-data-architecture.md` |
| 5.3 Semantic model | **Draft v1** | ~4 | `section-05-3-semantic-model.md` |
| 5.4 KPI design for student careers | **Draft v1 (OVER BUDGET)** | ~5–6 | `section-05-4-kpi-design.md` |
| 5.5 Power BI implementation | **Draft v1** | ~2 | `section-05-5-power-bi-implementation.md` |
| 5.6 Data quality considerations | Not started | 1–2 | — |
| 6. Discussion: before vs after | Not started | 3–4 | — |
| 7. Skills acquired | Not started | 1–2 | — |
| 8. Conclusions | Not started | 1–2 | — |

**Drafted so far**: ~18–19 pages
**Target total**: 30–40 pages
**Remaining budget**: ~11–22 pages across sections not yet started

⚠️ **Budget note**: Section 5.4 is still over budget (~5-6 pages vs 3-4 target). Section 5.5 was kept compact (~2 pages) to partially compensate. Still need decision on TO CONFIRM #19 (whether to trim 5.4 or accept the overrun).

---

## Open [TO CONFIRM] items across all drafted sections

### Section 4 (Problem definition)
1. San Raffaele as "Italian private university" — correct?
2. ESSE3 described as "operational platform widely adopted by Italian HEIs" — if you know vendor (CINECA/Kion), can be more precise.
3. "Program coordinators, departmental leadership" as examples — if more precise role names exist (without naming individuals), adapt.
4. "Developed during the internship" phrasing implies active participation — correct?

### Section 5.1 (Overall approach)
5. Authorship: semantic model + DAX (student careers) = you; Snowflake medallion + fact design + review = team. Correct?
6. Power BI distribution: via Power BI Service, distributed as Power BI app with access rights by user role. Correct? (Not row-level security?)

### Section 5.2 (Data architecture)
7. Nightly batch time — specify "~21:00 CET" or keep generic?
8. Add a simplified SQL Listing 1 showing the SCD Type 2 upsert pattern? Or keep it conceptual only?
9. Include specific number "39 silver tasks" or keep generic?
10. **[DA CHIEDERE AL TEAM KPMG]** Tool used for ingestion ESSE3 → Snowflake bronze (e.g., Azure Data Factory, Fivetran, custom script), and refresh cadence.

### Section 5.3 (Semantic model)
11. UGOV: include as example of "other operational systems" or restrict student careers domain to ESSE3 only?
12. Attribution of the wide fact table trade-off: emerged during **your** design work with team validation, or was it proposed by senior team and implemented by you?
13. Refresh cadence "target lag of one day" — verified against reality?
14. "Client confirmed acceptable" about lack of filter sync — formally discussed with the client, or client simply didn't complain?

### Section 5.4 (KPI design)
15. Selection process for KPIs — iterative with first list from academic management refined via review cycles? Or more top-down (KPMG proposed → client approved)?
16. Thresholds 11% and 22% for the traffic-light attendance risk band — formal policy of the client or defined during the project with client validation?
17. Validation via "manually reconstructed reference values" — is that how you actually validated?
18. Display folders — I mention only `Iscrizione` and `Rendimento`. Are there other folders you use elsewhere?
19. Page budget overrun in 5.4 (~5-6 pages vs 3-4 target). Options: (a) accept overrun and compensate in 5.5/5.6 (already partially compensated in 5.5), (b) shorten by removing 1-2 zoom sub-sections, (c) shorten by removing families 4-5-6 from Table 1. Preference?

### Section 5.5 (Power BI implementation) — NEW
20. **Number of thematic reports** — how many reports in the app? From the .bim files I see at least didattica and servizi studenti; are there other reports (research output, economic-financial)?
21. **Report names** — do I use actual report names (like `Modello_Semantico_didattica`) or keep generic?
22. **Page names** — do actual pages have specific titles I should mention?
23. **Drill-down path** — I described institution → programme → cohort → student. Is that actually the navigation path?
24. **Access rights mechanism** — I wrote "audience within the application" (standard for Power BI apps). Is that how it's actually configured, or via workspace roles / Azure AD groups / row-level security?
25. **Slicers list** — I mention "academic year, cycle, gender" as examples. Real main slicers, or generalise?

---

## Next steps (recommended order)

1. **Answer TO CONFIRM items** — prioritise #5, #12, #15, #16, #19, #24 (authorship, key design choices, budget, technical accuracy of access model).
2. **Section 5.6 (Data quality)** — quality issues encountered on ESSE3 data, how the medallion + semantic model addressed them, residual limitations. Will build on §5.3.4 and connect back to §4.3.
3. **Section 6 (Discussion)** — natural once §5 is complete. Compares before/after using the four dimensions of §4.4. §5.5.4 already anticipates part of this.
4. **Sections 2, 3, 7** — shorter sections about KPMG, objectives, skills.
5. **Sections 1, 8** — introduction and conclusions written LAST.

---

## Working conventions in this session

- **Language**: English (professional academic register), per Master's guidelines.
- **Formatting for final Word doc**: TNR 12pt, 1.5 spacing, 2.5 cm margins, justified, A4, one-sided (still to apply — currently drafts are Markdown for editability).
- **Citations**: Bayesian Analysis journal convention. Author-year in-text (Kimball and Ross, 2013), full entries in `references-bib.md`.
- **Humanizer skill**: applied automatically to every draft.
- **Confidentiality**: names/paths/procedures/measure names in source materials are treated as already anonymised at source (per Alessio's explicit confirmation).
- **Authorial honesty**: contribution boundaries between Alessio and the KPMG team are stated explicitly in the relevant sections.
- **Persistence note**: I save each new section as a Markdown file and present it for download. I cannot push to GitHub directly (no write connector available in this chat). Alessio pushes manually from his own machine.

---

## Files in the repo (drafts folder)

- `progress.md` (this file)
- `README-repo-setup.md` — how to push to GitHub
- `references-bib.md` — running bibliography
- `section-04-problem-definition.md`
- `section-05-1-overall-approach.md`
- `section-05-2-data-architecture.md`
- `section-05-3-semantic-model.md`
- `section-05-4-kpi-design.md`
- `section-05-5-power-bi-implementation.md` ← NEW this session
