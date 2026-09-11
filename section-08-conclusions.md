# 8 Conclusions

## 8.1 Summary

This thesis has described and analysed a Business Intelligence solution built for Università Vita-Salute San Raffaele during an internship at KPMG Advisory. The solution replaces an IT-mediated reporting process, in which every figure was produced on request through a query against the student information system, with a governed data platform and a set of Power BI dashboards. The thesis focused on the student careers domain: the gold layer of the Snowflake warehouse, the semantic model, and the KPI catalogue built on top of them, together with the data quality work that supports the numbers the dashboards show.

## 8.2 Answer to the research question

The research question asked how the new system improves reporting efficiency and decision support for academic management, compared to the previous ad hoc, IT-mediated process. Read along the four dimensions of Section 4.4, the answer is the following.

Process efficiency improves because the flow from the source systems to the dashboards is automated. For a question the dashboards already cover, the manual query cycle is gone, and the effort that used to be spent once per request is spent once, at design time.

Consistency and governance improve because each KPI is defined in one place, in the semantic model, and every report that uses it returns the same value. The reconciliation of the definitions against the values the university recognises as correct fixed this consistency at a figure the client agrees with.

Standing visibility and self-service improve because the dashboards are a persistent artefact, refreshed daily, that management can consult and, within what the report enables, explore through drill-down, without opening a new request each time.

The distribution of technical dependency changes rather than disappears. Access to information is aligned with the responsibility for the decisions it supports, through role-based access to the reports, and the user no longer depends on IT to obtain a figure. The dependency moves to the pipeline and the semantic model and to whoever maintains them, and it becomes a dependency on a small number of governed artefacts rather than on a queue of individual requests.

The improvement is therefore real along all four dimensions, and it is qualitative. This thesis does not measure its size, and the dependency is transformed rather than removed.

## 8.3 Contribution

The main contribution of the thesis is a documented case of a Business Intelligence transformation in a higher education institution, described from the data architecture to the dashboards, with the real modelling and data quality decisions that shaped it and with an explicit account of what the author built and what the wider team built. Cases at this level of detail are useful to other institutions considering a similar move, because the value of such a project depends on decisions, about grain, about definitions, about trade-offs, that are usually invisible in a higher-level description.

## 8.4 Limitations

The analysis is qualitative and rests on a single case, observed by someone who took part in building the solution. It does not include user interviews or timing measurements, for the reasons of confidentiality and scope stated in Section 4.4, and its claims about efficiency and dependency are structural rather than measured (Section 6.5).

## 8.5 Future work

Three directions would extend this work. The first is a quantitative evaluation: a study with the users of the dashboards, and a measurement of the time and the effort a reporting need now takes compared with the previous process, would put a size on the improvement this thesis describes qualitatively. The second is the extension of the analysis to the other domains of the project, and to the areas planned for the later waves, to see whether the findings for student careers hold where the information needs and the governance stakes are different. The third is a study of the solution in use over time, which would test the maintainability of the design choices made here, in particular the page-specific fact tables, once the system has been running and evolving for a longer period.
