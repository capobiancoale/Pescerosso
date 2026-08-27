# Thesis progress tracker

Last update: session with Claude, section 5.3 completed.

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
| 5.4 KPI design for student careers | Not started | 3–4 | — |
| 5.5 Power BI implementation | Not started | 2–3 | — |
| 5.6 Data quality considerations | Not started | 1–2 | — |
| 6. Discussion: before vs after | Not started | 3–4 | — |
| 7. Skills acquired | Not started | 1–2 | — |
| 8. Conclusions | Not started | 1–2 | — |

**Drafted so far**: ~11 pages
**Target total**: 30–40 pages
**Remaining budget**: ~19–29 pages across the sections not yet started

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
14. "Client confirmed acceptable" about lack of filter sync — formally discussed with the client, or client simply didn't complain? (If the latter, reformulate as "no complaints raised".)

---

## Next steps (recommended order)

1. **Answer the [TO CONFIRM] above** — even partial answers help, especially the authorship ones (5, 12), which affect the honesty of the thesis.
2. **Section 5.4 (KPI design)** — will need you to tell me which KPIs you actually implemented (time-to-degree, exam pass rate, dropout indicators, credits per semester, etc.) so I don't invent metrics you never calculated.
3. **Section 5.5 (Power BI implementation)** — dashboard structure, distribution model, screenshots (anonymised) if you have any.
4. **Section 5.6 (Data quality)** — will build partly on §5.3.4, plus concrete quality issues encountered on ESSE3 data.
5. **Section 6 (Discussion)** — natural once §5 is complete.
6. **Sections 2, 3, 7** — shorter sections about KPMG, objectives, skills. Write once substance is in place.
7. **Sections 1, 8** — introduction and conclusions written LAST, so they reflect what the thesis actually became.

---

## Working conventions in this session

- **Language**: English (professional academic register), per Master's guidelines.
- **Formatting for final Word doc**: TNR 12pt, 1.5 spacing, 2.5 cm margins, justified, A4, one-sided (still to apply — currently drafts are Markdown for editability).
- **Citations**: Bayesian Analysis journal convention. Author-year in-text (Kimball and Ross, 2013), full entries in `references-bib.md`.
- **Humanizer skill**: applied automatically to every draft (avoids AI writing patterns: em dash overuse, rule-of-three, superficial -ing constructions, "landscape/underscore/leverage/crucial", copula avoidance, etc.).
- **Confidentiality**: names/paths/procedures in the source materials are treated as already anonymised at source (per Alessio's explicit confirmation). No further re-anonymisation is applied.
- **Authorial honesty**: contribution boundaries between Alessio and the KPMG team are stated explicitly in the relevant sections. No fabrication of details that were not supplied.
