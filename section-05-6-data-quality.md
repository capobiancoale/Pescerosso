# 5.6 Data quality considerations

The value of the KPIs described in the previous sections depends on the quality of the data that feeds them. This section discusses the data quality problems encountered while building the student careers domain, the mechanisms through which the gold layer and the semantic model addressed them, and the limitations that remain. The problems fall under two of the data quality dimensions identified by Wang and Strong (1996): completeness, whether the data a KPI needs is present, and accuracy, whether the value a KPI returns agrees with the figure the organisation recognises as correct.

The scope of this discussion is the gold layer and the reporting model. The ingestion of the source data into the bronze and silver layers, and the correctness of the records held in the operational systems, sit upstream of this boundary and were handled by the wider delivery team and by the client. The author's contribution to data quality was at the gold layer and above: the construction of the gold tables for the student careers domain within the framework defined by the team, the semantic model, and the DAX measures.

## 5.6.1 Problems encountered

Two classes of problem affected the construction of the student careers KPIs.

The first is incompleteness of the source data. For a number of records, the data a KPI needed was not present, either because it had never been registered in the operational system or because it was not available for other reasons. Attendance is the clearest case. A student with no recorded lectures for a given programme and academic year has no absence rate to compute, and a calculation that ignores this returns either an error or a value that places the student in a risk band that does not mean anything.

The second is the risk of duplication in the joins, which inflates a measure by matching each row on one side to several rows on the other. It has two causes in this model. One is historisation: a dimension in the gold layer inherits several versions of the same business key from the silver layer, so a fact row joined to it without deduplication matches every version. The other is a mismatch in grain between two tables that are related directly. A concrete instance of the second appeared in the programmed teaching hours. When the fact table of the didactic activities was related to the students, the hours of each activity were repeated once for every student connected to it, and the total programmed hours for a single study programme rose above twenty thousand, a figure with no physical meaning. The hours belong to the activity, not to the pair of activity and student. The problem was resolved by computing the hours at the grain at which they are defined, with one table reporting programmed and delivered hours per study programme and a separate table reporting them per teacher, which is the page-specific fact design discussed in Section 5.3.3. In neither case does the fan-out appear in the dashboard as an error. It appears as numbers that are too high, which is why it has to be prevented in the model rather than found by inspecting the output.

## 5.6.2 Validating the data: the UAT of the pipeline and the reconciliation of the KPIs

The data behind the reports was validated on two levels. The pipeline that loads the warehouse was checked through a User Acceptance Testing exercise run by the delivery team on the flow from ESSE3 to the Snowflake silver layer, and the KPIs built on top of the warehouse were reconciled against reference values supplied by the university.

The UAT was organised in two levels of control. The first level was a set of automatic technical checks, run with the OpenMetadata tool on every silver table before the session with the client: a comparison of the row count between the bronze and the silver table, a check that the primary key, including composite keys, is unique, and a check that no field of the key is null. The second level was a functional validation carried out with the client: a sampling of representative records compared against the source, a check of the table structure against the expected schema, and a matching of the element counts between ESSE3 and the silver layer. Each control was recorded as pass, fail, or warning, where a warning is a difference within a tolerated threshold that is documented rather than treated as a defect. The dimensions these controls address, completeness, uniqueness, accuracy, validity, and consistency, are data quality dimensions in the sense of Wang and Strong (1996).

One finding of this exercise shows the kind of problem a count-based control catches. A reconciliation of the student careers loaded into the warehouse, about 38,600 records, against the source found nine that were present in the warehouse but no longer in ESSE3. These were careers deleted at the source after they had been loaded, whose deletion had not propagated downstream. The number is small, but the case is instructive: a record that is correct when it is loaded can become stale if it is later removed at the source, and only a control that compares the two systems will surface it.

The KPIs built on the validated data were then reconciled against reference values supplied by Università Vita-Salute San Raffaele, which provided sample figures for a set of indicators. A measure was treated as validated only when its output matched these figures. In several cases the first computation did not match. The mismatch was not always a coding error in the measure. For some indicators it came from a difference between the definition assumed in the semantic model and the definition the university had used to produce its own figures. When this happened, the team asked the university directly for the logic behind the reference values, that is, for the way each figure was calculated at the source, and aligned the definition in the semantic model to it.

