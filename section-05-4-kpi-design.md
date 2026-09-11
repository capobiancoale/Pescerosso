# 5.4 KPI design for student careers

The semantic model described in Section 5.3 exposes to the reporting layer the entities and the relationships of the student careers domain. The KPIs discussed in this section are the DAX measures defined on top of that model. They translate the operational questions of academic management into calculations that Power BI evaluates in the context of the filters selected by the user of the dashboard. The measures documented in this section were designed and implemented by the author together with the semantic model on which they operate, within the framework and the naming conventions defined by the KPMG delivery team.

## 5.4.1 KPI design methodology and organisation

The KPI catalogue was built iteratively during the internship, starting from a first list of information needs collected from academic management at Università Vita-Salute San Raffaele and refined over successive review cycles between the author, the KPMG delivery team, and the client. Each candidate KPI was defined by three attributes before being implemented as a DAX measure: the business question it answered, the grain at which it applied (for example, one row per student per academic year), and the analytical dimensions along which it needed to be sliced in the reports. Only after these three attributes were agreed with the client was the measure implemented in the semantic model and validated against manually reconstructed reference values.

Two design choices at the level of the semantic model support the KPI catalogue as a whole.

The first is a *hybrid approach to the location of the measures within the model*. A dedicated table named `KPI`, unrelated to any fact or dimension table, holds the measures that summarise the state of the domain at the top level of the reports (for example, `# Iscritti`, `Media Ponderata`, `% Lode`) and are typically used across several report pages. Measures that are strongly bound to a specific entity, such as those that aggregate the hours of a specific fact or that classify the students of a specific dimension, are defined directly on the corresponding table. The distribution is not accidental: it reflects the criterion of proximity between the measure and its primary owning entity, and follows the principle that a measure whose logic is inseparable from a specific entity should live on that entity. All measures of the model, whether they reside in the `KPI` table or in one of the fact or dimension tables, are exposed to the report developer and available for use in the dashboards; the location determines only where the measure appears in the field list of the reporting tool.

The second is the use of *display folders* to group the measures thematically. Within the `KPI` table, two folders were adopted for the student careers domain, `Iscrizione` for the population-and-enrolment measures and `Rendimento` for the academic performance measures, so that the report developer sees the top-level KPIs organised by business meaning rather than in alphabetical order. Measures that reside outside the `KPI` table inherit the grouping given by their owning entity, which places them naturally alongside the columns and the other measures of that entity in the field list.

## 5.4.2 The KPI catalogue

The full catalogue of KPIs implemented in the student careers semantic model is presented in Table 1. The KPIs are grouped into six thematic families, ordered by their proximity to the core focus of the thesis and by their frequency of use in the dashboards. Families 1, 2, and 3 (enrolment and population, academic performance, risk and attendance) are the direct focus of this section and are discussed in detail in Section 5.4.3. Families 4, 5, and 6 (didactic organisation, teaching staff, classrooms) are components of the wider solution and are included in the catalogue for completeness, without an individual discussion.

