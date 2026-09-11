# 5.4 KPI design for student careers

The semantic model described in Section 5.3 exposes to the reporting layer the entities and the relationships of the student careers domain. The KPIs discussed in this section are, for the most part, the DAX measures defined on top of that model, together with a small number of classifications computed in the gold layer and exposed through it. They translate the operational questions of academic management into calculations that Power BI evaluates in the context of the filters selected by the user of the dashboard. The measures documented in this section were designed and implemented by the author together with the semantic model on which they operate, within the framework and the naming conventions defined by the KPMG delivery team.

## 5.4.1 KPI design methodology and organisation

The KPI catalogue was built iteratively during the internship, starting from a first list of information needs collected from academic management at Università Vita-Salute San Raffaele and refined over successive review cycles between the author, the KPMG delivery team, and the client. Each candidate KPI was defined by three attributes before being implemented as a DAX measure: the business question it answered, the grain at which it applied (for example, one row per student per academic year), and the analytical dimensions along which it needed to be sliced in the reports. Only after these three attributes were agreed with the client was the measure implemented in the semantic model and validated against reference values supplied by the university. Where a measure did not reproduce the reference value, its definition was reconciled with the client, as described in Section 5.6.2.

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

Five KPIs from families 1, 2, and 3 are discussed in detail below. They were selected because they illustrate design choices representative of the catalogue as a whole: filter encapsulation, denominator alignment, use of relationships for cross-fact computations, delegation of categorical logic to dimensions, and the encoding of business rules in the model.

**Media Ponderata.** The weighted average of exam grades by credits is the standard summary of a student's academic performance in the Italian university system. Its DAX implementation uses `SUMX` to compute grade × credits at the row level of the booklet fact, then divides by `SUM` of credits. Three filters restrict the calculation to the exams that must contribute to the average: state of the activity equal to "S" (passed), no-media flag equal to false, and evaluation mode equal to "V" (numeric evaluation, as opposed to pass/fail). Each of the three filters removes a class of records that would otherwise inflate or deflate the average, and their omission is a common source of error when the same computation is done ad hoc against the operational system. Encapsulating the three filters inside the measure guarantees that every dashboard using `Media Ponderata` computes it consistently.

**% Lode.** The share of exams passed with honours is another standard performance indicator. The DAX code is a `DIVIDE` between two `CALCULATE` blocks. The design choice that matters for this discussion is the definition of the denominator. The denominator uses the same set of records that populates `Media Ponderata`: passed exams that contribute to the average and are evaluated with a numeric grade. Alignment of denominators across related KPIs is a governance requirement that a semantic model can enforce and an ad hoc query cannot, because in the ad hoc case the denominator is redefined by every analyst who writes the query.

**Durata Carriera (anni).** The average duration of a student's career is computed as the average, over the graduates in the current filter context, of the year difference between the enrolment date on the career dimension and the graduation date on the graduation fact. Two design details are worth noting. First, the enrolment date is not on the same fact as the graduation date and must be retrieved through the `RELATED` function against the career dimension; the measure therefore relies on the fact-to-dimension relationship defined in the semantic model and would not work outside it. Second, the measure applies an average over graduates rather than computing a single date-to-date difference, which makes it correctly aggregable in the presence of dashboard filters that select multiple graduates.

**Studenti fuori corso.** The count of out-of-schedule students is defined as the count of rows in the enrolment dimension whose calculated column `Corso o fuori corso` is equal to "Fuori corso". The interesting design choice here is the delegation of the categorisation to a calculated column of the dimension rather than to the DAX measure itself. Keeping the categorisation in the dimension has two effects: the same categorisation is available to any measure or visual that uses the enrolment dimension, without redefining the logic; and if the definition of "fuori corso" needs to be revised in the future, the revision happens in one place. The reciprocal measure `Studenti In corso` follows the same pattern with the opposite filter value.

**Fascia Studente CdS.** The student risk band is the most elaborate rule of the catalogue, and the one that best shows the value of pushing a business rule into the governed layer. It is not a DAX measure but a categorical attribute computed in the gold Dynamic Table `TFCT_PRST_PRESENZE_STUDENTE`, at the grain of one row per student per didactic activity per study programme per academic year, and exposed to the semantic model as a column. For each row it takes the ratio between the student's absences and the maximum programmed hours of the activity and assigns one of four bands: green up to 11%, yellow up to 22%, red up to 33%, and black above 33%. When the data is not sufficient for the classification to mean anything, the row is labelled "N/D" rather than forced into a band; this happens when the student has two recorded lectures or fewer, or when the maximum programmed hours of the activity are missing or zero. The same logic is computed a second time at the coarser grain of the study programme, where the absences are compared against an absence budget aggregated across the activities of the programme, so that a report can show a band both per activity and per programme. Three design choices are worth commenting.

The first is that the rule lives in the gold layer rather than in a report. The thresholds of 11%, 22%, and 33%, the four band labels, and the related rules (a non-admission flag raised when the absence rate exceeds 33% for an activity with mandatory attendance, and a below-threshold flag raised when the presence rate falls under the required percentage) are computed once, in the definition of the fact table. Every report that shows a band reads the same value, and a change in the policy is applied by editing one table rather than several reports.

