# NovaTech Revenue Intelligence Dashboard

* **Project:** Revenue Intelligence Dashboard for NovaTech Solutions
* **Author:** Eric Jang

Project completed as part of the Future AWS Agentic AI Business Professional Nanodegree track under the [Udacity X AWS AWS AI & ML Scholars programme](https://www.udacity.com/scholarships/aws-ai-ml-scholars).

## Project overview

NovaTech Solutions is a fictional B2B SaaS company whose sales, marketing, and support data are stored in separate systems. This project uses Amazon Quick Suite (QuickSight dashboard authoring and Q) to combine those sources into three decision-focused views for the VP of Revenue and the wider revenue team.

The project addresses three business questions:

1. Which marketing activities generate responses, conversions, and attributed revenue?
2. How are deal outcomes, revenue, and sales-cycle performance changing?
3. Which product areas and customer accounts show the strongest support-risk signals?

## Executive report

Start with the [NovaTech Revenue Intelligence Dashboard executive report](<06_report/Executive Report_ NovaTech Revenue Intelligence Dashboard.pdf>) for a concise overview of the dashboard findings, business implications, recommendations, and analytical limitations. The accompanying [dashboard executive summary text](06_report/06_dashboard_executive_summary_text.md) records the narrative generated in Amazon Quick Suite.

## Dashboard outputs

| View | Business purpose | Annotated output | Dashboard-only output |
|---|---|---|---|
| Marketing Funnel | Evaluate campaign spend, response, conversion, and attributed revenue | [Marketing Funnel with insight](03_dashboard/03_dashboard_pages_with_annotation/NovaTech_Marketing_Funnel_annotation.pdf) | [Marketing Funnel](<03_dashboard/03_dashboard_pages_no_annotation/NovaTech_Marketing%20Funnel.pdf>) |
| Sales Pipeline | Monitor deal outcomes, won revenue, win rates, and days to close | [Sales Pipeline with insight](03_dashboard/03_dashboard_pages_with_annotation/NovaTech_Sales_Pipeline_annotation.pdf) | [Sales Pipeline](<03_dashboard/03_dashboard_pages_no_annotation/NovaTech_Sales%20Pipeline.pdf>) |
| Customer Health | Review support demand, sentiment, severity, and account-level risk indicators | [Customer Health with insight](03_dashboard/03_dashboard_pages_with_annotation/NovaTech_Customer_Health_annotation.pdf) | [Customer Health](<03_dashboard/03_dashboard_pages_no_annotation/NovaTech_Customer%20Health.pdf>) |

## Key findings

- Sales: 315 of 499 CRM deal records were marked won, producing $707,201 in won revenue and a 63.13% record-level win rate. Average days to close reached 90.1 for the 28 records closed in January 2025, compared with 54.4 in Q4 2024 and 51.5 in Q1 2024.
- Marketing: $12.36 million in campaign spend generated $1.13 million in attributed revenue, an overall ROI of -90.88%. NovaEdge Awareness accounted for 12.3% of spend but 3.0% of attributed revenue, with the lowest response rate (6.2%) and closed-won conversion rate (5.1%).
- Customer support: Authentication and Data Pipeline produced 279 of 450 high-severity tickets (62.0%) while accounting for 892 of 3,000 total tickets (29.7%).

These figures were cross-checked against the included CSV files. See [Analytical limitations](#analytical-limitations) before interpreting the joined Customer Health revenue measures.

## Methodology

1. Verified row counts, date coverage, category values, and missing values in all three source files.
2. Corrected field types and created calculated fields for revenue, conversion, response, resolution time, severity, and risk analysis.
3. Aggregated CRM and marketing measures to one row per account before joining them to the support-ticket dataset.
4. Built Marketing Funnel, Sales Pipeline, and Customer Health dashboard views and loaded the datasets into SPICE.
5. Configured an Amazon Q topic, tested baseline and domain-specific questions, and compared Q's answers with dashboard visuals.

Detailed evidence is available in the [verification log](01_verification/01_data_verification_log.md), [data-preparation log](02_data_preparation/02_data_transformation.md), [dashboard configuration log](03_dashboard/03_dashboard.md), [topic-configuration log](04_topic/04_topic.md), [Q exploration log](05_exploration/05_NovaTech_Q_Exploration_Log.md), [executive report](<06_report/Executive Report_ NovaTech Revenue Intelligence Dashboard.pdf>), and [dashboard executive summary text](06_report/06_dashboard_executive_summary_text.md).

## Data

| Dataset | Rows | Columns | Coverage field and range |
|---|---:|---:|---|
| CRM deals | 499 | 20 | `deal_created_date`: 2023-06-17 to 2025-01-25 |
| Marketing campaigns | 2,240 | 20 | `campaign_date`: 2023-01-01 to 2025-01-31 |
| Support tickets | 3,000 | 20 | `ticket_created_date`: 2023-06-01 to 2025-02-28 |

All datasets use `account_id` as a common join key. CRM contains 85 accounts (`ACCT-001` to `ACCT-085`). Marketing and support also contain intentional orphan account IDs (`ACCT-101` to `ACCT-115`) that do not map to CRM.

Field definitions and known data-quality conditions are documented in the [data dictionary](Reference%20Docs/novatech_data_dictionary.md).

## Repository guide

```text
.
|-- 01_verification/        Data checks and source-import evidence
|-- 02_data_preparation/    Type corrections, calculated fields, joins, and SPICE evidence
|-- 03_dashboard/           Dashboard configuration evidence and PDF exports
|-- 04_topic/               Amazon Q topic configuration and before/after tests
|-- 05_exploration/         Q exploration results and reflection
|-- 06_report/              Executive report PDF and supporting summary text
|-- Reference Docs/         Company brief, dashboard brief, and data dictionary
`-- Structured Data/        Source CSV files
```

## How to review or reproduce

1. Read the executive report for the findings, recommendations, and limitations.
2. Review the annotated dashboard PDFs for visual evidence supporting the report.
3. Read the company background and dashboard requirements in `Reference Docs/`.
4. Import the three CSV files from `Structured Data/` into Amazon Quick Suite or QuickSight.
5. Apply the type corrections and account-level aggregation steps documented in the data-preparation log.
6. Load the three source datasets and the unified dataset into SPICE.
7. Recreate the calculated fields, visuals, filters, and Q topic using the configuration screenshots.
8. Compare the result with the exported dashboard PDFs and the Q exploration log.

The repository contains source data, documentation, screenshots, and PDF exports. It does not contain a portable editable QuickSight analysis asset, so the interactive dashboard cannot be reproduced locally without access to Amazon Quick Suite or QuickSight.

## Analytical limitations

- The unified dataset has one row per support ticket. CRM and marketing account-level totals therefore repeat for every ticket associated with the account. Account-level measures must be aggregated once per account, for example with `max`, before they are totalled across accounts. Summing `Total Revenue Won` across ticket rows inflates the Customer Health revenue values.
- The Q1 2025 days-to-close result is based on 28 deals closed in January 2025. It is not a complete quarter and should be compared with full-quarter results cautiously.
- The Customer Sentiment donut is sized by the high-severity indicator. Its centre total of 450 represents high-severity tickets, not all 3,000 tickets, and the empty category represents non-high-severity rows.
- Risk labels are descriptive indicators based on ticket volume, negative sentiment, and deal value. The data contains no observed churn outcome, so the dashboard does not measure churn probability.
- Missing `annual_income` affects 24 of 2,240 marketing rows (1.1%). A separate set of 59 support rows has no sentiment value. The 59 unresolved tickets have no resolution timestamp and are excluded from average resolution-time calculations.
- The source contains three duplicated `opportunity_id` values and four duplicated `ticket_id` values. Row-count KPIs therefore represent records, not distinct identifiers.

## Tools and skills demonstrated

- Amazon Quick Suite / QuickSight dashboard authoring
- SPICE dataset management
- Multi-source data preparation and joins
- Calculated fields and KPI design
- Amazon Q topic configuration and natural-language testing
- Business-focused data validation, interpretation, and documentation