| Family | KPI | Type | Focus |
|---|---|---|---|
| 1. Enrolment & population | `# Iscritti` | Count of distinct students | Active or enrolled students |
| 1 | `# Matricole` | Count | First-year new entries |
| 1 | `Iscritti primo anno` | Count | Students enrolled with course year = 1 |
| 1 | `% Donne` / `% Uomini` | Ratio | Gender distribution |
| 1 | `% Pre-Laurea` / `% Post-Laurea` | Ratio | Distribution by academic-cycle level |
| 1 | `Studenti In corso` / `Studenti fuori corso` | Count | Career-progression status |
| 2. Academic performance | `# Attività Didattica` | Distinct count | Distinct courses/exams in students' booklets |
| 2 | `# Corsi` | Distinct count | Distinct study programmes |
| 2 | `Attivita Superate` / `Programmate` / `Frequentate` | Count | Booklet state segmentation |
| 2 | `Media CFU per Studente` | Average | Credits earned per student |
| 2 | `Media Aritmetica` | Average of grades | Passed exams contributing to average |
| 2 | `Media Ponderata` | Weighted average by CFU | Passed exams contributing to average |
| 2 | `% Lode` | Ratio | Share of exams passed with honours |
| 2 | `Media Voto Laurea Adj` | Average with adjustment | Degree grade, honours weighted |
| 2 | `Durata Carriera (anni)` | Average year-difference | Enrolment-to-graduation, per graduate |
| 3. Risk & attendance | `% Mancato Superamento` | Ratio | Share of exam failures |
| 3 | `% Assenze con almeno 5 lezioni` | Ratio | Absence rate, statistically relevant lectures |
| 3 | `Fascia Studente CdS` | Categorical (traffic light) | Risk band per student × programme × year |
| 4. Didactic organisation | `# Attività didattiche` | Count | Scheduled didactic activities |
| 4 | `Ore Programmate` / `Ore Effettive` | Sum of hours | Scheduled and delivered hours |
| 4 | `Media Ore per Attività Didattica` | Ratio | Delivered hours per activity |
| 5. Teaching staff | `Ore Docenti A Contratto` / `Ore Docenti Di Ruolo` | Sum | Delivered hours by teacher type |
| 5 | `Lezioni Totali` | Count | Lectures in the teacher registry |
| 5 | `Durata Media Lezione (ore)` | Average | Average lecture duration |
| 6. Classrooms | `% Capienza Aula` | Ratio | Attendance over classroom capacity |
| 6 | `Numero Studenti` (aule) | Distinct count | Distinct students per classroom |
| 6 | `Nr Iscrizioni` (aule) | Count | Enrolments per classroom |

**Table 1** — *KPI catalogue for the student careers domain.*

## 5.4.3 Design choices in representative KPIs

Five KPIs from families 1, 2, and 3 are discussed in detail below. They were selected because they illustrate design choices representative of the catalogue as a whole: filter encapsulation, denominator alignment, use of relationships for cross-fact computations, delegation of categorical logic to dimensions, and encoding of business rules as measures.

**Media Ponderata.** The weighted average of exam grades by credits is the standard summary of a student's academic performance in the Italian university system. Its DAX implementation uses `SUMX` to compute grade × credits at the row level of the booklet fact, then divides by `SUM` of credits. Three filters restrict the calculation to the exams that must contribute to the average: state of the activity equal to "S" (passed), no-media flag equal to false, and evaluation mode equal to "V" (numeric evaluation, as opposed to pass/fail). Each of the three filters removes a class of records that would otherwise inflate or deflate the average, and their omission is a common source of error when the same computation is done ad hoc against the operational system. Encapsulating the three filters inside the measure guarantees that every dashboard using `Media Ponderata` computes it consistently.

**% Lode.** The share of exams passed with honours is another standard performance indicator. The DAX code is a `DIVIDE` between two `CALCULATE` blocks. The design choice that matters for this discussion is the definition of the denominator. The denominator uses the same set of records that populates `Media Ponderata`: passed exams that contribute to the average and are evaluated with a numeric grade. Alignment of denominators across related KPIs is a governance requirement that a semantic model can enforce and an ad hoc query cannot, because in the ad hoc case the denominator is redefined by every analyst who writes the query.

**Durata Carriera (anni).** The average duration of a student's career is computed as the average, over the graduates in the current filter context, of the year difference between the enrolment date on the career dimension and the graduation date on the graduation fact. Two design details are worth noting. First, the enrolment date is not on the same fact as the graduation date and must be retrieved through the `RELATED` function against the career dimension; the measure therefore relies on the fact-to-dimension relationship defined in the semantic model and would not work outside it. Second, the measure applies an average over graduates rather than computing a single date-to-date difference, which makes it correctly aggregable in the presence of dashboard filters that select multiple graduates.

**Studenti fuori corso.** The count of out-of-schedule students is defined as the count of rows in the enrolment dimension whose calculated column `Corso o fuori corso` is equal to "Fuori corso". The interesting design choice here is the delegation of the categorisation to a calculated column of the dimension rather than to the DAX measure itself. Keeping the categorisation in the dimension has two effects: the same categorisation is available to any measure or visual that uses the enrolment dimension, without redefining the logic; and if the definition of "fuori corso" needs to be revised in the future, the revision happens in one place. The reciprocal measure `Studenti In corso` follows the same pattern with the opposite filter value.

