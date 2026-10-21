# ShopSmart Retail Data Warehouse & Business Intelligence

**Business Intelligence | Data Warehousing | PostgreSQL | AWS | DBeaver | Power BI**

## Project Overview

ShopSmart is a retail Business Intelligence and data warehousing project developed to integrate sales, customer, product, store, promotion, and inventory data into a centralized analytical solution.

The business operates through online and physical retail channels, with data distributed across seven separate datasets. Differences in field names, data formats, missing values, and duplicate records make consistent reporting challenging.

To address these challenges, the project implements a dimensional data warehouse using **PostgreSQL hosted on AWS**, with **DBeaver** used for database management, SQL development, data transformation, and validation. Microsoft Power BI is used to visualize the consolidated data and present business insights through an interactive dashboard.

The project demonstrates the complete Business Intelligence workflow, from raw data profiling and ETL to dimensional modelling, warehouse implementation, analytical SQL, and management reporting.

## Project Objectives

- Integrate data from seven retail source datasets.
- Develop a centralized PostgreSQL data warehouse hosted on AWS.
- Connect to and manage the database using DBeaver.
- Clean and standardize inconsistent data using SQL.
- Remove verified duplicate records while preserving data integrity.
- Design a dimensional model using fact and dimension tables.
- Analyze sales performance across online and physical stores.
- Evaluate product, category, customer, store, and promotion performance.
- Monitor inventory levels and identify potential stock risks.
- Validate warehouse records and reconcile analytical measures.
- Develop a Power BI dashboard to support data-driven business decisions.

## 🛠️ Tools and Technologies

| Technology | Purpose |
|---|---|
| Amazon Web Services (AWS) | Hosting the PostgreSQL database |
| PostgreSQL | Relational database and data warehouse |
| DBeaver | Database connection, SQL development, data imports, and validation |
| SQL | Data transformation, analytical queries, joins, aggregations, and validation |
| Microsoft Power BI | Interactive dashboards and business reporting |
| Power Query | Data preparation and transformation for reporting |
| Dimensional Modelling | Organizing data into facts and dimensions |

## Database Architecture

The project uses PostgreSQL hosted on AWS as the central data warehouse. DBeaver provides the database interface for importing data, executing SQL scripts, creating database objects, and inspecting query results.

The architecture follows this workflow:

```text
Seven Source CSV Datasets
          |
          v
    PostgreSQL on AWS
          |
          v
   DBeaver / SQL
          |
          v
     Staging Tables
          |
          v
  Data Cleaning and ETL
          |
          v
 Cleaned Transformation Views
          |
          v
 Dimension and Fact Tables
          |
          v
 Data Validation and SQL Analytics
          |
          v
      Microsoft Power BI
          |
          v
 Retail Performance Dashboard
```

### AWS and DBeaver integration

- **AWS:** Hosts the PostgreSQL database used for warehouse implementation.
- **DBeaver:** Connects to PostgreSQL to manage database objects, import source data, execute SQL, and validate results.
- **PostgreSQL:** Stores staging tables, transformation views, dimensional tables, and analytical facts.
- **Power BI:** Connects to PostgreSQL through an ODBC connection with SSL enabled, as documented in the project, to provide management reporting.

This setup separates data storage, transformation, analysis, and visualization into distinct stages.

## Source Data

The warehouse integrates seven datasets representing retail transactions, master data, and inventory.

| Dataset | Source records | Description |
|---|---:|---|
| Online Sales | 8,000 | Online orders, prices, discounts, and delivery dates |
| Store Sales | 12,012 | Physical-store transactions |
| Products | 60 | Product details, categories, and unit costs |
| Customers | 600 | Customer details, segments, locations, and loyalty tiers |
| Stores | 10 | Store locations, regions, and store types |
| Daily Inventory | 54,000 | Daily product-store inventory snapshots |
| Promotions | 20 | Promotion details, dates, and discount rates |

The source data is synthetic and supplied for educational purposes.

## Data Cleaning and ETL

Data profiling identified missing values, duplicate records, inconsistent text formatting, and identifier inconsistencies.

SQL-based transformations were implemented using PostgreSQL functions and techniques such as:

- `TRIM()` to remove unnecessary whitespace.
- `UPPER()` to standardize product codes.
- `INITCAP()` to normalize category and subcategory labels.
- `ROW_NUMBER()` to identify and remove verified duplicate records.
- Date and numeric type conversions for analytical consistency.
- Unknown Customer and No Promotion handling to preserve valid transactions with incomplete attributes.
- Staging load logs to record data ingestion activity.

The ETL process preserves source data in staging and applies transformations through cleaned views before loading the analytical warehouse.

