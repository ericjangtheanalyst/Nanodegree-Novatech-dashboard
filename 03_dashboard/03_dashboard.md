# NovaTech Dashboard Configuration Log

- **Student:** Eric Jang
- **Date:** 2026-09-06
- **Purpose:** Document dataset use, interactive filters, one-click filtering, and cross-sheet navigation in the NovaTech dashboard.

## 1. Customer Health Sheet

The Customer Health sheet uses two datasets: the Customer Health source dataset and the unified Customer Health dataset. The source dataset supports ticket-level measures, while the unified dataset adds account-level CRM and marketing context.

![Customer Health sheet using the Customer Health source dataset](images/image-24.png)
![At-risk accounts table using the unified Customer Health dataset](images/image-5.png)

## 2. Interactive Filter Controls

Interactive filters allow NovaTech stakeholders to narrow each dashboard sheet by relevant segment, region, or time period.

### Marketing Funnel

![Marketing Funnel filter-control configuration](images/image-14.png)

**Before applying a filter**

![Marketing Funnel dashboard before applying a filter](images/image-6.png)

**After applying a filter**

![Marketing Funnel dashboard after applying a filter](images/image-7.png)

### Sales Pipeline

![Sales Pipeline filter-control configuration](images/image-13.png)

**Before applying a filter**

![Sales Pipeline dashboard before applying a filter](images/image-8.png)

**After applying a filter**

![Sales Pipeline dashboard after applying a filter](images/image-11.png)

### Customer Health

![Customer Health filter-control configuration](images/image-12.png)

**Before applying a filter**

![Customer Health dashboard before applying a filter](images/image-9.png)

**After applying a filter**

![Customer Health dashboard after applying a filter](images/image-10.png)

### Cross-dataset filter configuration

Because the Customer Health sheet uses two datasets, **Apply cross-datasets** was enabled so its filter controls apply to compatible visuals from both datasets.

![Customer Health filter configured to apply across datasets](images/image-15.png)

Field mapping links matching columns across the two datasets so one control can filter the relevant KPIs and visuals consistently.

![Cross-dataset field mapping for the Customer Health sheet](images/image-16.png)

## 3. One-Click Filtering

One-click filtering allows a stakeholder to select a value in one visual and apply it as a filter to connected visuals on the same sheet.

### Sales Pipeline example

A bar in the **Revenue by Product** chart was selected to demonstrate how the other Sales Pipeline visuals respond.

#### View 1

**Before selection**

![Sales Pipeline product and customer-size visuals before selecting a product](images/image-18.png)

**After selection**

![Sales Pipeline product and customer-size visuals after selecting a product](images/image-19.png)

#### View 2

**Before selection**

![Sales Pipeline outcome visuals before selecting a product](images/image-20.png)

**After selection**

![Sales Pipeline outcome visuals after selecting a product](images/image-21.png)

The action was configured in the visual properties and applied to multiple target visuals.

![One-click filtering action configured in visual properties](images/image-22.png)
![Target visuals selected for the one-click filtering action](images/image-23.png)

## 4. Cross-sheet navigation

Cross-sheet navigation allows stakeholders to move between dashboard sheets while retaining the selected business context. Two fields were configured as cross-sheet filters: **Customer Tier** and **Company Name**.

![Customer Health sheet showing the cross-sheet filter controls](images/image-1.png)
![Customer Tier configured as a cross-sheet filter](images/image-2.png)
![Company Name configured as a cross-sheet filter](images/image-3.png)
