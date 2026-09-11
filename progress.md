# Thesis progress tracker

Last update: Sections 1 (introduction) and 8 (conclusions) drafted, confirmed, and pushed. All thesis sections (1–8) are now drafted, about 34–35 pages. Next step: assembly and formatting into the final Word document (title page, abstract, table of contents, references), plus clearing the remaining minor TO CONFIRM items.

---

## Overall status

| Section | Status | Estimated pages | File |
|---|---|---|---|
| 1. Introduction | **Draft v1** | ~2 | `section-01-introduction.md` |
| 2. Hosting company: KPMG | **Draft v1** | ~2 | `section-02-hosting-company.md` |
| 3. Internship learning objectives | **Draft v1** | ~1.5 | `section-03-learning-objectives.md` |
| 4. Problem definition | **Draft v1** | ~3.5 | `section-04-problem-definition.md` |
| 5.1 Overall approach | **Draft v1** | ~0.7 | `section-05-1-overall-approach.md` |
| 5.2 Data architecture (medallion) | **Draft v1** | ~2.7 | `section-05-2-data-architecture.md` |
| 5.3 Semantic model | **Draft v1** | ~4 | `section-05-3-semantic-model.md` |
| 5.4 KPI design for student careers | **Draft v1 (overrun accepted)** | ~5–6 | `section-05-4-kpi-design.md` |
| 5.5 Power BI implementation | **Draft v1** | ~2 | `section-05-5-power-bi-implementation.md` |
| 5.6 Data quality considerations | **Draft v2 (UAT + real cases)** | ~3 | `section-05-6-data-quality.md` |
| 6. Discussion: before vs after | **Draft v1** | ~3–4 | `section-06-discussion.md` |
| 7. Skills acquired | **Draft v1** | ~1.5 | `section-07-skills-acquired.md` |
| 8. Conclusions | **Draft v1** | ~1.5 | `section-08-conclusions.md` |

**Drafted so far**: ~34–35 pages (all sections 1–8)
**Target total**: 30–40 pages
**Remaining budget**: ~11–22 pages across sections not yet started

⚠️ **Budget note**: Section 5.4 runs ~5-6 pages against a 3-4 local target. Globally the thesis is at ~20-21 pages against a 30-40 target, so the real pressure is to reach 30, not to stay under 40. TO CONFIRM #19 resolved: the author accepted the 5.4 overrun and the full catalogue is kept (families 4-6 included). No trim.

---

## Open [TO CONFIRM] items across all drafted sections

### Section 2 (Hosting company: KPMG) — NEW
29. KPMG figures (143 countries / 275,000 globally; ~5,000 professionals / ~25 offices / >6,000 clients in Italy) — keep, or drop for a purely qualitative overview?
30. Role progression wording (internship of three months, extended by two, then converted into an apprenticeship) — correct?
31. Naming KPMG Advisory S.p.A. (Milan) — keep, or generic ("KPMG Advisory in Italy")?
32. Precise team or service-line name within Advisory, if the author wants one used (anonymised if needed). Not invented.

### Section 3 (Internship learning objectives) — NEW
33. Objectives content — do the reconstructed technical/methodological/professional/academic objectives match what the author set out to learn? Add, remove, or reword.
34. Anything explicitly required at the start (a specific technology, a certification, a deliverable) to add as an objective.

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
12. RESOLVED — the author worked mainly in the gold layer; §5.3 attribution now credits the author with the student careers gold tables plus the semantic model and DAX. The trade-off emerged in the author's own design work with team validation.
13. Refresh cadence "target lag of one day" — verified against reality?
14. "Client confirmed acceptable" about lack of filter sync — formally discussed with the client, or client simply didn't complain?

### Section 5.4 (KPI design)
15. Selection process for KPIs — iterative with first list from academic management refined via review cycles? Or more top-down (KPMG proposed → client approved)?
16. Thresholds 11% and 22% for the traffic-light attendance risk band — formal policy of the client or defined during the project with client validation?
17. RESOLVED — validation was against client-provided reference values, reconciled directly with the client where they did not match (see §5.6.2). §5.4.1 wording updated.
18. Display folders — I mention only `Iscrizione` and `Rendimento`. Are there other folders you use elsewhere?
19. RESOLVED — the author accepted the overrun. Section 5.4 is kept in full, families 4-6 included.

