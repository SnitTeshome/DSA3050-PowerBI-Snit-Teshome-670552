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
# 2. Section B — Power Query: Data Cleaning & Transformation

**Marks: 20**

## 2.1 Section B Requirement

The assignment requires Power Query to be used extensively to transform the raw dataset into a form suitable for analysis.

The examination specifically states that:

> "Simply importing a clean CSV/Excel file and clicking Close & Apply will receive very limited marks."

Therefore, the Power Query process was based on the actual data-quality problems identified during the initial profiling in Section A.

The cleaning approach was **evidence-based**. Transformations were not performed simply to increase the number of steps. Each significant transformation was justified using the structure:

**Problem → Transformation → Reason → Result**

The Power Query workflow includes more than the required **8 significant transformations**.

---

# 2.2 Power Query Cleaning Strategy

The main data-quality issues identified during the initial profiling were:

1. Incorrect/uncertain data types
2. An identified test record (`TEST001`)
3. Missing and inconsistently formatted `CustomerID` values
4. Negative quantities
5. Cancellation transactions
6. Non-cancellation negative quantities
7. Zero-price transactions
8. Negative-price transactions
9. Duplicate records
10. The need for final data-quality validation

The transformations were applied sequentially so that each cleaning step builds on the result of the previous step.

The overall workflow was:

**Raw Data**
↓
**Correct Data Types**
↓
**Remove TEST001**
↓
**Clean CustomerID**
↓
**Investigate Negative Quantity**
↓
**Identify Cancellation Invoices**
↓
**Remove Non-Cancellation Negative Quantities**
↓
**Investigate/Handle Zero Prices**
↓
**Investigate/Handle Negative Prices**
↓
**Remove Duplicates**
↓
**Final Data Validation**
↓
**Clean Analytical Dataset**

---

# 2.3 Transformation 1 — Correct Data Types

### Problem

The raw CSV contains fields representing identifiers, text, numerical values and date/time information. These fields must have appropriate data types before filtering, calculations and modelling.

### Transformation

Power Query was used to explicitly assign appropriate data types:

- `Invoice` → Text
- `StockCode` → Text
- `Description` → Text
- `Quantity` → Whole Number
- `InvoiceDate` → Date/Time
- `Price` → Decimal Number
- `CustomerID` → Text
- `Country` → Text

### Reason

Correct data types ensure that Power Query performs filtering, comparisons, calculations and later DAX operations correctly.

For example, `InvoiceDate` must be recognized as a date/time field for time-based analysis, while `Quantity` must be numeric for sales calculations.

### Result

All eight variables were assigned appropriate analytical data types.

### 📸 Screenshot Evidence

![Data Type Investigation](screenshots/02_power_query/12_datatype_investigation.png)

---

# 2.4 Transformation 2 — Remove TEST001

### Problem

During the initial data profiling, a record with:

`StockCode = TEST001`

was identified.

The corresponding description indicated that it was a test product rather than a genuine retail product.

### Transformation

Power Query was used to filter out the record where:

`StockCode = TEST001`

### Reason

A test transaction does not represent a genuine business transaction. Keeping it could affect product counts, transaction counts and other analytical results.

### Result

The identified test record was removed while the remaining valid transactions were retained.

### 📸 Before Transformation

![TEST001 Before Removal](screenshots/02_power_query/01_test_records_before.png)

### 📸 After Transformation

![TEST001 Removed](screenshots/02_power_query/02_test_records_removed.png)

---

# 2.5 Transformation 3 — Clean and Standardize CustomerID

### Problem

The `CustomerID` field contained missing values and inconsistent identifier formatting.

Some customer identifiers were also represented with unnecessary decimal formatting such as `.0`.

### Transformation

The `CustomerID` field was transformed so that:

- Missing values were represented as `Unknown`
- Blank values were handled
- Leading/trailing spaces were removed
- Unnecessary `.0` formatting was removed

### Reason

Missing `CustomerID` values do not necessarily mean that the associated transaction is invalid.

Deleting these transactions would unnecessarily reduce the amount of valid sales information available for analysis.

Representing missing identifiers as `Unknown` allows the transaction to remain in the dataset while clearly indicating that the customer identity is unavailable.

### Result

The `CustomerID` field was standardized while valid transactions with missing customer information were retained.

### 📸 Investigation

![Missing CustomerID Investigation](screenshots/02_power_query/03_missing_customerid_investigation.png)

### 📸 Before Cleaning

![CustomerID Before Cleaning](screenshots/02_power_query/04_missing_customerid_before.png)

### 📸 Query Evidence

![CustomerID Query](screenshots/02_power_query/05_missing_customerid_handled_Query.png)

