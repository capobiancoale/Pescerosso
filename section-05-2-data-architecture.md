# 5.2 Data architecture: the Snowflake medallion approach

The data platform underlying the Business Intelligence solution is built on Snowflake and organised according to the medallion architecture, a layered pattern that has become common practice for analytical data platforms in the last few years. The pattern was originally popularised in the Databricks Lakehouse but is now widely adopted also on Snowflake and on other cloud data warehouses, because it addresses two recurring needs in analytical pipelines: the separation between raw ingestion and analytical consumption, and the ability to reprocess or audit the data at each stage without touching the source systems. The medallion pattern separates the data flow into three progressively refined layers, conventionally named bronze, silver, and gold. The following description focuses on how the pattern is instantiated in the specific pipeline that feeds data from ESSE3 into the reporting layer used in this project.

## 5.2.1 End-to-end flow

The pipeline moves data from the ESSE3 Oracle database into the Snowflake analytical platform through three sequential phases: an extraction phase carried out on an on-premise virtual machine, a bronze loading phase orchestrated within Snowflake, and a silver refinement phase implemented through stored procedures. The three phases are chained by a signalling mechanism designed around a fail-fast principle: if any phase does not complete correctly, the subsequent phases do not start, and the analytical layer keeps the data of the previous day rather than exposing partially updated information. The end-to-end flow is illustrated in Figure X.

## 5.2.2 Extraction from ESSE3 to the Snowflake stage

The extraction phase runs each night, orchestrated by a bash script executed on an on-premise virtual machine. The script coordinates three sequential steps.

The first step is the *spool*: a delimited text file is produced for each source table, driven by a per-table configuration file that specifies the extraction query. If any single table fails to spool, the step as a whole is marked as failed and the pipeline stops.

The second step is the *stage upload*: the text files produced by the spool are transferred to a Snowflake stage located in the landing area of the bronze database. The upload only runs if the spool has completed successfully.

The third step is the *semaphore*: an empty marker file is uploaded to a dedicated semaphore stage in the ELT database. The semaphore is written only if both the spool and the stage upload have succeeded. Its absence, on any given night, is a sufficient condition to prevent Snowflake from starting the downstream processing.

## 5.2.3 The bronze layer

The bronze layer holds the raw data extracted from ESSE3. Its tables preserve the structure and the field names of the source system, and are not filtered, cleaned, or restructured. The role of the bronze layer is to provide a faithful copy of the source data at the moment of extraction, so that any subsequent transformation can be traced back to a well-defined starting point.

Loading into the bronze layer is triggered by a Snowflake stream defined on the semaphore stage: the arrival of the semaphore file is detected as new data on the stream, which activates a task DAG. The root task of the DAG has no schedule and fires only when the stream reports data, which encodes the fail-fast principle directly at the level of the Snowflake orchestrator: no semaphore, no run.

The bronze loading task iterates over a parameter table that lists the source tables enabled for the current run. For each enabled table, the loading procedure performs a TRUNCATE of the target bronze table and a COPY INTO from the landing stage, using pattern matching on the file name to associate each stage file with the corresponding table. The file format used is CSV with configurable NULL handling and optional field quoting. The choice of TRUNCATE + COPY INTO, rather than an incremental load, keeps the bronze layer aligned with a full daily snapshot of the source, which simplifies both traceability and reprocessing.

If any table fails to load, the loading procedure raises an exception, the task is marked as failed, and the downstream silver tasks do not start, again as a direct consequence of the fail-fast principle. Every operation is tracked in two logging tables: a job-level log that records one row per source table, and a step-level log that records one row per operation performed (TRUNCATE, COPY INTO, and any error captured during the run).

## 5.2.4 The silver layer

The silver layer contains the cleaned and historised version of the source data. In this project, the silver layer is not a simple cleansing layer but implements a full Slowly Changing Dimension of Type 2 (SCD Type 2), so that the analytical model has access not only to the current state of each entity but also to its history over time.

The silver tables are populated by a collection of stored procedures, one per silver table. The procedures are grouped into six sequential batches, with up to seven procedures running in parallel within each batch. Batch N+1 waits for the completion of every procedure in batch N before starting.

