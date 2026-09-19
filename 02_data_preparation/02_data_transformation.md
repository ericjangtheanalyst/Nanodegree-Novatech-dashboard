# NovaTech Data Transformation and Join Log

- **Student:** Eric Jang
- **Date:** 2026-09-06
- **Purpose:** Document data-type corrections, calculated fields, account-level aggregation, joins, and SPICE storage.

## 1. Data-type corrections

I used the **Change data types** step to correct fields that were inferred incorrectly. For example, `annual_revenue_usd` and `deal_value` in the CRM dataset contain decimal values and were changed from integer to decimal.

### CRM deals

![CRM field type corrections](images/image-1.png)

### Marketing campaigns

![Marketing field type corrections](images/image-2.png)

### Customer Health

![Customer Health field type corrections](images/customer-health-dataset.png)

## 2. Calculated fields

I created calculated fields in each dataset to support the required KPIs and visuals. These include revenue and outcome measures for CRM, response and conversion measures for marketing, and resolution-time, severity, and risk measures for support.

### CRM deals

![CRM calculated-field steps](images/image-3.png)

### Marketing campaigns

![Marketing calculated-field steps](images/image-4.png)

### Customer Health

![Customer Health calculated-field steps](images/image-5.png)

## 3. Unified dataset

The unified dataset supports the Customer Health view by connecting support activity with account-level CRM and marketing measures. `account_id` is the common join key. The support-ticket dataset is the left-side table, so the resulting grain is one row per support ticket and all 3,000 support rows are retained.

![Full transformation flow for the unified Customer Health dataset](images/image-10.png)

### Join strategy

Directly joining the raw files would create a many-to-many row expansion because each account can have multiple deals, marketing leads, and tickets. To prevent this, I first reduced the CRM and marketing datasets to one summary row per account. I then left-joined those summaries to the 3,000-row support-ticket dataset.

#### CRM aggregation

Starting with the raw CRM dataset, I created two account-level summaries:

- **Total Deal Value:** Sum of `deal_value` by `account_id`.
- **Total Revenue Won:** Sum of `deal_value` by `account_id` after filtering `deal_stage` to `Won`.

I rejoined the summaries on `account_id`. The transformation step shown as **Max Deal Value** is labelled inconsistently in the interface screenshot; its configured aggregation is **Sum**.

![CRM aggregation flow and account-level total deal value](images/image-16.png)
![Account-level won-revenue aggregation after filtering for won deals](images/image-14.png)

#### Marketing aggregation

I aggregated the marketing data by `account_id` to calculate lead count, total campaign spend, total attributed revenue, and total campaign responses.

![Marketing measures aggregated to one row per account](images/image-15.png)

The CRM and marketing summaries each contain one row per account. Joining them to the ticket-level support data preserves the support grain and produces 3,000 rows.

> **Grain limitation:** Account-level CRM and marketing values repeat on every support-ticket row for the same account. These fields must be aggregated once per account, such as with `max`, before they are totalled across accounts. Summing them across ticket rows will overstate revenue, spend, leads, and responses.

#### Left joins

**1. Support Tickets to the CRM account summary on `account_id`**

![First left join between support tickets and the CRM account summary](images/image-11.png)

**2. First joined result to the marketing account summary on `account_id`**

![Second left join adding the marketing account summary](images/image-12.png)

## 4. SPICE confirmation

All four prepared datasets were saved to SPICE.

![Amazon Quick datasets tab showing the datasets stored in SPICE](images/datasets-saved-to-spice.png)

### CRM deals

![Transformed CRM dataset summary in SPICE](images/crm-transformed-dataset-summary.png)
![CRM analysis created from the SPICE dataset](images/crm-analysis-created.png)
![Sales Pipeline dashboard loaded from SPICE](images/sales-dashboard-spice-confirmation.png)

### Marketing campaigns

![Transformed marketing dataset summary in SPICE](images/marketing-transformed-dataset-summary.png)
![Marketing analysis created from the SPICE dataset](images/marketing-analysis-created.png)
![Marketing Funnel dashboard loaded from SPICE](images/marketing-dashboard-spice-confirmation.png)

### Customer Health

![Transformed Customer Health dataset summary in SPICE](images/support-transformed-dataset-summary.png)
![Customer Health analysis created from the SPICE dataset](images/support-analysis-created.png)
![Customer Health dashboard loaded from SPICE](images/customer-health-dashboard-spice-confirmation.png)

### Unified Customer Health

![Unified Customer Health dataset summary in SPICE](images/unified-dataset-summary.png)
![Unified Customer Health analysis created from the SPICE dataset](images/unified-analysis-created.png)
![Customer-risk visual loaded from the unified SPICE dataset](images/customer-risk-visual-spice-confirmation.png)