### 📸 After Cleaning

![CustomerID Handled](screenshots/02_power_query/06_missing_customerid_handled.png)

---

# 2.6 Transformation 4 — Investigate Negative Quantities

### Problem

The `Quantity` field contained negative values.

A negative quantity cannot automatically be assumed to be an error because retail datasets may contain returns or cancellation transactions.

### Transformation

All negative quantities were first isolated and investigated.

The invoice identifiers were then examined to determine whether the negative quantity was associated with a cancellation invoice.

### Reason

Automatically deleting every negative quantity could remove legitimate business events.

The analysis therefore distinguished between:

- Negative quantities associated with cancellation invoices
- Negative quantities that were not cancellation invoices

### Result

Negative quantities were classified before the cleaning decision was made.

### 📸 Investigation Evidence

![Negative Quantity Investigation](screenshots/02_power_query/09_negative_quantity_investigation.png)

### 📸 Summary Evidence

![Negative Quantity Summary](screenshots/02_power_query/10_negative_quantity_summary.png)

---

# 2.7 Transformation 5 — Identify and Validate Cancellation Invoices

### Problem

Some invoice identifiers begin with the letter `C`.

For example:

`C493411`

These records represent cancellation transactions and therefore require separate treatment.

### Transformation

Power Query was used to identify invoices beginning with `C`.

The cancellation records were then investigated based on:

- Quantity
- Price
- Invoice identifier

### Reason

Cancellation transactions are meaningful business events.

They should not automatically be deleted because the final analysis may need to measure cancellation activity.

### Result

Cancellation transactions were identified and retained as part of the analytical transaction history.

### 📸 Cancellation Investigation

![Cancellation Invoice Investigation](screenshots/02_power_query/11_cancellation_invoice_investigation.png)

### 📸 Cancellation Summary

![Cancellation Summary](screenshots/02_power_query/13_cancellation_summary.png)

### 📸 Cancellation Validation

![Cancellation Validation](screenshots/02_power_query/14_cancellation_validation.png)

### 📸 Final Cancellation Results

![Cancellation Final Results](screenshots/02_power_query/15_cancellation_final_results.png)

---

# 2.8 Transformation 6 — Remove Non-Cancellation Negative Quantities

### Problem

After investigating negative quantities, some negative-quantity records were found that were **not cancellation invoices**.

These records could distort quantity and revenue calculations.

### Transformation

The final transaction table was filtered to retain:

- Transactions with non-negative quantities
- Cancellation invoices

Negative quantities that were not associated with cancellation invoices were removed.

### Reason

This approach preserves legitimate cancellation activity while preventing unexplained negative quantities from distorting the analytical dataset.

### Result

Non-cancellation negative quantities were removed, while cancellation transactions were retained.

### 📸 Evidence

![Negative Quantity Cleaning Evidence](screenshots/02_power_query/10_negative_quantity_summary.png)

---

# 2.9 Transformation 7 — Investigate and Handle Zero-Price Transactions

### Problem

The initial profiling identified:

**2,051 records with `Price = 0`.**

A zero price may represent a legitimate business situation or an invalid transaction, so these records required investigation before removal.

### Transformation

Zero-price transactions were isolated and investigated separately from cancellation transactions.

The records determined to be invalid were removed from the final analytical transaction table.

### Reason

Revenue is calculated using:

**Revenue = Quantity × Price**

Therefore, invalid zero-price transactions could affect revenue and product-performance analysis.

The records were investigated before removal rather than being deleted automatically.

### Result

The invalid zero-price records identified during the investigation were removed from the analytical dataset.

### 📸 Investigation Evidence

![Zero Price Investigation](screenshots/02_power_query/08_zero_price_investigation.png)

### 📸 Cleaning Evidence

![Zero Price Removed](screenshots/02_power_query/07_zero_price_removed.png)

---

# 2.10 Transformation 8 — Investigate and Handle Negative Prices

### Problem

The `Price` field contained negative values.

Negative unit prices are not appropriate for ordinary sales revenue calculations and therefore required investigation.

### Transformation

Negative-price records were isolated and examined before the invalid records were removed.

### Reason

The `Price` field contributes directly to revenue calculations.

Retaining invalid negative prices could produce misleading revenue and product-performance results.

### Result

The identified invalid negative-price records were removed from the cleaned analytical dataset.

### 📸 Investigation Evidence

![Negative Price Investigation](screenshots/02_power_query/16_negative_price_investigation.png)

### 📸 Cleaning Evidence

![Negative Price Removed](screenshots/02_power_query/17_removed_negative_price.png)

---

# 2.11 Transformation 9 — Remove Duplicate Records

### Problem

