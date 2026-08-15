# DSA3050-PowerBI-Snit-Teshome-670552

## DSA3050A Business Intelligence & Data Visualization
### Power BI Retail Sales Analytics Project

---

# 1. Dataset Selection & Understanding

## 1.1 Dataset Source

The dataset used for this Power BI project is the **Online Retail II** dataset from the **UCI Machine Learning Repository**.

**Source:** UCI Machine Learning Repository  
**Dataset:** Online Retail II  
**Original dataset:** Approximately 1.07 million transaction records  
**Original period:** December 2009 – December 2011

The original dataset is available from:

https://archive.ics.uci.edu/dataset/502/online%2Bretail%2Bii

The original dataset contains transaction-level records from a UK-based registered non-store online retailer. It provides information about invoices, products, quantities, transaction dates, prices, customers and countries.

---

## 1.2 Dataset Description

The Online Retail II dataset contains transaction-level information from an online retail business.

Each row represents an individual **product transaction line associated with an invoice**. Therefore, one invoice can contain multiple rows when a customer purchases multiple products.

The dataset contains information about:

- Transaction/invoice identification
- Product identification
- Product description
- Quantity purchased
- Transaction date and time
- Unit price
- Customer identification
- Customer country

This structure makes the dataset appropriate for business intelligence analysis because sales performance can be analyzed across different dimensions such as **time, product, customer and country**.

---

## 1.3 Dataset Selection Rationale

The Online Retail II dataset was selected because it satisfies the main dataset requirements for the DSA3050A practical examination.

The original dataset contains more than one million records, which is considerably larger than necessary for the practical examination. Therefore, a continuous **six-month analytical window** was selected from the original dataset.

The selected working dataset contains:

**209,875 records and 8 variables.**

Using a six-month analytical scope makes the Power BI project more manageable while still retaining enough records and business complexity for meaningful analysis.

The selected period is:

**4 January 2010 – 29 June 2010**

The dataset was intentionally kept in its original transaction-level form rather than using a pre-cleaned or summarized version.

This allows the project to demonstrate actual data profiling, cleaning, transformation, modelling, DAX development and dashboard construction.

---

## 1.4 Dataset Size and Scope

### Original Dataset

The original Online Retail II dataset contains approximately:

**1,067,371 transaction records**

covering approximately two years of retail activity.

### Selected Analytical Dataset

For this project, a continuous six-month period was selected to create a manageable analytical scope.

| Attribute | Value |
|---|---:|
| Records | 209,875 |
| Variables | 8 |
| Start Date | 4 January 2010 |
| End Date | 29 June 2010 |
| Distinct Invoice Values | 12,349 |
| Distinct Stock Codes | 4,034 |
| Distinct Product Descriptions | 3,744 |
| Distinct Customer IDs | 2,815 |
| Countries | 32 |

The selected period provides sufficient scale and analytical diversity while avoiding unnecessary processing of the entire 1+ million-row source dataset.

---

## 1.5 Main Variables

The dataset contains the following eight original variables:

| Variable | Data Type | Description | Analytical Role |
|---|---|---|---|
| `Invoice` | Text / Identifier | Identifies the transaction/invoice | Transaction identifier |
| `StockCode` | Text / Identifier | Identifies the product | Product identifier |
| `Description` | Text / Categorical | Describes the product | Product attribute |
| `Quantity` | Numerical | Number of units involved in the transaction | Sales quantity |
| `InvoiceDate` | Date/Time | Date and time of the transaction | Time dimension |
| `Price` | Numerical | Unit price associated with the transaction | Revenue calculation |
| `CustomerID` | Identifier | Identifies the customer | Customer dimension |
| `Country` | Categorical | Country associated with the customer | Geographic dimension |

### Numerical Variables

The main numerical variables are:

- `Quantity`
- `Price`

These variables can be used to calculate sales-related KPIs.

For example:

**Sales Amount = Quantity × Price**

### Categorical Variables

The main categorical variables include:

- `Description`
- `Country`

`StockCode` and `Invoice` are primarily identifiers rather than descriptive categorical variables.

### Date/Time Variable

`InvoiceDate` provides transaction date and time information and allows the analysis of:

- Daily sales
- Monthly sales
- Sales trends
- Month-over-month performance

### Identifier Variables

The main identifiers are:

- `Invoice`
- `StockCode`
- `CustomerID`

These identifiers will later support the data model and relationships between fact and dimension tables.

---

## 1.6 Initial Dataset Quality Assessment

Before applying any transformations, the dataset was profiled in Power Query using:

- Column Quality
- Column Distribution
- Column Profile

Column profiling was performed using the **entire dataset** rather than only the default preview rows.

### Initial findings

The raw dataset contains realistic data-quality issues that require investigation before modelling.

#### Missing CustomerID

Approximately **20% of CustomerID values are empty**.

This does not automatically mean that the corresponding sales transactions are invalid. Therefore, these records will not be removed without further investigation.

The impact of missing CustomerID will be considered when performing customer-level analysis.

