# NovaTech Topic Configuration

- **Student:** Eric Jang
- **Date:** 2026-09-13
- **Purpose:** Record the Amazon Q topic configuration and compare question performance before and after configuration.

## Before-topic baseline

**Question 1: What is the total revenue from closed-won deals?**
![Baseline Q response for total closed-won revenue](images/image.png)

**Question 2: Which campaign channel has the highest conversion rate?**
![Baseline Q response for the highest-converting campaign channel](images/image-1.png)
![Baseline conversion-by-channel results](images/image-2.png)

**Question 3: What is the average resolution time for critical tickets compared with low-priority tickets?**
![Baseline Q response comparing critical and low-priority resolution time](images/image-3.png)
![Baseline resolution-time interpretation](images/image-4.png)

## Topic configuration

![NovaTech Revenue Intelligence topic setup](images/image-25.png)

### Question 1: Total revenue from closed-won deals
**Question 1: What is the total revenue from closed-won deals?** 

- **Relevant columns:** `Deal Value ($USD)` and `Deal Stage`
- **Rationale:** These fields define the `Won Revenue` calculation used by the KPI.

```text
sumIf({Deal Value ($USD)}, {Deal Stage} = 'Won')
```

**Column: 'Deal Value ($USD)'**
![Topic configuration for Deal Value in USD](images/image-27.png)

**Column: 'Deal Stage'**
![Topic configuration for Deal Stage](images/image-26.png)

### Question 2: Highest campaign-channel conversion rate

**Question 2: Which campaign channel has the highest conversion rate?** 

- **Relevant columns:** `Campaign Channel`, `Is Closed (Won)`, and `Lead ID`
- **Rationale:** These fields define `Lead-to-Deal Conversion` and allow the result to be grouped by channel.

```text
sum({Is Closed (Won)}) / count({Lead ID})
```

**Column: 'Is Closed (Won)'**
![Topic configuration for the Is Closed Won field](images/image-28.png)
![Boolean synonyms for the Is Closed Won field](images/image-29.png)

**Column: 'Campaign Channel'**
![Topic configuration for Campaign Channel](images/image-30.png)

**Column: 'Lead ID'**
![Topic configuration for Lead ID](images/image-31.png)

### Question 3: Resolution time by priority

**Question 3: What is the average resolution time for critical tickets compared with low-priority tickets?** 

- **Relevant columns:** `Priority` and `Resolution Hours`
- **Rationale:** These fields define the average resolution-time measure and allow comparison by priority.

```text
avg({Resolution Hours})
```

**Column: 'Priority'**
![Topic configuration for ticket Priority](images/image-32.png)

**Column: 'Resolution Hours'**
![Topic configuration for Resolution Hours](images/image-33.png)

## After-topic results

**Question 1: What is the total revenue from closed-won deals?**
![Configured Q response showing total won revenue](images/image-5.png)

**Question 2: Which campaign channel has the highest conversion rate?**
![Configured Q response identifying Direct Mail as the highest-converting channel](images/image-6.png)
![Configured conversion-by-channel table](images/image-7.png)

**Question 3: What is the average resolution time for critical tickets compared with low-priority tickets?**
![Configured Q response comparing critical and low-priority resolution time](images/image-8.png)
![Configured resolution-time interpretation by priority](images/image-9.png)

## Additional exploration questions

**Question 4: Are there any campaigns where we spent more than we earned back?  (Marketing)**
![Q table comparing campaign spend with attributed revenue](images/image-10.png)
![Q interpretation of campaign profitability](images/image-11.png)

**Question 5: What is the average deal size by company size? (CRM Deals)**
![Q response for average deal size by company size](images/image-12.png)
![Q clarification that the returned values are totals rather than averages](images/image-13.png)

**Question 6: What are the top 10 accounts by support ticket volume, and what is their total deal revenue? (Cross-Dataset)**
![Q response for the top accounts by support-ticket volume](images/image-17.png)
![Top-account results continued](images/image-19.png)
![Q interpretation of the top-account results](images/image-20.png)

**Question 7: What is the average deal size for accounts with more than 3 support tickets in the last 30 days? (Cross-Dataset)**
![Q response explaining why the 30-day account question is not supported](images/image-23.png)
![Alternative questions suggested by Q](images/image-24.png)

**Question 8: What is our win rate for deals from accounts sourced through Partner Referral? (Cross-Dataset)**
![Q response explaining the missing Partner Referral to deal relationship](images/image-21.png)
![Q response showing related but non-equivalent available metrics](images/image-22.png)

## Configuration outcome

The configured topic answered questions reliably when the required fields, calculations, and dashboard visuals were available. It did not reliably answer questions that required a metric absent from the visual, an unsupported time-window aggregation, or a relationship removed during account-level aggregation. These boundaries are documented in the [Q exploration log](../05_exploration/05_NovaTech_Q_Exploration_Log.md).