The count of active students is an example. The first definition treated a student as active when the student was enrolled and had paid the enrolment fee. The figure matched the university's only after a third condition was added, that the career status of the student is active, a condition the university applied in its own definition and the model had not. The correction touched a limited number of records, but it changed the meaning of the indicator, and it is the kind of difference that stays invisible until a computed figure is placed next to a reference figure.

This process did more than correct individual numbers. It fixed the definition of the affected KPIs at the value the client recognised as correct, which is the consistency requirement set out in Section 4.3, and it placed that definition inside the semantic model, where it is applied once for every dashboard instead of being rewritten in each ad hoc query.

## 5.6.3 Safeguards in the gold layer and the model

The safeguards already described in Section 5.3.4 address the two classes of problem of Section 5.6.1 at the point where the data enters the model.

Deduplication of dimensions through `QUALIFY ROW_NUMBER()` removes the version-based fan-out, by selecting a single version of each business key before the dimension is joined to a fact. This pattern belongs to the shared gold framework and is applied across the student careers tables.

The ghost record pattern handles the incompleteness of the joins. A fact row whose dimensional attributes are unknown is matched to a dedicated ghost row rather than dropped, so the incomplete records stay visible in the dashboard as a distinct category instead of disappearing from the totals.

At the measure level, the `Fascia Studente CdS` KPI handles the missing-attendance case directly. When a student has no recorded lectures for a programme and year, the measure returns the label "N/D" rather than a risk band, which surfaces the gap in the report instead of hiding it behind a colour. The systematic use of `NULLIF` on the denominators of the ratios computed in the gold layer plays the same preventive role for divide-by-zero situations, returning an empty value that the report renders as a blank cell.

A further check is procedural rather than embedded in the code. During the construction of a fact table, the row count is compared before and after each join with a dimension. A change that is not explained by the modelling logic signals a fan-out or a missing filter, and is treated as a blocker until it is understood. The programmed hours case of Section 5.6.1 is exactly the kind of problem this check catches. At the pipeline level, the bronze-to-silver count control of the UAT plays the same role for the loads that feed the gold layer.

## 5.6.4 Residual limitations

The safeguards above prevent or expose data quality problems inside the reporting system, but they do not remove the limitations that originate outside it.

Completeness at the source is the main one. If a value was never recorded in the operational system, no layer downstream can reconstruct it. The same holds in reverse for records deleted at the source after they were loaded: unless a reconciliation detects them, as in the case of the nine careers in Section 5.6.2, they persist in the warehouse until the next check. The system makes these gaps visible, through the ghost record, the "N/D" label, and the count controls, but it cannot fill them on its own, and a KPI computed over incomplete source data is only as complete as the source. This is a property of the data rather than a defect of the model, but it constrains how some indicators can be read.

The refresh lag is a second limitation. The reports reflect the state of the source at the end of the previous day, so a correction made at the source during the day is not visible until the next refresh. For the decisions this system supports, a latency of one day is acceptable, as discussed in Section 5.3, but it is a limitation to keep in mind when a figure is checked against the live operational system.

The absence of cross-page filter synchronisation, described in Section 5.3.3, has a data quality consequence as well as a usability one. A figure read on one page and a figure read on another are not guaranteed to sit under the same filter context, so a user who reconciles two pages has to set the filters consistently on each.

Finally, the reconciliation of Section 5.6.2 is bounded by the availability of reference values. It was possible for the indicators for which the university provided sample figures. For indicators without an external reference, validation relied on internal consistency checks, which catch a computation that contradicts itself but not one that is internally consistent and still wrong.

---

## Notes for this section

Confirmed with the author and grounded in the project's own UAT documentation: the two-level UAT (L1 automatic technical checks with OpenMetadata, L2 functional checks with the client) and the case of the nine careers present in the warehouse but deleted from ESSE3. Aggregate figures only; no personal identifiers, no individual names, and no queries are reproduced (R7). The active students example, the programmed hours example, and the reconciliation with the client were provided by the author. Wang and Strong (1996) is cited for the data quality dimensions.
