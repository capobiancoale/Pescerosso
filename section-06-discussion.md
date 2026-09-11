# 6 Discussion: before and after

The previous sections described the reporting model that existed before the project (Section 4.2) and the Business Intelligence solution built during the internship (Section 5). This section reads one against the other, using the four dimensions of improvement set out in Section 4.4: process efficiency, consistency and governance, standing visibility and self-service exploration, and the distribution of technical dependency. The comparison is qualitative. No user study and no timing measurement were carried out, for the reasons given in Section 4.4, so the claims below are structural: they follow from how the two processes work, not from measured effect.

In the previous model, every figure was produced by a manual cycle. A user asked the IT department for a piece of information, IT wrote and ran a query against ESSE3, checked the result, and returned it as a file (Section 4.2). The solution replaces that cycle, for the information the dashboards cover, with an automated flow that moves data from the source systems through the medallion pipeline into the gold layer and into a set of governed dashboards that the user consults directly.

## 6.1 Process efficiency

In the previous model each information need cost one manual intervention by IT: write the query, run it, validate it, and, when the result looked wrong, revise and re-run it before delivering. The availability of a figure was bounded by IT capacity, and recurring needs competed with one-off ones for the same people (Section 4.3).

In the solution, the flow from source to dashboard runs without a per-request step. Data moves from the operational systems through the pipeline into the gold layer, and the dashboards refresh with a lag of one day (Sections 5.2 and 5.3). For a question the dashboards already cover, the manual cycle is gone: the user opens a report instead of filing a request, and the IT effort that used to be spent once per request is spent once, at design time.

This does not remove development work altogether. A genuinely new question, outside what the reports expose, still requires a change to the model or a new visual. What changes is that the recurring needs, which were the bulk of the manual load, are served without a manual query each time.

## 6.2 Consistency and governance

The previous model had no central set of metric definitions. The same question, asked at different moments or by different people, could be answered by structurally different queries, and small differences in filters or edge-case handling produced figures that were each defensible but not comparable. When the figures disagreed, the reporting lost credibility and the discrepancy had to be reconciled by hand (Section 4.3).

In the solution, each KPI is defined once in the semantic model, and every dashboard that shows it uses that definition (Section 5.4). The filters that select the right records sit inside the measure rather than in the query of whoever happens to write it. The reconciliation against the university's reference values (Section 5.6.2) fixed the affected definitions at the values the institution recognises as correct, as in the active students case, where the agreed definition required the career status to be active on top of enrolment and payment. A figure read in one report is the figure read in another.

## 6.3 Standing visibility and self-service exploration

The previous model was pull-based: information existed only when it was requested. Management could not follow a KPI over time, drill into an anomaly, or refine a question without opening a new IT request, which narrowed the range of questions that were realistic to ask (Section 4.3).

In the solution, the reports are a standing artefact refreshed daily (Section 5.5). A KPI can be read as a trend, and on the drill-down pages the user moves from the institution to the study programme, to the cohort, and to the individual student where the report allows it (Section 5.5.2). This is the self-service that the literature associates with a shift away from IT-mediated reporting (Pałys and Pałys, 2023; Passlick et al., 2023). It is guided self-service rather than open-ended querying: the user explores within what the report designer enabled, and a question nobody anticipated still needs development. The gain is in the range of questions that can now be answered without a request, not in unlimited freedom to ask any question.

## 6.4 Distribution of technical dependency

In the previous model the ability to answer any question sat with the IT department, so both administrative and academic users depended on IT availability, and access to information did not follow the distribution of decisional responsibility. The self-service BI literature identifies this concentration as one of the main drivers of BI transformation (Pałys and Pałys, 2023).

In the solution, the reports are distributed through a Power BI application with access differentiated by role, for the rectorate, the study programme directors, and the administrative offices (Section 5.5.3). A user who consults a report no longer needs IT to obtain the figure, so access to information is aligned with the responsibility for the decisions it supports.

Dependency is not removed by this. It moves. The institution now depends on the pipeline and the semantic model working correctly, and on whoever maintains them, rather than on IT being available for each request. The dependency changes in kind, from per-request and manual to structural and shared, and it becomes visible in a smaller number of well-defined artefacts rather than in a queue of individual requests.

## 6.5 Limitations of the analysis

The comparison above has clear limits, and they bound what can be claimed from it.

It is qualitative and it is a single case. The analysis maps the design of the solution onto the four dimensions of Section 4.4; it does not measure how large the improvement is. One engagement at one institution cannot support a general quantitative claim.

There was no user study and no timing measurement. The statements about latency and effort are structural, drawn from the two processes, not from interviews or before-and-after timings. The confidentiality of the engagement, stated in Section 4.4, is part of the reason this evidence was not collected.

The author is not a neutral observer. Having taken part in building the solution gives direct knowledge of its design, but it is not a detached standpoint from which to judge its effect in use.

Finally, the evaluation is of the solution as delivered. Its value in practice depends on adoption by the intended audiences, which fell outside the observation window of the internship.

## 6.6 Managerial implications for higher education institutions

The bottleneck that a governed BI layer removes is organisational as much as technical. What made the previous model slow was that every figure passed through one team, and what the semantic model changes is that the definition of a figure moves out of individual queries into a shared object that many reports reuse. An institution that adopts the tool without governing the definitions would keep much of the old inconsistency.

For a higher education institution weighing a similar move, the case suggests that the return is largest where the information need is recurring and shared across roles, which is the student careers domain, and where a single agreed definition carries governance weight, such as the figures reported to academic authorities. This is consistent with the conditions for BI adoption in higher education discussed by Sequeira et al. (2024).

The design choices in this project are trade-offs, not defaults. The page-specific fact tables and the daily refresh fit the needs observed here (Sections 5.3.3 and 5.6.4). An institution with a stronger requirement for cross-page consistency, or with decisions that need sub-daily data, would weigh those choices differently.

---

## [TO CONFIRM] for this section

1. Before/after example: I use your point that every figure previously came from an IT query and that the process is now automated. I scope it as "for the information the dashboards cover" rather than "everything automated", to avoid overclaiming (a new question outside the dashboards still needs development). Is that framing right?
2. Maintenance and dependency (§6.4): I wrote that the institution now depends on "whoever maintains" the pipeline and the semantic model. Who maintains it after delivery, the KPMG team on an ongoing basis, an internal university IT/BI team, or a mix? If you can tell me, I will make the sentence precise.
3. Managerial implications (§6.6): do you agree with the three points, in particular that the biggest return is in the recurring, shared, governance-heavy indicators? If your experience points elsewhere, I will adjust.
