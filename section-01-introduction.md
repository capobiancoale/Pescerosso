# 1 Introduction

## 1.1 Data and decisions in higher education

Universities manage a set of interrelated activities: admissions, student progression, teaching, research, and the allocation of financial resources. Decisions in these areas increasingly draw on data held in the institution's own operational systems rather than on periodic reports or informal knowledge. In a systematic review of Business Intelligence adoption in higher education, Sequeira et al. (2024) note that decisions taken without the support of systematised institutional data can compromise both the effectiveness and the efficiency of institutional action.

The difficulty is that the systems which hold this data are built for transactions, not for analysis. A student information system records enrolments, exams, and careers one operation at a time. It does not provide, on its own, the aggregated and cross-domain views that academic management needs to monitor a cohort, compare programmes, or decide where to intervene. Closing this gap is the problem that a Business Intelligence layer addresses.

## 1.2 Setting: an internship at KPMG and a project for Università Vita-Salute San Raffaele

This thesis is based on the work carried out during an internship at KPMG Advisory, on a project for Università Vita-Salute San Raffaele. The project, named Kaleidos, was the programme through which the university set out to build a new reporting model on a centralised data platform: a Data Hub that integrates the university's operational data in a Snowflake warehouse, and a reporting layer built in Power BI on top of it (Section 2).

The project covers several domains of academic management. This thesis focuses on one of them, student careers and academic performance, which is the most information-intensive area of academic management and the domain in which the author worked directly, on the gold layer of the warehouse and on the Power BI semantic model and the DAX measures (Sections 3 and 5).

## 1.3 Motivation

Before the project, the university met its information needs through an IT-mediated process. Every figure was produced on request: a user asked the IT department for a piece of information, IT wrote and ran a query against the student information system, and the result was returned as a file (Section 4). This process was functional, but it made every figure depend on IT availability, it left the definition of each indicator to whoever wrote the query, and it offered no way to explore the data without opening a new request.

The motivation of this thesis is to document and analyse how a governed Business Intelligence system changes this situation: what it improves, at what cost, and where its limits are. The engagement offers a concrete, real case in which the change can be examined end to end, from the data platform to the dashboards that management consults.

## 1.4 Research question

The thesis addresses the following research question:

> *How does a Snowflake-based semantic model and Power BI KPI dashboard system improve reporting efficiency and decision-support for academic management at Università Vita-Salute San Raffaele, compared to the previous ESSE3-based process of ad hoc, IT-mediated query requests?*

The notion of improvement is examined along four qualitative dimensions: process efficiency, consistency and governance, standing visibility and self-service exploration, and the distribution of technical dependency (Sections 4.4 and 6). The analysis is qualitative. It does not measure the size of the improvement through user studies or timing, a scoping choice explained in Section 4.4.

## 1.5 Contribution

The thesis contributes a documented case study of a Business Intelligence transformation in a higher education institution. It describes the technical design of the solution, the medallion data architecture, the semantic model, and the KPI catalogue for the student careers domain, at a level of detail that shows the real modelling and data quality decisions behind it. It then reads the solution against the previous reporting model along the four dimensions above. Throughout, the boundary between the work of the author and the work of the wider KPMG team is stated explicitly, so that the contribution of the internship is neither overstated nor hidden.

## 1.6 Structure of the thesis

Section 2 describes the hosting company, KPMG, and the setting of the internship. Section 3 states the learning objectives of the internship. Section 4 defines the problem, the pre-existing reporting model and its limitations, and states the research question. Section 5 describes the solution: the overall approach, the data architecture, the semantic model, the KPI design, the Power BI implementation, and the data quality considerations. Section 6 discusses the change along the four dimensions of improvement. Section 7 describes the skills acquired during the internship. Section 8 draws the conclusions and answers the research question directly.
