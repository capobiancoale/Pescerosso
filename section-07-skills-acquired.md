# 7 Skills acquired during the internship

This section describes the skills the internship developed, grouped as technical, methodological, and professional. Where the learning objectives of Section 3 stated what the internship set out to build, this section states what the work actually produced in terms of competence, with reference to the parts of the project that provided the evidence.

## 7.1 Technical skills

The internship built a working command of a cloud data warehouse. The author learned to model and construct the gold layer of a Snowflake warehouse for the student careers domain: writing the fact and dimension tables to a documented grain, applying the hash-key and ghost-record patterns for the foreign keys, deduplicating dimensions with `QUALIFY ROW_NUMBER()`, and guarding the ratios with `NULLIF` (Sections 5.3 and 5.6). These are the patterns of a governed gold layer, learned by applying them to real tables rather than by reading about them.

On top of the warehouse, the author learned semantic modelling and DAX in Power BI. This covered the design of a semantic model for a report domain, the management of relationships to avoid ambiguity, and the implementation of the KPIs of Section 5.4, including the more complex measures that encode a business rule and handle the empty case, such as `Fascia Studente CdS`. Reading a management indicator and expressing it as a correct, reusable DAX measure is the single most transferable technical skill the internship produced.

The author also gained familiarity with the analysis and testing artefacts that surround the build: the source-to-target mapping that documents how each target field derives from its source, and the data quality testing carried out in the User Acceptance Testing phase, including the automatic checks run with OpenMetadata and the functional checks run with the client (Section 5.6.2).

## 7.2 Methodological skills

The internship taught a way of working that starts from a management question and ends at a governed metric. The author learned to define a KPI by its business question, its grain, and the dimensions along which it is analysed before writing any code (Section 5.4.1), and to hold a definition in one place so that every report that uses it returns the same value.

A second methodological skill is validation. The author learned to test an analytical output against an external reference rather than against personal expectation: to compare a measure with the figure the client recognises as correct, and, when the two disagree, to trace the difference to a definition and reconcile it (Section 5.6.2). This is a habit of not trusting a number until it has been checked against something outside the model.

## 7.3 Professional skills

The internship was the author's first experience of consulting delivery, and it developed the skills that this setting requires. The author learned to work inside a delivery team, to conventions and naming standards set by more senior colleagues, and to have his work reviewed before it entered the shared model. Working to a shared standard, rather than to personal preference, is a professional skill in its own right.

The author also developed the ability to communicate about data with people who are not data specialists: to gather a reporting requirement from a directorate, to agree the definition of an indicator with the people who will use it, and to present a result in terms of the management question it answers rather than the query that produced it. The conversion of the internship into an apprenticeship reflects the level of professional integration reached by the end of the period.

---

## [TO CONFIRM] for this section

1. Skills content: these are drawn from the documented work. Do they match the skills you feel you actually acquired? Add, remove, or reword any.
2. Specific tools or skills to highlight: is there anything concrete I should name that is not yet here (for example Git or version control, a specific Snowflake feature, a BI or data certification, presentation or workshop facilitation)?
3. Length: this section is about one and a half pages. If you want it closer to two, I can expand the technical or the professional part with more concrete detail from the project.
