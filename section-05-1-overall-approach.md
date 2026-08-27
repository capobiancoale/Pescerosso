# 5. Research methods and analysis

## 5.1 Overall approach

The research presented in this chapter is applied and design-oriented. It documents the design and implementation of the Business Intelligence solution developed for Università Vita-Salute San Raffaele, and analyses it against the pre-existing reporting model discussed in Chapter 4.

The end-to-end architecture has four layers, illustrated in Figure X:

1. ESSE3 as the operational source system.
2. A Snowflake data platform, organised according to the medallion pattern (bronze, silver, and gold layers), which ingests the data extracted from ESSE3 and progressively refines it.
3. A semantic model built in Power BI on top of the gold layer, which defines the entities, relationships, and DAX measures that underlie the KPI catalogue.
4. A set of Power BI dashboards published through Power BI Service and distributed to the client as a Power BI application, with access rights differentiated by user role.

Two clarifications on authorship are needed before the sections that follow. The semantic model and the DAX measures for the student careers domain were designed and implemented directly by the author, in collaboration with the KPMG delivery team; the team contributed to the overall architecture, to the design of the fact tables, and to the review of the modelling choices. The Snowflake medallion layers were built by the wider team, and the author's involvement in that part of the pipeline was limited to consuming the gold layer and to aligning with the team on the data structures it made available.

The remainder of the chapter follows the architecture in order. Section 5.2 describes the Snowflake medallion approach at a level of detail proportionate to its role in the thesis. Section 5.3 presents the semantic model, including the trade-offs adopted when a pure star-schema design was not feasible. Section 5.4 details the KPI catalogue for the student careers domain, Section 5.5 illustrates the Power BI implementation and its distribution model, and Section 5.6 discusses the data quality considerations that emerged during the project.

---

## [TO CONFIRM] for this section

1. Authorship attribution: semantic model + DAX measures (student careers domain) = you; Snowflake medallion + fact table design + review of modelling choices = KPMG team. Correct?
2. Power BI distribution: dashboards published via Power BI Service, distributed as Power BI app with access rights by user role. Correct? (I avoided saying "row-level security" because I don't know if that's the mechanism used or if it's access control at workspace/app level.)