### Data quality results

| Validation measure | Result |
|---|---:|
| Source datasets | 7 |
| Accepted sales transaction lines | 20,000 |
| Accepted online sales lines | 8,000 |
| Accepted physical-store sales lines | 12,000 |
| Daily inventory records | 54,000 |
| Verified duplicate store records excluded | 12 |
| Source-to-staging row-count differences | 0 |
| Sales net revenue reconciliation difference | 0 |

The twelve duplicate physical-store records were excluded from the accepted analytical load after verification. The resulting sales facts contain 20,000 accepted lines.

## Dimensional Data Warehouse

The project uses a dimensional model to support efficient analytical queries and reporting.

### Dimension tables

| Dimension | Purpose |
|---|---|
| `dim_date` | Calendar attributes for date-based analysis |
| `dim_product` | Product names, categories, subcategories, and unit costs |
| `dim_customer` | Customer attributes, segments, locations, and loyalty information |
| `dim_store` | Store locations, regions, and store types |
| `dim_promotion` | Promotion names, dates, and discount rates |

Surrogate keys are used to connect dimension records to fact records, while business identifiers are retained for traceability.

### Fact tables

**`fact_sales`**

Stores sales transaction lines from both online and physical-store channels, including:

- Transaction identifier
- Sales channel
- Date, product, customer, store, and promotion keys
- Quantity sold
- Gross sales
- Discount amount
- Net sales

**`fact_inventory_daily`**

Stores daily inventory snapshots at the product-store-date level, including:

- Date, product, and store keys
- Opening stock
- Units received
- Units sold
- Closing stock
- Stock balance variance, where implemented

Sales and inventory are maintained at different levels of detail. Inventory balances are not summed across dates because doing so would overstate stock.

## Power BI Dashboard

The ShopSmart Retail Performance Dashboard provides a visual overview of business performance.

### Dashboard components

- **Key Performance Indicators:** Gross sales, quantity sold, discounts, and net sales.
- **Monthly Sales Trends:** Comparison of sales performance across the known months of 2025.
- **Product and Category Analysis:** Identification of high-performing products and categories.
- **Store Performance:** Comparison of revenue across physical store locations.
- **Sales Channel Analysis:** Online versus physical-store sales.
- **Customer Segmentation:** Analysis by customer segment and loyalty tier.
- **Promotion Analysis:** Sales associated with different promotions.
- **Inventory Monitoring:** Identification of zero-stock and low-stock product-store combinations.

The dashboard is intended to help management understand retail performance and investigate areas requiring operational attention.

## Key Business Results

The following figures are reported in the project documentation.

### Overall sales performance

| Metric | Result |
|---|---:|
| Gross sales | R33,359,348.59 |
| Discounts | R2,404,133.93 |
| Net sales | R30,955,214.66 |
| Quantity sold | 47,090 units |
| Physical-store net sales | R19,023,103.10 |
| Online net sales | R11,932,111.56 |
| Physical-store share of net sales | 61.45% |
| Online share of net sales | 38.55% |

### Product and category performance

- Groceries was the leading sales category, generating R7,813,701.15 in net sales.
- Household ranked second, with R6,544,984.64 in net sales.
- Maize Meal 5kg was the highest-revenue product, generating R1,380,238.85.
- Maize Meal 5kg and Paper Towels tied for the highest reported unit sales, at 1,189 units each.

### Store and channel performance

- Pietermaritzburg recorded the highest physical-store net sales, at R2,004,728.90.
- Physical stores generated more total net sales than the online channel.
- Average transaction value was R1,585.26 for physical stores and R1,491.51 for online sales.

### Monthly sales performance

- August was the strongest month in 2025, with net sales of R2,705,200.65.
- November was the weakest month, with net sales of R2,453,325.80.

These results describe the supplied synthetic dataset and should not be interpreted as evidence of recurring seasonal trends or causal relationships.

## Inventory Analysis

The inventory fact table captures daily stock movements and balances at the product-store-date level.

The documented inventory analysis uses the latest available snapshot, dated **31 March 2026**, with these project-defined categories:

- **Zero Stock:** Closing stock equals zero.
- **Low Stock:** Closing stock is between 1 and 10 units.

The SQL analysis identified seven zero-stock product-store combinations and one combination with three units remaining.

Cape Town had the highest reported count of zero-stock observations during the supplied inventory period.

The ten-unit low-stock threshold is a project assumption, not a validated product-specific replenishment policy. Inventory findings should be checked against the latest snapshot and the stock balance calculation before operational decisions are made.

## Analytical SQL