Duplicate transaction records can cause the same transaction line to be counted more than once.

This can artificially increase:

- Revenue
- Quantity
- Transaction counts
- Product performance
- Customer performance

### Transformation

Duplicate records were investigated in Power Query and confirmed duplicate records were removed.

### Reason

Each valid transaction line should contribute only once to the analytical dataset.

Removing confirmed duplicates prevents double counting while preserving distinct legitimate transactions.

### Result

Confirmed duplicate records were removed.

### 📸 Duplicate Investigation

![Duplicate Investigation](screenshots/02_power_query/18_duplicate_investigation.png)

### 📸 Duplicates Removed

![Duplicates Removed](screenshots/02_power_query/19_removed_duplicates.png)

---

# 2.12 Final Data Quality Validation

After completing the significant Power Query transformations, the cleaned transaction table was subjected to a final validation.

The purpose of this step was to verify that:

- The identified test record was removed
- CustomerID was handled consistently
- Non-cancellation negative quantities were removed
- Valid cancellation transactions were retained
- Invalid price records were handled
- Duplicate records were removed
- Data types remained appropriate
- The final dataset was suitable for modelling and analysis

### Result

The final transaction table represents the cleaned analytical dataset that will be used for the subsequent **Data Modelling** stage.

### 📸 Final Power Query Validation

![Final Power Query Cleaning Validation](screenshots/02_power_query/20_power_query_final_cleaning_validation.png)

---

# 2.13 Complete Power Query Applied Steps

The individual screenshots above provide evidence for the important transformation decisions.

The final screenshot below provides evidence of the **complete Power Query workflow**, showing that the transformations were actually applied sequentially rather than simply describing them in the README.

### 📸 Complete Applied Steps

![Complete Power Query Applied Steps](screenshots/02_power_query/20_power_query_final_cleaning_validation.png)

---

# 2.14 Section B — Transformation Summary

The following table demonstrates that the Power Query work satisfies the examination requirement of documenting significant transformations using:

**Problem → Transformation → Reason → Result**

| # | Problem | Transformation | Reason | Result |
|---|---|---|---|---|
| 1 | Fields required appropriate analytical types | Corrected data types | Ensure reliable filtering, calculations and modelling | Consistent data types |
| 2 | `TEST001` identified as a test record | Removed `TEST001` | Prevent test data from affecting analysis | Test record removed |
| 3 | Missing/inconsistent CustomerID values | Cleaned and standardized CustomerID | Preserve valid transactions while standardizing identifiers | CustomerID standardized |
| 4 | Negative quantities identified | Investigated negative quantities | Distinguish cancellations from invalid negatives | Negative quantities classified |
| 5 | Cancellation invoices identified | Isolated and validated `C...` invoices | Preserve meaningful cancellation activity | Cancellation records retained |
| 6 | Non-cancellation negative quantities identified | Removed non-cancellation negatives | Prevent invalid quantities from distorting analysis | Invalid negative quantities removed |
| 7 | Zero-price records identified | Investigated and handled invalid zero-price records | Protect revenue calculations | Invalid zero-price records removed |
| 8 | Negative prices identified | Investigated and removed invalid negative prices | Prevent distorted revenue calculations | Invalid negative-price records removed |
| 9 | Duplicate records identified | Removed confirmed duplicates | Prevent double counting | Duplicate records removed |
| 10 | Final cleaned dataset required | Performed final validation | Confirm cleaning decisions and dataset integrity | Dataset ready for modelling |

---

# 2.15 Section B Marking Criteria Coverage

The Power Query work addresses the examination requirements as follows:

### ✔ Extensive Power Query Use

The raw transaction dataset was transformed through multiple sequential Power Query steps rather than simply importing the CSV and loading it directly.

### ✔ Transformations Based on Actual Problems

Each major transformation was based on an issue identified during the initial dataset profiling in Section A.

### ✔ At Least 8 Significant Transformations

More than eight significant transformations have been documented.

### ✔ Problem → Transformation → Reason → Result

Every significant transformation is explicitly documented using the required structure.

### ✔ Screenshot Evidence

Screenshots are included immediately after the relevant transformation to demonstrate the actual Power Query work.

### ✔ Final Applied Steps Evidence

The final Power Query screenshot demonstrates the complete sequence of transformations applied to the dataset.

### ✔ Evidence-Based Cleaning

The project avoids aggressive deletion of records. Potentially meaningful business events, particularly cancellations, were investigated before cleaning decisions were made.

---

# 2.16 Section B Completion Status

**Section B — Power Query Data Cleaning & Transformation: COMPLETED**

The cleaned transaction dataset is now ready to proceed to:

**Section C — Data Modelling**
---