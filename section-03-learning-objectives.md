# 3 Internship learning objectives

The internship took place inside Project Kaleidos, the programme through which Università Vita-Salute San Raffaele built a new university reporting model on a centralised data platform (Sections 1 and 2). The project ran in phases: a discovery phase that mapped the existing situation and collected the target requirements, a select-and-design phase that chose the technology stack and produced the source-to-target analysis and the solution design, a development phase that integrated the source systems, built the platform, and validated it through User Acceptance Testing, and the go-live and adoption phases that followed. The author's work concentrated in the select-and-design and development phases.

The learning objectives that oriented the internship fall into four groups: technical, methodological, professional, and academic. This section states them and connects each to the phase and the part of the project in which it was pursued. Sections 5 to 7 show how far they were met.

## 3.1 Technical objectives

The first objective was to learn a modern cloud data stack in practice rather than in theory. This meant working with Snowflake as the data warehouse, with the medallion architecture that organises the flow from raw to reporting-ready data, and with the patterns that make a gold layer usable by a reporting tool (Sections 5.2 and 5.3). In the project, this warehouse is the Data Hub that the development phase set out to build.

The second was to learn dimensional and semantic modelling for Business Intelligence: the design of fact and dimension tables, the discipline of an explicit grain, the relationships that carry filters through a model, and the translation of a warehouse into a Power BI semantic model (Section 5.3).

The third was to reach a level of DAX sufficient to implement measures that are not trivial, including measures that encode a business rule and handle the empty or edge case rather than failing on it (Section 5.4). Taken together, these objectives describe a move from a theoretical knowledge of data management to the ability to build a working reporting layer on real data.

## 3.2 Methodological objectives

One objective was to learn to translate a management question into a data model and a measure: to begin from what a manager needs to know and arrive at a KPI with an agreed grain and a single definition (Sections 5.4 and 5.6.2). The source-to-target analysis of the select-and-design phase, which maps each target field back to its source, is where this translation begins in a structured form.

A second was to learn to validate analytical output. The development phase closed with a User Acceptance Testing exercise, organised in automatic technical checks and functional checks carried out with the client (Section 5.6.2). Taking part in this, and reconciling a KPI definition with the client when the computed figure and the reference figure disagreed, taught the discipline of validating against an external benchmark rather than against one's own expectation.

A third was to absorb the discipline of a governed model: definitions that live in one place, a grain that is documented rather than assumed, and data quality safeguards built into the tables (Sections 5.3.4 and 5.6.3).

## 3.3 Professional objectives

The internship was also an opportunity to learn how a consulting engagement is delivered. The project ran with a defined governance. On the KPMG side it was led by the associate partners G. Palumbo and F. De Cassai, with change management directed by M. Lodigiani (Senior Manager) and the delivery run by a core team in which F. Scorpiniti (Manager) and A. Pessolano (Assistant Manager) led the technical work and M. Vanoli (Assistant Manager) the functional work. The author worked inside this technical team, to conventions set by more senior colleagues, with the author's work reviewed before it entered the shared model (Sections 2.3 and 5.3). On the client side, the project was sponsored at the level of the delegated councillor and steered by the university's directorates, among them the Operations Directorate, the Information Systems Directorate, and the Directorate for Teaching Development, Accreditation and Quality.

A related objective was to learn to communicate with a client and with people who are not technical: to gather requirements, to agree the definition of an indicator, and to hand over a reporting product that the client can use without a technical intermediary for each question. The requirement-gathering and the UAT sessions, both run with the university's directorates, were where this was practised. Behind these sits a simpler objective, to work to the standards and the pace of a professional services firm, which the conversion of the internship into an apprenticeship reflects.

## 3.4 Academic objectives

The academic objective was to connect the Master's programme in Data Science for Management to a real management setting, and to see how the methods studied in the programme apply to the decisions of an institution. The second academic objective was to produce the documented case study that this thesis is: a description and an analysis of a real Business Intelligence transformation, carried out with the rigour the Master requires.

---

## [TO CONFIRM] for this section

1. RESOLVED — confirmed by the author: the work was in the select-and-design and development phases only.
2. RESOLVED — the author asked to name the KPMG team; the client (UniSR) individuals are referred to by role only, and no contact details are included (the kick-off deck is marked KPMG Confidential).
3. Objectives content: these are reconstructed from the role and the documented work. Do they match what you set out to learn? Add, remove, or reword any.
4. Anything you were explicitly asked to focus on at the start (a specific technology, a certification, a deliverable) that should be stated here as an objective?
