## **Business Insights**
![Cover Page](screenshots/05_dashboard_overview/Cover_Page.png)
**Insight 1: Missing customer identification represents a measurable share of revenue.**
![Executive Overview](screenshots/05_dashboard_overview/01_executive_overview.png)

The Identified versus Unknown Customer Sales chart on Page 2 shows that 13.35 percent of sales value, approximately £0.51 million out of £3.82 million in total sales, is associated with transactions lacking a customer identifier. This is not a small edge case: it represents roughly one in every seven pounds of revenue that cannot currently be attributed to a specific customer.

The practical effect reaches further than the pie chart itself. The `Unique Customers` measure explicitly excludes the Unknown segment by design, so the reported customer count of 3,412 understates total buying activity rather than the number of people transacting. Any analysis that depends on customer identity, repeat purchase behaviour, customer lifetime value, or targeted marketing segmentation, can only be performed reliably on the 86.65 percent of revenue tied to identified customers. The remaining 13.35 percent is effectively invisible to customer level analysis even though it is fully visible in aggregate sales figures.

This pattern traces directly back to Power Query Transformation 3, where missing CustomerID values were deliberately labelled Unknown rather than deleted, precisely so that this gap could be measured and reported rather than silently discarded. The Diagnostic Insights page treats this as a monitored metric rather than a resolved problem.

*Recommendation:* Investigate whether missing CustomerID values are concentrated in a specific checkout flow, payment method, or guest checkout option, since this is often a process gap rather than a data gap. If a large share of the Unknown segment originates from one identifiable source, capturing an email address or account identifier at that point could recover a meaningful share of the £0.51 million currently outside customer level analysis. The `Sales from Unknown Customers` measure can be tracked monthly as a KPI to confirm whether this improves after any process change.

**Insight 2: Cancellation activity affects a material proportion of invoices.**

![Detailed Analysis](screenshots/05_dashboard_overview/01_detailed_analysis..png)

The `Cancellation Rate Percent` KPI on Page 3 reports a rate of 18.71 percent, meaning close to one in every five invoices in the six month dataset was subsequently cancelled. This figure was derived directly from the 4,394 cancellation invoices confirmed and validated during Power Query Transformation 6, so it reflects genuine, verified cancellation activity rather than an artifact of the negative quantity records that required cleaning elsewhere in the dataset.

The Cancelled Sales Trend line chart adds a time dimension to this figure: cancellation value does not stay flat across the six months, it rises sharply in March, alongside the same period where the Monthly Sales Trend on Page 1 also peaks, dips through April, and climbs again toward June. This co-movement between gross sales and cancelled sales is worth attention in its own right. It suggests cancellation volume may scale with overall order volume rather than remaining a constant background rate, which would mean periods of highest sales growth are also the periods generating the most cancellation exposure.

*Recommendation:* Segment the cancellation rate by product and by country, using the same `Cancellation Rate Percent` measure filtered through the existing slicers, to determine whether cancellations are broadly distributed or concentrated in specific stock items or markets. If concentrated, this points toward a fulfilment, description accuracy, or shipping issue tied to specific products rather than a general customer behaviour pattern, and would justify a targeted rather than dataset wide response.

**Insight 3: Revenue is heavily concentrated in a small number of products and one country.**

![Diagnostic Insights](screenshots/05_dashboard_overview/01_diagnostic_insights.png)

The Top 10 Products charts on Pages 1 and 2 show that a small set of products, led consistently by the white hanging heart tea light holder and the regency cakestand, account for a disproportionate share of both revenue and quantity sold. This concentration holds under two different rankings: by revenue on Page 1, and by quantity sold on Page 2, which indicates the top products are genuinely high performing on both dimensions rather than appearing high revenue only because of an inflated unit price on a low volume item.

Geographic concentration is more extreme still. The Geographic Sales Distribution visuals confirm the United Kingdom as the dominant market, generating sales at a scale that dwarfs every other country combined, with the next largest markets, Netherlands, EIRE, Germany, and France, each contributing a comparatively small fraction. With `Total Countries` reporting 32 distinct markets in the dataset, the practical reality is that the business operates as a UK domestic retailer with a long tail of minor international activity rather than a genuinely diversified international operation.

*Recommendation:* Treat the top 10 products as the business's core inventory priority for stock availability and supplier reliability, since a disruption to any one of these items would have an outsized effect on total revenue. Separately, the geographic concentration represents both a risk, dependency on a single national market, and a rarely-tested opportunity: the `Average Transaction Value by Country` chart on Page 2 can be reviewed to identify whether any of the smaller markets show above-average order value despite low volume, which would indicate an under-served market worth deliberate expansion rather than one that is simply small by coincidence.

---