**Fascia Studente CdS.** The student risk band per programme is the most complex measure of the catalogue, and the one that best illustrates the value of a governed semantic model for management reporting. It is defined directly on the presenze fact table, in accordance with the hybrid approach described in Section 5.4.1: its logic is tightly coupled to the attendance data and its grain matches that of the fact. Its purpose is to classify each student, within a given study programme and academic year, into one of three risk bands (labelled with a green, yellow, or red traffic-light convention) based on the student's absence rate: below 11%, between 11% and 22%, and above 22%. The measure is defined at the grain of student × study programme × academic year, and it computes for each such triple the ratio between total absences and total lectures, then assigns the band using a `SWITCH` expression on the ratio. Three design choices are worth commenting.

The first is the explicit computation of the grain inside the measure. Because the calculation must be performed per student per programme per year regardless of the filter context, the measure uses variables (`VAR`) to capture the values of the three grain attributes at the current row and then applies a `CALCULATE` with an explicit `FILTER` that restricts the calculation to that same triple. This ensures that the categorisation of a student is stable across dashboard filters that change the visible level of aggregation.

The second is the encoding of a business rule as a data-model object. The thresholds of 11% and 22% and the traffic-light labels are policies of the academic management of the university, not of the reporting tool. Their inclusion in the measure means that every dashboard displaying a fascia value uses the same thresholds, and that a change in the policy (for example, a shift of the yellow-red threshold from 22% to 20%) is applied by editing one measure rather than by hunting for the definition across multiple reports.

The third is the handling of the empty case. When a student has zero recorded lectures for a given programme and year, the ratio would be undefined and the traffic-light categorisation would be misleading. The measure returns the explicit label "N/D" in this case, which surfaces the incomplete data in the report rather than collapsing it into a colour band.

## 5.4.4 Design patterns across the catalogue

The KPIs described in Section 5.4.3 illustrate a small set of design patterns that recur across the catalogue and follow from the choice to centralise the definitions in the semantic model rather than in the individual reports or in ad hoc queries.

The first pattern is the encapsulation of filter conditions inside the measure. Filters that identify the correct subset of records for a given KPI, such as the three filters of `Media Ponderata`, are written once inside the measure and reused implicitly by every visual that consumes it. This removes a well-known class of error caused by re-implementing the same filters differently across queries.

The second pattern is the reuse of measures inside other measures. Several KPIs of the catalogue are defined by referring to a previously defined KPI as their building block: `# Matricole` refers to `# Iscritti`, and the gender percentages are defined against `# Iscritti`. This composition guarantees that a change to the base definition propagates to the derived KPIs consistently, and it makes the intent of the derived KPI explicit in its expression.

The third pattern is the delegation of categorical logic to calculated columns of the dimensions, illustrated by `Studenti In corso` / `Studenti fuori corso`. When a categorisation is applied by many measures or visuals, defining it at the dimension level rather than at the measure level reduces the number of places at which it must be maintained.

Taken together, these patterns implement in the semantic model the governance principle discussed in Section 4.3 as the second limitation of the pre-existing reporting model: the same metric, asked at different moments or by different users, must return the same value.

---

## [TO CONFIRM] for this section

1. Selection process for KPIs — I described it as "iteratively" with a "first list of information needs collected from academic management" refined through "review cycles between author, KPMG team, and client". Correct? Or was it more top-down (KPMG proposed → client approved)?
2. Thresholds 11% and 22% for the traffic-light attendance risk band — do these come from a formal policy of the client (approved by academic authorities), or were they defined during the project with client validation?
3. Validation against "manually reconstructed reference values" — is that how you actually validated the measures? Or was it more via comparison with previous reports, or another method?
4. Display folders — I mention only `Iscrizione` and `Rendimento` (the two that appear in the model file). Are there any other folders that exist for other domains but that I don't see because I only have the didattica model?