Each silver procedure applies the same two-step SCD Type 2 upsert. The first step closes the records whose values have changed with respect to the source: an UPDATE sets the end-of-validity timestamp of the currently active version to the run time, for every silver record that no longer matches the corresponding bronze record. The comparison is expressed as a MINUS between the bronze extract and the currently active silver records, so that only genuinely changed or new records are returned. The second step inserts the new versions: an INSERT writes into the silver table the rows returned by the same MINUS, with an open end-of-validity timestamp conventionally represented as 9999-12-31. New records are handled by the same mechanism, since they appear in the bronze but not in the silver and are therefore returned by the MINUS as well.

Every silver table carries four audit columns in addition to the source attributes: an insertion timestamp, a last-update timestamp, a validity-from timestamp, and a validity-to timestamp. The active version of a record is identified by the conventional value of the validity-to timestamp; any earlier value corresponds to a historical version. This allows any KPI that requires point-in-time reconstruction to be computed correctly against the state of the data as of a specific date, without additional infrastructure.

## 5.2.5 Transactional guarantees and the finaliser

The error-handling strategy of the silver layer is different from that of the bronze layer, and reflects the different consequences that a partial success would have.

Each silver stored procedure is written as a single transaction: it opens with BEGIN TRANSACTION, applies both steps of the SCD Type 2 upsert, and closes with COMMIT. If any step fails, the procedure performs a local ROLLBACK and returns the error message rather than raising an exception. The reason for not raising is operational: raising an exception would abort the entire silver batch and prevent the other procedures from running, whereas the transactional local rollback keeps the failure contained to the affected table and allows the rest of the batch to proceed.

However, allowing a run to end with some silver tables updated and others reverted is not acceptable from a consistency standpoint, because a report that consumes the gold layer built on top might mix values from different points in time. To handle this case, the DAG ends with a finaliser task, whose responsibility is to enforce a global consistency guarantee across the silver layer.

The finaliser inspects the logs of the current run and checks whether any silver procedure has failed. If at least one procedure failed, the finaliser performs a global rollback of every silver table that was updated successfully in the same run, using Snowflake Time Travel to restore each table to its state before the run began. The net effect is that the silver layer is either entirely up-to-date or entirely aligned with the previous day, and never in an intermediate state. This design applies the fail-fast principle at the end of the pipeline as well as at its beginning: partial data is treated as worse than stale data.

## 5.2.6 The gold layer

The gold layer contains the tables that are directly consumed by the Power BI semantic model. Its structure reflects the analytical needs of the reporting layer rather than the structure of the source system: the tables are shaped as fact and dimension entities and are pre-aggregated where the reporting logic makes it convenient. Because the semantic model developed in the student careers domain adopted a hybrid modelling approach, discussed in detail in Section 5.3, the gold layer includes a mix of shared dimensions and of wider, page-specific fact tables designed to serve individual sections of the report without generating ambiguous relationships in the semantic model.

## 5.2.7 Summary and role in the thesis

The three-phase pipeline described above provides two properties that are relevant for the rest of the thesis. First, it exposes to the analytical layer a data foundation whose consistency is guaranteed at the level of the run as a whole, rather than at the level of the individual table: for the semantic model built on top, this means that the values displayed in a dashboard reflect a coherent state of the source data, not a mixture of times. Second, the SCD Type 2 historisation makes point-in-time analysis available to the KPI layer without requiring additional infrastructure, which is a precondition for several of the KPIs discussed in Section 5.4.

The scope of the thesis does not extend to the detailed contents of the transformations applied to individual source tables at the silver layer, which were developed by the wider team. The remainder of the chapter focuses on the layers where the author's direct contribution is concentrated: the semantic model built on top of the gold layer (Section 5.3), the KPI catalogue implemented within it (Section 5.4), and the Power BI dashboards and their distribution model (Section 5.5).

---

## [TO CONFIRM] for this section

1. Nightly batch time — I wrote generically "each night" without specifying ~21:00 CET. If you want to be more precise: "in the late evening" or "each night after operational hours". Preference?
2. SCD Type 2 logic described conceptually without SQL snippets (`UPDATE ... FROM (SELECT ... MINUS SELECT ...) src`) — I can add a simplified SQL example in a Listing 1 (numbered as figure), which is normal in technical theses. Want?
3. Specific number "39 silver tasks" — not included because it's a very specific implementation detail. Include?