#### Missing Description

The `Description` column contains a small number of missing values, representing **less than 1%** of the records.

These records will be investigated during Power Query rather than being automatically deleted.

#### Negative Quantity

Negative quantity values are present.

Negative quantities may represent cancellations, returns or reversed transactions. Therefore, they require investigation before any decision is made to remove them.

#### Cancellation Transactions

Some invoice values begin with `C`, for example:

`C493411`

This indicates a potential cancellation pattern that will be investigated during Power Query.

Rather than automatically deleting these records, the project will determine whether a cancellation indicator should be created.

#### Zero Price Values

The profiling identified:

**2,051 records with Price = 0.**

These records require investigation to determine whether they represent legitimate business cases, special transactions or invalid records.

#### Negative Price Values

The `Price` profile contains negative values and therefore requires further investigation.

The minimum observed value is unusually negative, so the affected records will be examined before deciding whether any transformation is necessary.

#### Test Product Record

The raw dataset contains a record with:

`StockCode = TEST001`

and:

`Description = This is a test product.`

This will be investigated during Power Query to determine whether it represents a genuine business record or a test transaction.

### Initial Quality Assessment

Based on the initial profiling, the dataset is considered to contain **manageable, realistic data-quality issues rather than severe corruption**.

The project will therefore follow a **light and evidence-based cleaning approach**.

No transformation will be performed simply to increase the number of transformations required by the assignment.

Each Power Query transformation will be based on an actual problem identified in the dataset.

---

## 1.7 Data Profiling Evidence

The following screenshots document the initial dataset inspection.

### Dataset Loading

The original six-month dataset was loaded into Power BI.

![Dataset Loading](screenshots/01_raw_data/01_loading_preview.png)

### Raw Power Query State

The dataset was opened in Power Query before any transformations were applied.

![Raw Power Query State](screenshots/01_raw_data/02_power_query_raw_state.png)

### Data Quality Baseline

Column Quality, Column Distribution and Column Profile were enabled to assess the dataset before cleaning.

![Data Quality Baseline 1](screenshots/01_raw_data/03_data_quality_baseline_1.png)

![Data Quality Baseline 2](screenshots/01_raw_data/03_data_quality_baseline_2.png)

### Date Range Verification

The `InvoiceDate` column was profiled to verify the analytical period.

![Date Range](screenshots/01_raw_data/04_date_range.png)

These screenshots provide evidence of the dataset state before Power Query transformations.

---

## 1.8 Potential KPIs

The dataset supports several meaningful retail KPIs.

### Total Revenue

Revenue can be calculated using:

**Revenue = Quantity × Price**

### Other potential KPIs

- Total Revenue
- Total Quantity Sold
- Total Orders
- Total Customers
- Average Order Value
- Average Unit Price
- Revenue per Customer
- Revenue per Product
- Cancellation Rate
- Average Items per Order
- Monthly Revenue Growth
- Revenue Contribution by Product
- Revenue Contribution by Country

These KPIs will be implemented using DAX after the Power Query and data modelling stages are completed.

---

## 1.9 Business / Analytical Problem

The main business problem is to understand **retail sales performance across time, products, customers and countries**.

The Power BI solution will investigate the major drivers of sales performance and identify areas that require management attention.

The analysis will focus on questions such as:

- Which products generate the most revenue?
- Which customers contribute most to revenue?
- Which countries generate the highest sales?
- How does revenue change over time?
- Which products have high or low sales volumes?
- How does average order value change?
- Are cancellations or unusual transactions affecting reported performance?

The final dashboard will convert the transaction-level data into an interactive business intelligence solution that supports decision-making.

---

## 1.10 Analytical Questions

The Power BI solution will answer the following analytical questions:

### Question 1
**How does revenue change over the six-month period?**

This will identify sales trends and changes in monthly performance.

### Question 2
**Which products generate the highest revenue?**

This will identify the major product-level revenue drivers.

### Question 3
**Which products have the highest sales volume?**

This will distinguish products that sell large quantities from products that generate high revenue because of higher prices.

### Question 4
**Which countries contribute the most revenue?**

This will provide a geographic view of business performance.

### Question 5
**Which customers generate the highest revenue?**

This will identify high-value customers and customer concentration.

### Question 6
**How does average order value change over time?**

This will help determine whether customers are spending more or less per order.

### Question 7
**What transaction patterns or unusual records require attention?**

This will investigate cancellations, negative quantities, zero prices, missing customer information and other data-quality patterns identified during profiling.

---

## 1.11 Section A Conclusion

The Online Retail II dataset satisfies the minimum dataset requirements for the DSA3050A Power BI practical examination.

The selected six-month dataset contains **209,875 records**, multiple numerical and categorical variables, date/time information, customer and product identifiers, and sufficient dimensional complexity for business intelligence analysis.

The initial Power Query profiling also confirmed that the dataset is not already completely cleaned or summarized. Instead, it contains manageable real-world issues that can be investigated and addressed through evidence-based Power Query transformations.