### Section 5.5 (Power BI implementation) — NEW
20. **Number of thematic reports** — how many reports in the app? From the .bim files I see at least didattica and servizi studenti; are there other reports (research output, economic-financial)?
21. **Report names** — do I use actual report names (like `Modello_Semantico_didattica`) or keep generic?
22. **Page names** — do actual pages have specific titles I should mention?
23. **Drill-down path** — I described institution → programme → cohort → student. Is that actually the navigation path?
24. **Access rights mechanism** — I wrote "audience within the application" (standard for Power BI apps). Is that how it's actually configured, or via workspace roles / Azure AD groups / row-level security?
25. **Slicers list** — I mention "academic year, cycle, gender" as examples. Real main slicers, or generalise?

### Section 6 (Discussion) — NEW
26. Before/after example scoped as "for the information the dashboards cover" rather than "everything automated". Framing OK?
27. §6.4 maintainer of the pipeline and the semantic model after delivery — KPMG on an ongoing basis, an internal university IT/BI team, or a mix? Currently phrased generically as "whoever maintains them".
28. §6.6 managerial implications — agree with the three points, in particular that the largest return is in recurring, shared, governance-heavy indicators?

### Section 7 (Skills acquired) — NEW
35. Skills content — do the technical/methodological/professional skills match what you feel you acquired? Add, remove, or reword.
36. Specific tool, feature, or certification to name (e.g. Git, Snowflake Dynamic Tables/Streams/Tasks, dbt, a Snowflake or Power BI certification, workshop facilitation)?

---

## Next steps (recommended order)

1. **Assemble and format the final document** — consolidate Sections 1–8 into one Word file with the required formatting (TNR 12 pt, 1.5 spacing, 2.5 cm margins, justified, A4, one-sided), and add the title page, abstract, table of contents, and reference list.
2. **Clear remaining minor TO CONFIRM items** — mostly minor wording confirmations (Section 4 role names, Section 5.2 batch time and ingestion tooling, Section 5.5 report and page names, Section 6 maintainer of the pipeline, Sections 3 and 7 content).
3. **Final formatting pass and proofread** before submission.

---

## Working conventions in this session

- **Language**: English (professional academic register), per Master's guidelines.
- **Formatting for final Word doc**: TNR 12pt, 1.5 spacing, 2.5 cm margins, justified, A4, one-sided (still to apply — currently drafts are Markdown for editability).
- **Citations**: Bayesian Analysis journal convention. Author-year in-text (Kimball and Ross, 2013), full entries in `references-bib.md`.
- **Humanizer skill**: applied automatically to every draft.
- **Confidentiality**: names/paths/procedures/measure names in source materials are treated as already anonymised at source (per Alessio's explicit confirmation).
- **Source materials (11 Sept 2026)**: the author provided real project artifacts (Source-to-Target mapping `STT`, bronze-silver mapping, table descriptions, UAT documentation, kick-off deck). The author and his KPMG manager cleared using project names and data. They are used at the structural and process level. Rules applied: individual student identifiers and personal records are not reproduced (aggregate figures only, e.g. the nine deleted careers); the KPMG team is named per the author's instruction; UniSR client individuals are referred to by role only; the kick-off deck is marked "KPMG Confidential", so contact details and internal confidential specifics are excluded.
- **Authorial honesty**: contribution boundaries between Alessio and the KPMG team are stated explicitly in the relevant sections.
- **Persistence note**: In this Claude Code session the assistant has write access to the GitHub repository `capobiancoale/Pescerosso` and pushes each confirmed file directly to the branch `claude/awesome-ride-pfilsf`. The earlier `Fiorebianco` / no-write-connector arrangement described in the master prompt does not apply in this environment.

---

## Files in the repo (drafts folder)

- `progress.md` (this file)
- `README-repo-setup.md` — how to push to GitHub
- `references-bib.md` — running bibliography
- `section-01-introduction.md`
- `section-02-hosting-company.md`
- `section-03-learning-objectives.md`
- `section-04-problem-definition.md`
- `section-05-1-overall-approach.md`
- `section-05-2-data-architecture.md`
- `section-05-3-semantic-model.md`
- `section-05-4-kpi-design.md`
- `section-05-5-power-bi-implementation.md`
- `section-05-6-data-quality.md`
- `section-06-discussion.md`
- `section-07-skills-acquired.md`
- `section-08-conclusions.md` ← NEW this session