The second is the explicit handling of the denominator and of the missing dimension. The ratio is guarded so that a zero or missing number of hours produces an "N/D" label rather than an error or a misleading band, and a fact row whose study programme cannot be matched is attached to a ghost record rather than dropped. These are the safeguards described in Section 5.3.4, visible here in one definition.

The third is that the classification is stable across the filters of the dashboard. Because the band is materialised at a defined grain in the gold layer, its value for a given student, activity, programme, and year does not change when the user changes the level of aggregation shown on the page; the report aggregates or counts the pre-computed bands rather than recomputing them in a way that could depend on the visible context. Listing 1 shows a simplified extract of the rule, with the personal attributes of the student removed.

```sql
CREATE OR REPLACE DYNAMIC TABLE TFCT_PRST_PRESENZE_STUDENTE
  TARGET_LAG = '1 day', WAREHOUSE = WH_ELT_XS_DEV, REFRESH_MODE = AUTO
AS
WITH stats_studente AS (            -- per student x activity x programme x year
  SELECT PRES_ID_STU_ID, ELEN_ID_AD_ID, ELEN_ID_CDS_ID, ELEN_NR_AA_OFF_ID,
         COUNT(*) AS NR_LEZIONI_TOTALI,
         COUNT(CASE WHEN PRES_CD_PRESENZA = 'FALSE' THEN 1 END) AS NR_ASSENZE
  FROM ...                          -- silver attendance joined to lecture context
  GROUP BY 1, 2, 3, 4
)
SELECT
  COALESCE(dcds.COST_HK, MD5_BINARY('GHOST_RECORD')) AS FK_COST_HK,
  ROUND(st.NR_ASSENZE / NULLIF(st.NR_LEZIONI_TOTALI, 0), 4) AS PRST_PR_ASSENZE,
  CASE
    WHEN st.NR_LEZIONI_TOTALI <= 2
      OR omax.ORE_MAX IS NULL OR omax.ORE_MAX = 0   THEN 'N/D'
    WHEN st.NR_ASSENZE / omax.ORE_MAX <= 0.11        THEN 'VERDE'
    WHEN st.NR_ASSENZE / omax.ORE_MAX <= 0.22        THEN 'GIALLO'
    WHEN st.NR_ASSENZE / omax.ORE_MAX <= 0.33        THEN 'ROSSO'
    ELSE 'NERO'
  END AS PRST_CD_FASCIA_RISCHIO
FROM stats_studente st
LEFT JOIN dim_corso dcds ON dcds.COST_ID_CDS_ID = st.ELEN_ID_CDS_ID
LEFT JOIN ore_max   omax ON omax.AD_GEN_ID = st.ELEN_ID_AD_ID
                        AND omax.ADMO_NR_AA_OFF_ID = st.ELEN_NR_AA_OFF_ID;
```

**Listing 1** — *Simplified extract of the risk-band logic in the gold layer, with the student's personal attributes omitted.*

## 5.4.4 Design patterns across the catalogue

The KPIs described in Section 5.4.3 illustrate a small set of design patterns that recur across the catalogue and follow from the choice to centralise the definitions in the semantic model rather than in the individual reports or in ad hoc queries.

The first pattern is the encapsulation of filter conditions inside the measure. Filters that identify the correct subset of records for a given KPI, such as the three filters of `Media Ponderata`, are written once inside the measure and reused implicitly by every visual that consumes it. This removes a well-known class of error caused by re-implementing the same filters differently across queries.

The second pattern is the reuse of measures inside other measures. Several KPIs of the catalogue are defined by referring to a previously defined KPI as their building block: `# Matricole` refers to `# Iscritti`, and the gender percentages are defined against `# Iscritti`. This composition guarantees that a change to the base definition propagates to the derived KPIs consistently, and it makes the intent of the derived KPI explicit in its expression.

The third pattern is the delegation of categorical logic to calculated columns of the dimensions, illustrated by `Studenti In corso` / `Studenti fuori corso`. When a categorisation is applied by many measures or visuals, defining it at the dimension level rather than at the measure level reduces the number of places at which it must be maintained.

Taken together, these patterns implement in the semantic model the governance principle discussed in Section 4.3 as the second limitation of the pre-existing reporting model: the same metric, asked at different moments or by different users, must return the same value.

---

## [TO CONFIRM] for this section

1. Selection process for KPIs — I described it as "iteratively" with a "first list of information needs collected from academic management" refined through "review cycles between author, KPMG team, and client". Correct? Or was it more top-down (KPMG proposed → client approved)?
2. Thresholds 11%, 22% and 33% for the four-band attendance risk classification (verde/giallo/rosso/nero) — do these come from a formal policy of the client (approved by academic authorities), or were they defined during the project with client validation?
3. RESOLVED — validation was against reference values supplied by the university; where a measure did not match, the definition was reconciled directly with the client. Reflected in the text above and in Section 5.6.2.
4. Display folders — I mention only `Iscrizione` and `Rendimento` (the two that appear in the model file). Are there any other folders that exist for other domains but that I don't see because I only have the didattica model?
