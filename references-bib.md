# References — running bibliography

Formatted per Bayesian Analysis journal conventions, as required by the Master's guidelines.
This file is the single source of truth for what can be cited in the thesis. Add here before citing in a section, never the reverse.

---

## Cited references

### Business Intelligence in higher education

Sequeira, R., Reis, A., Alves, P., and Branco, F. (2024). "Roadmap for implementing business intelligence systems in higher education institutions: systematic literature review". *Information*, 15(4): 208.
- **Used in**: §4.1 (managerial context, information needs)
- **Why it matters**: systematic literature review specifically on BI adoption in HEIs; supports the claim that HEIs need systematised data for effective decision-making.

### Self-service Business Intelligence

Pałys, M. and Pałys, A. (2023). "Benefits and challenges of self-service business intelligence implementation". *Procedia Computer Science*, 225: 795–803.
- **Used in**: §4.3 (limitations of IT-mediated model)
- **Why it matters**: peer-reviewed systematic review of SSBI benefits and challenges; supports the framing of IT bottleneck as a documented driver of BI transformation.

Passlick, J., Grützner, L., Schulz, M., and Breitner, M. H. (2023). "Self-service business intelligence and analytics application scenarios: a taxonomy for differentiation". *Information Systems and e-Business Management*, 21(1): 159–191.
- **Used in**: §4.3 (limitations of IT-mediated model)
- **Why it matters**: taxonomy of SSBI application scenarios; strengthens the literature grounding of the four limitations discussed in §4.3.

### Dimensional modelling

Kimball, R. and Ross, M. (2013). *The Data Warehouse Toolkit: The Definitive Guide to Dimensional Modeling*. John Wiley & Sons, third edition.
- **Used in**: §5.3.1 (design principles), §5.3.3 (star schema shape)
- **Why it matters**: canonical reference for dimensional modelling; establishes the star schema and the fact/dimension terminology used throughout the semantic model chapter.

### Data quality

Wang, R. Y. and Strong, D. M. (1996). "Beyond accuracy: what data quality means to data consumers". *Journal of Management Information Systems*, 12(4): 5–33.
- **Used in**: §5.6 (data quality dimensions: completeness and accuracy)
- **Why it matters**: seminal framework of data quality dimensions grounded in what data consumers need; provides the completeness and accuracy vocabulary used to frame the data quality discussion.

### Company sources (Section 2)

KPMG International (2025). About KPMG. KPMG International Limited. https://kpmg.com/xx/en/home/about.html (accessed 11 September 2026).
- **Used in**: §2.1 (global scale and service lines)
- **Why it matters**: KPMG's own reporting on the size and structure of the global organisation; source for the figures on countries and territories and on headcount.

KPMG in Italy (2026). KPMG in Italy. https://kpmg.com/it/it.html (accessed 11 September 2026).
- **Used in**: §2.2 (Italian organisation and Advisory practice)
- **Why it matters**: KPMG's own presentation of its Italian organisation; source for the figures on professionals, offices, and clients in Italy.

### Technical documentation (added in the v2 revision, verified 14 October 2026)

Databricks (2026). "What is the medallion lakehouse architecture?". *Databricks Documentation*. https://docs.databricks.com/aws/en/lakehouse/medallion (accessed 14 October 2026).
- **Used in**: §5.2 (medallion pattern: bronze, silver, gold)

Microsoft (2026a). "Model relationships in Power BI Desktop". *Microsoft Learn*. https://learn.microsoft.com/en-us/power-bi/transform-model/desktop-relationships-understand (accessed 14 October 2026).
- **Used in**: §5.3.3 (only one active filter path between two tables; ambiguous-path errors)

Microsoft (2026b). "Understand star schema and the importance for Power BI". *Microsoft Learn*. https://learn.microsoft.com/en-us/power-bi/guidance/star-schema (accessed 14 October 2026).
- **Used in**: §5.3.3 (star schema for semantic models; attributes kept on the fact table as an accepted exception)

Snowflake (2026a). "Dynamic tables". *Snowflake Documentation*. https://docs.snowflake.com/en/user-guide/dynamic-tables-about (accessed 14 October 2026).
- **Used in**: §5.3.2 (target lag)

Snowflake (2026b). "Understanding & using Time Travel". *Snowflake Documentation*. https://docs.snowflake.com/en/user-guide/data-time-travel (accessed 14 October 2026).
- **Used in**: §5.2.5 (global rollback by the finaliser)

---

## To be added later (placeholders)

- Kimball on shared dimensions and conformed dimensions (may need dedicated chapter reference from the 2013 book)
- ~~Snowflake Dynamic Tables~~ — DONE in v2: Snowflake (2026a).
- ~~Databricks / medallion architecture documentation~~ — DONE in v2: Databricks (2026).
- ~~Additional references for §5.6 (data quality frameworks)~~ — DONE: Wang and Strong (1996) added above.
- Additional references for §6 (discussion) — likely needs one or two on higher-education analytics transformation
