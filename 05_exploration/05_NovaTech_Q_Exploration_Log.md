# NovaTech Q Exploration Log

- **Student:** Eric Jang
- **Date:** 2026-09-13
- **Purpose:** Test natural-language questions and compare Q's responses with the available dashboard evidence.

> **Interpretation note:** A “Yes” result means Q matched the dashboard visual. It does not by itself confirm that the dashboard measure is valid at the source-data grain.

## Q Exploration Questions

### Baseline questions

| # | Question Asked | Q's Answer (summarize) | Dashboard Visual Used to Cross-Check | Dashboard Shows | Match? | Notes |
|---|---------------|----------------------|--------------------------------------|-----------------|--------|-------|
| 1 | What is the total revenue from closed-won deals? | $707,201 | Sales Pipeline: Won Revenue | $707,201 | Yes | Source CSV cross-check also equals $707,201. |
| 2 | Which campaign channel has the highest conversion rate? | Direct Mail at 48.99% | Marketing Funnel: Conversion by Channel | Direct Mail at 48.99% | Yes | The measure is the share of marketing leads whose `funnel_stage` is `Closed Won`. |
| 3 | What is the average resolution time for critical tickets compared with low-priority tickets? | Critical: 56.13 hours; low: 58.92 hours | Customer Health: Average Resolution Hours by Priority | Critical: 56.13 hours; low: 58.92 hours | Yes | The calculation uses whole-hour date differences and excludes unresolved tickets. |

### Domain-specific questions

| # | Question Asked | Q's Answer (summarize) | Dashboard Visual Used to Cross-Check | Dashboard Shows | Match? | Notes |
|---|---------------|----------------------|--------------------------------------|-----------------|--------|-------|
| 4 | Are there any campaigns where we spent more than we earned back? | Yes. Spend exceeded attributed revenue for all six campaigns. | Marketing Funnel: Spend vs Revenue | Spend exceeds attributed revenue for all six campaigns. | Yes | Requires direct navigation to the combo chart. |
| 5 | What is the average deal size by company size? | Returned total deal value by customer tier and product category rather than an average per deal. | Sales Pipeline: Revenue by Customer Size | Not available | No | The visual contains total deal value, so Q could not answer the requested average reliably. |
| 6 | What are the top 10 accounts by support ticket volume, and what is their total deal revenue? | Reproduced the 10 accounts and revenue figures shown in the Customer Health table. | Customer Health: At-Risk Accounts | Same account ranking and displayed values | Yes | The displayed revenue is inflated because an account-level value is summed across ticket rows. See the caveat below. |
| 7 | What is the average deal size for accounts with more than three support tickets in the last 30 days? | No reliable answer | Customer Health: At-Risk Accounts | Not available | No | The topic does not expose the required account-level 30-day ticket threshold together with per-deal values. The source has `tickets_last_30_days`, but the required aggregation and metric are not available in the tested visual. |
| 8 | What is our win rate for deals from accounts sourced through Partner Referral? | No reliable answer | Marketing Funnel: Conversion by Channel | Not available | No | Campaign channel is not retained with deal outcomes in the account-level unified dataset, so the requested cross-domain relationship cannot be calculated from the tested topic. |

### Data-grain caveat for question 6

The Customer Health dataset has one row per support ticket. `Total Revenue Won` is an account-level value repeated on each of those rows. Summing it in the table multiplies an account's revenue by its ticket count. For example, the table displays $13.6 million for YieldMax Software even though total won revenue across the entire CRM source is $707,201. The account ranking by ticket count is usable, but the displayed revenue values are not valid account totals. A corrected visual should use one value per account, such as `max(Total Revenue Won)`, or an account-grain dataset.

## Reflection

- **Where did Q agree with the dashboard?**  
  Q matched supported visuals for $707,201 in won revenue, Direct Mail's 48.99% conversion rate, and average resolution times of 56.13 hours for critical tickets and 58.92 hours for low-priority tickets. It also identified that every campaign's spend exceeded its attributed revenue and reproduced the dashboard's top-10 ticket-volume ranking.


- **Where did Q disagree or struggle, and why?**  
  Q struggled when the requested calculation was not available in a visual or when the topic lacked the required relationship. It substituted total deal value for average deal size, could not combine the 30-day account threshold with per-deal values, and could not connect Partner Referral sourcing to deal outcomes. Its narrative interpretation of the resolution-time gap also changed even though the figures did not.

- **When would you use Q versus the dashboard?**  
  I would use Q for quick questions that map directly to defined fields, calculations, and relationships. I would use the dashboard to monitor performance, apply filters, inspect trends, and verify Q's response against a visible measure. For questions outside the current semantic model, I would first add or correct the required relationship, calculation, or visual, then retest Q before using the result for a decision.