Fifteen management questions were investigated through SQL queries, joins, aggregations, rankings, and date-based analysis.

The analysis covers:

1. Annual sales, quantities, discounts, and estimated profitability.
2. Monthly sales trends.
3. Highest-performing product categories.
4. Top products by revenue.
5. Highest-performing physical stores.
6. Sales by region.
7. Online versus physical-store performance.
8. Revenue by customer segment.
9. Sales by loyalty tier.
10. Sales associated with promotions.
11. Products with the lowest closing stock.
12. Stores with the most zero-stock observations.
13. Top products by quantity sold.
14. Average transaction value by channel.
15. Strongest and weakest sales months.

The analytical results provide a foundation for product planning, inventory monitoring, customer analysis, and management reporting.

## Data Validation

The project includes validation checks to establish the consistency of the warehouse and analytical outputs.

Key checks include:

- Comparing source and staging row counts.
- Reconciling accepted source records with fact table records.
- Verifying sales transaction identifier uniqueness.
- Checking mandatory fact keys.
- Validating foreign-key relationships.
- Checking dimension business-key uniqueness.
- Comparing net sales between cleaned source data and the consolidated sales fact.
- Inspecting inventory grain and stock balance discrepancies.

The documented net sales reconciliation produced a total of R30,955,214.66 on both sides, with a difference of zero.

Some final dashboard items, including inventory snapshot consistency and monthly date filtering, were identified for further correction in the project documentation.

## Business Recommendations

Based on the documented analysis:

- Review stock availability for high-demand categories such as Groceries and Household.
- Investigate zero-stock observations and stock balance exceptions.
- Use product-level demand and stock movement to inform replenishment decisions.
- Compare online and physical-store performance using consistent revenue definitions.
- Investigate the factors contributing to monthly sales differences before making seasonal planning decisions.
- Evaluate promotions using appropriate comparison methods before attributing sales increases to promotional activity.
- Improve the handling of missing transaction dates.
- Reconcile dashboard filters and inventory measures with validated SQL results.
- Complete and validate estimated profitability measures before using them for management decisions.

## imitations

- The source data is synthetic and may not represent actual retail operations.
- Sixty-nine online sales records have missing order dates, affecting chronological analysis.
- Inventory and sales cover different periods: known-date sales cover 2025, while inventory snapshots cover January to March 2026.
- Estimated profit and margin outputs require further validation and are not confirmed as completed dashboard measures.
- Promotion-associated sales do not establish causal promotional uplift.
- Inventory thresholds are project assumptions and should be reviewed against actual replenishment policies.
- Some dashboard date-filter and inventory aggregation discrepancies were documented for correction.

## How to Run the Project

1. Clone or download the GitHub repository.
2. Ensure PostgreSQL and DBeaver are available, or configure access to the hosted AWS PostgreSQL database.
3. Configure the database connection using the appropriate host, port, database name, username, and authentication method.
4. Import the seven source CSV datasets into the staging tables.
5. Execute the SQL scripts in the correct order to create tables, transformation views, dimensions, and fact tables.
6. Run the validation queries to confirm record counts, relationships, and sales reconciliation.
7. Execute the analytical SQL queries to reproduce the business results.
8. Open the Power BI report and configure its PostgreSQL ODBC connection.
9. Refresh the report and explore the available visualizations and filters.

**Security note:** Do not commit database passwords, AWS credentials, private connection strings, or other secrets to GitHub. Use your own authorized database connection details.

The repository should include the relevant SQL scripts, Power BI file, and permitted sample data if you want other users to reproduce the results.

## Suggested Repository Structure

```text
ShopSmart-Data-Warehouse/
│
├── README.md
├── sql/
│   ├── 01_create_staging_tables.sql
│   ├── 02_data_cleaning_etl.sql
│   ├── 03_create_dimensions.sql
│   ├── 04_create_fact_tables.sql
│   ├── 05_data_validation.sql
│   └── 06_analytical_queries.sql
│
├── powerbi/
│   └── ShopSmart_Retail_Dashboard.pbix
│
├── data/
│   └── README.md
│
└── images/
    └── shopsmart-dashboard.png
```

This is a suggested structure. Update the filenames to match the actual files you publish.


## Project Summary

ShopSmart demonstrates how a dimensional data warehouse can integrate multiple retail datasets into a consistent analytical environment. By combining AWS-hosted PostgreSQL, DBeaver, SQL-based ETL, dimensional modelling, data validation, and Power BI, the project illustrates the process of converting raw retail data into meaningful business insights.

The project also highlights the importance of data quality, clearly defined fact-table grain, reconciliation of measures, and consistent reporting logic in building reliable Business Intelligence solutions.

