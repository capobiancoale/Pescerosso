# 4. Problem Definition

## 4.1 The managerial context: information needs in academic management

Universities manage a range of interrelated activities: student admissions and progression, teaching, research output, and the allocation of financial resources. Decisions in these areas increasingly draw on data from the institution's own operational systems rather than on informal knowledge or periodic reports. Sequeira et al. (2024), in a systematic review of Business Intelligence adoption in higher education, note that decisions taken without the support of systematised institutional data may compromise both the effectiveness and the efficiency of institutional action.

Within academic management, monitoring student careers is a particularly information-intensive area. It informs decisions on didactic offering, on tutoring interventions, on the way programs are resourced, and on measures to reduce dropout. For a reporting system to be useful to management, it must deliver information that is accurate, available when decisions are actually being taken, and consistent across the people who consult it.

## 4.2 The pre-existing situation: IT-mediated reporting through ESSE3

The internship engagement analysed in this thesis was carried out at Università Vita-Salute San Raffaele, an Italian private university whose student information system is ESSE3, the operational platform used by most Italian higher education institutions to manage student careers, exams, and administrative records. ESSE3 is designed for transactional use, not for analytical use: it does not natively provide the aggregated, cross-domain views that academic management needs to support its decisions.

Before the introduction of the Business Intelligence solution described in this thesis, the university addressed this gap through an IT-mediated reporting model. Information needs from administrative offices (such as the student registrar and the didactic services) and from academic management (program coordinators and departmental leadership) were channelled through requests to the IT department. Each request followed the same pattern:

1. A requester formulated an information need, for example the number of students in a cohort who had reached a given progression threshold.
2. The IT department translated the request into one or more SQL queries against the ESSE3 database.
3. The queries were executed and the results checked for correctness.
4. When the results appeared inconsistent or incomplete, the query was revised and re-executed. Several iterations were sometimes required.
5. Once the results were considered reliable, they were sent to the requester, typically as a spreadsheet or a static document.

The same workflow served both recurring reporting needs, such as periodic reports on enrolments or exam outcomes, and one-off requests tied to specific decisions. Each information cycle required a discrete manual intervention by IT staff.

## 4.3 Limitations of the pre-existing model

The IT-mediated model was functional but had several structural limitations that affected both the efficiency of the reporting process and the quality of the decision support it provided. These limitations are documented in the wider literature on self-service Business Intelligence (Pałys and Pałys, 2023; Passlick et al., 2023) and can be discussed under four headings.

**Latency and IT bottleneck.** Every information need required IT staff to write, run, and validate a query. The availability of reports was therefore constrained by IT capacity and workload. Recurring requests competed with one-off ones for the same limited resource, and the correction loop between requester and IT often extended the response time further. In practice, decisions were sometimes taken either without the most current information or after a delay that reduced its usefulness.

**Inconsistency across requests.** In the absence of a centrally governed set of metric definitions, similar questions asked at different moments, or by different requesters, could be answered by structurally different queries. Small differences in filters, join logic, or in the treatment of edge cases could produce results that were individually correct but not mutually comparable. When discrepancies emerged, the credibility of the reporting output was called into question and rework was needed to reconcile the figures.

**Absence of standing visibility and self-service exploration.** The pre-existing model was inherently pull-based: information existed only when explicitly requested. Management could not observe trends over time, drill down into a dashboard to investigate an anomaly, or refine a question without initiating a new IT request. This constrained the type of managerial questions that could realistically be asked, and effectively narrowed the analytical scope of the institution to what had already been extracted.

**Concentration of technical dependency.** Because the ability to answer any question resided with the IT department alone, both administrative and academic users depended structurally on IT availability. The literature on self-service BI identifies this concentration of dependency as one of the main drivers of BI transformation (Pałys and Pałys, 2023). For an academic institution, it also means that access to information does not follow the distribution of decisional responsibility.

Taken together, these limitations describe a reporting model whose cost, measured in IT effort, in decision latency, and in analytical opportunities that are never taken, grows with the information intensity of the institution.

## 4.4 Research question

On this basis, the thesis addresses the following research question:

> *How does a Snowflake-based semantic model and Power BI KPI dashboard system improve reporting efficiency and decision-support for academic management at Università Vita-Salute San Raffaele, compared to the previous ESSE3-based process of ad hoc, IT-mediated query requests?*

The notion of "improvement" is operationalised along four qualitative dimensions that mirror the limitations set out in Section 4.3: process efficiency, consistency and governance, standing visibility and self-service access, and the distribution of technical dependency. The thesis does not attempt to quantify these dimensions through user studies or timing measurements. This is an intentional scoping choice, motivated by the confidentiality constraints of the internship engagement and by the qualitative nature of the observational evidence available.

## 4.5 Scope and delimitation

The Business Intelligence solution developed during the internship covers several KPI domains: student careers, admissions, teaching offering, research output, and economic-financial indicators. To keep the analysis at a sufficient depth within the length constraints of this report, the thesis focuses on the **student careers and academic performance** domain. This is the most information-intensive area of academic management. It also best illustrates the transition from IT-mediated reporting to a governed semantic model, and it is the domain in which the author was most directly involved during the internship. The other KPI domains are mentioned where relevant, as parallel components of the wider solution, but their KPI catalogue, semantic modelling choices, and dashboard implementation are not discussed in detail.

---

## [TO CONFIRM] for this section

1. San Raffaele as "Italian private university" — correct?
2. ESSE3 described as "operational platform widely adopted by Italian higher education institutions" — if you know more (vendor CINECA/Kion, etc.), can be more precise.
3. "Program coordinators, departmental leadership" as examples of academic management — if you have more precise role names (without naming individuals), can be adapted.
4. Phrasing "Business Intelligence solution developed during the internship" implies active participation on your part — correct?
