# Customer Behavior Analysis Using Clustering and Association Rules

An end-to-end **Data Mining & Data Warehousing** project integrating CRM and ERP data to uncover customer segments, purchasing patterns, and segment-specific product associations.

## 1. Project Overview

Organizations collect customer, product, sales, and demographic data from multiple operational systems. However, raw data alone does not explain how customers behave, which customer groups exist, or which products are frequently purchased together.

This project integrates data from two source systems, builds a structured SQL Server data warehouse, and applies clustering and association rule mining to discover meaningful customer behavior patterns.

**Core workflow:**

`CRM + ERP → Bronze → Silver → Gold → SQL Analysis → Clustering → Association Rules → Business Insights`

The project combines practical data engineering, SQL, data warehousing, and data mining in one reproducible workflow.

## 2. Project Objective

To discover meaningful customer segments and behavioral relationships from integrated customer and transaction data using clustering and association rule mining, and translate these patterns into actionable business insights.

### Research questions

**Customer behavior**

* What major purchasing patterns exist in the customer base?
* Which features help distinguish customers?

**Clustering**

* Can customers be grouped into meaningful behavioral segments?
* What characteristics define each segment?
* How do segments differ in spending, frequency, and product diversity?

**Association rules**

* Which products or categories are frequently purchased together?
* Which associations have meaningful support, confidence, and lift?
* Which patterns could inform cross-selling or product recommendations?

**Combined analysis**

* Do different customer segments exhibit different purchasing associations?
* Does combining clustering and association rules reveal deeper insights?

## 3. Dataset

The project uses a multi-source retail dataset containing six CSV files.

### CRM source

| File                | Purpose                     |
| ------------------- | --------------------------- |
| `cust_info.csv`     | Customer master information |
| `prd_info.csv`      | Product information         |
| `sales_details.csv` | Sales transactions          |

### ERP source

| File              | Purpose                         |
| ----------------- | ------------------------------- |
| `CUST_AZ12.csv`   | Additional customer information |
| `LOC_A101.csv`    | Customer location information   |
| `PX_CAT_G1V2.csv` | Product category information    |

Initial inspection indicates approximately 18K customer records, 60K sales records, 27K orders, 397 product records, and 37 product category records. These are preliminary figures and will be verified during the formal data audit.

The source files have different structures and key formats. Customer and product relationships must be validated before integration.

**Data policy:** Raw source files will be preserved. Credentials, personal information not required for the analysis, and large raw datasets should not be committed to the public repository.

## 4. Data Warehouse Architecture

The warehouse will follow a **Bronze–Silver–Gold layered architecture**.

### Bronze: Raw source layer

Purpose: preserve and understand the original source data.

Activities:

* Inspect CRM and ERP source systems.
* Document source columns and data types.
* Establish table grain and candidate keys.
* Create raw/staging table definitions.
* Load source data without silently changing its meaning.
* Document source-to-table data flow.

### Silver: Cleaned and integrated layer

Purpose: create reliable, standardized data.

Activities:

* Handle missing, invalid, and duplicate values.
* Standardize data types, dates, and identifiers.
* Resolve customer and product key relationships.
* Validate joins and referential integrity.
* Apply documented transformation rules.
* Load cleaned tables through repeatable SQL procedures or scripts.

### Gold: Business-ready analytical layer

Purpose: provide consistent data for analysis.

The final dimensional model will be based on the audited source relationships. Candidate objects include:

**Dimensions**

* `dim_customer`
* `dim_product`
* `dim_category`
* `dim_date`
* `dim_location`, if justified by the data model

**Fact**

* `fact_sales`

The grain of each fact table and the keys used by dimensions will be explicitly documented. Measures and relationships will be validated before the schema is finalized.

The Gold layer will support SQL analysis, customer feature engineering, and transaction-basket creation.

## 5. End-to-End Project Lifecycle

The project will follow these stages in order.

### Phase 1 — Problem Definition

**Step 1. Business Problem, Objectives and Research Questions**

Define the business problem, analytical goals, scope, and expected outcomes.

### Phase 2 — Dataset Selection

**Step 2. Dataset Selection and Data Requirements**

Confirm the CRM and ERP sources, required fields, table grain, and suitability for both clustering and association rule mining.

### Phase 3 — Data Understanding

**Step 3. Data Understanding and Data Quality Audit**

Profile all six source files. Inspect schemas, data types, missing values, duplicates, candidate keys, date validity, and relationships.

### Phase 4 — Data Preparation

**Step 4. Data Cleaning, Transformation and Integration**

Implement the required transformations, standardize keys, validate source relationships, and prepare the Silver layer.

### Phase 5 — Data Warehouse

**Step 5. Data Warehouse Design**

Define the Bronze, Silver, and Gold layers, data flow, naming conventions, table grain, and dimensional modeling approach.

**Step 6. Implement the Warehouse in SQL Server**

Create the database, schemas, tables, loading scripts, and repeatable ETL procedures as appropriate.

**Step 7. Validate the Warehouse**

Check row counts, data types, key uniqueness, referential integrity, transformation results, and reconciliation against source data.

### Phase 6 — Analytical SQL

**Step 8. OLAP and SQL-Based Analysis**

Use SQL queries and aggregations to explore sales, customer activity, product performance, and purchasing behavior.

### Phase 7 — Data Mining

**Step 9. Customer Feature Engineering**

Build a customer-level analytical table using relevant behavioral variables.

**Step 10. Exploratory Data Analysis**

Study distributions, outliers, spending, purchase frequency, recency, and product diversity.

**Step 11. Customer Clustering**

Select and evaluate an appropriate clustering approach. Determine the number of clusters using analytical evidence.

**Step 12. Cluster Profiling and Interpretation**

Compare cluster characteristics and give each segment a label grounded in its observed behavior.

**Step 13. Association Rule Mining**

Transform transactions into baskets and mine product or category associations.

**Step 14. Association Rule Evaluation and Interpretation**

Evaluate rules using support, confidence, lift, and business relevance.

**Step 15. Segment-Specific Association Analysis**

Compare purchasing associations across customer segments, subject to sufficient transaction coverage in each segment.

### Phase 8 — Insights and Delivery

**Step 16. Business Insights and Recommendations**

Translate the results into evidence-based recommendations for segmentation, cross-selling, product discovery, and targeted engagement.

**Step 17. Dashboard Development**

Build a Power BI dashboard or another suitable visualization layer for the analytical results.

**Step 18. Final Report**

Document methodology, data quality, warehouse design, analytical decisions, results, limitations, and recommendations.

**Step 19. GitHub Documentation and Portfolio Presentation**

Finalize the README, data dictionary, architecture diagram, SQL scripts, notebooks, visuals, and reproducibility instructions.

**Step 20. Final Presentation and Viva**

Present the problem, data pipeline, warehouse, mining methods, findings, and limitations.

## 6. Data Mining Methodology

### Customer clustering

Potential features include:

* Recency: time since the last recorded purchase
* Frequency: number of distinct orders
* Monetary value: total sales value
* Average order value
* Total quantity purchased
* Product and category diversity
* Customer activity period

Feature definitions, observation dates, missing-value treatment, scaling, and outlier handling will be documented. The final feature set and algorithm will be chosen after data exploration.

Evaluation may include the elbow method, silhouette score, cluster stability where practical, and business interpretability.

### Association rule mining

Transactions will be converted into baskets containing unique products or categories.

Rules will be evaluated using:

* **Support:** how frequently an itemset occurs in the relevant transactions.
* **Confidence:** how frequently the consequent occurs when the antecedent occurs.
* **Lift:** how much more frequently the antecedent and consequent occur together than expected under independence.

Apriori or FP-Growth may be used depending on the data and computational requirements.

### Segment-specific analysis

Customer segments will be linked to their transactions. Association rules will then be compared across segments, using consistent definitions and suitable minimum support thresholds.

A segment's rules will be interpreted cautiously when its transaction count is small. Differences in rule metrics will not automatically be treated as proof of causation.

## 7. Technology Stack

| Area                       | Tools                               |
| -------------------------- | ----------------------------------- |
| Data processing            | Python, Pandas, NumPy               |
| Data warehouse             | Microsoft SQL Server                |
| SQL development            | SQL Server Management Studio (SSMS) |
| Mining                     | Scikit-learn, MLxtend               |
| Analysis                   | Jupyter Notebook                    |
| Visualization              | Matplotlib, Seaborn, Plotly         |
| Dashboard                  | Power BI                            |
| Architecture diagrams      | Draw.io                             |
| Planning and documentation | Notion, Markdown                    |
| Version control            | Git, GitHub                         |

The exact tool choices may be adjusted to fit the available environment without changing the project lifecycle.

## 8. Planned Repository Structure

```text
customer-behavior-analysis/
├── datasets/
│   ├── source_crm/
│   └── source_erp/
├── docs/
│   ├── architecture/
│   ├── data_dictionary.md
│   ├── data_flow.md
│   └── data_quality_report.md
├── sql/
│   ├── bronze/
│   ├── silver/
│   ├── gold/
│   └── analysis/
├── scripts/
│   ├── load_bronze/
│   ├── transform_silver/
│   └── validate/
├── notebooks/
│   ├── 01_data_audit.ipynb
│   ├── 02_eda.ipynb
│   ├── 03_customer_feature_engineering.ipynb
│   ├── 04_customer_clustering.ipynb
│   ├── 05_association_rules.ipynb
│   └── 06_segment_association_analysis.ipynb
├── src/
│   ├── preprocessing/
│   ├── clustering/
│   └── association_rules/
├── dashboard/
├── reports/
├── visuals/
├── requirements.txt
├── .gitignore
└── README.md
```

This is the planned structure; directories and files will be created when their corresponding project stages begin.

## 9. Naming and Documentation Conventions

* Use consistent, descriptive, lowercase `snake_case` names for Python variables, functions, and analytical columns.
* Use documented schema prefixes or directories to distinguish Bronze, Silver, and Gold objects.
* Use explicit, consistent names for primary keys, foreign keys, and measures.
* Document transformations and key mappings rather than relying on undocumented assumptions.
* Keep SQL scripts, notebooks, and data-flow documentation aligned with the actual implementation.

## 10. Expected Deliverables

* Audited and documented CRM/ERP sources
* Repeatable ETL and validation scripts
* SQL Server data warehouse
* Documented dimensional model and architecture diagram
* Customer-level behavioral feature table
* Evaluated customer clusters and segment profiles
* Association rules with support, confidence, and lift
* Segment-specific purchasing analysis
* Business recommendations and visualizations
* Dashboard, final report, and presentation

## 11. Portfolio Relevance

This project demonstrates an end-to-end workflow:

**Raw data → data quality → ETL → data integration → SQL data warehouse → analytical SQL → customer segmentation → association discovery → business recommendations.**

It is intended to demonstrate practical skills relevant to Data Analyst, Business Analyst, Business Intelligence Analyst, Junior Data Scientist, and entry-level Data Engineering roles.

## 12. Project Status

* [x] Step 1: Business problem and objectives
* [x] Step 2: Dataset selection and requirements
* [ ] Step 3: Data understanding and quality audit
* [ ] Step 4: Cleaning, transformation and integration
* [ ] Step 5: Warehouse design
* [ ] Step 6: SQL Server implementation
* [ ] Step 7: Warehouse validation
* [ ] Step 8: SQL/OLAP analysis
* [ ] Step 9: Customer feature engineering
* [ ] Step 10: Exploratory data analysis
* [ ] Step 11: Customer clustering
* [ ] Step 12: Cluster profiling
* [ ] Step 13: Association rule mining
* [ ] Step 14: Rule evaluation
* [ ] Step 15: Segment-specific association analysis
* [ ] Step 16: Business insights
* [ ] Step 17: Dashboard
* [ ] Step 18: Final report
* [ ] Step 19: GitHub documentation
* [ ] Step 20: Final presentation

## Project Principle

**Build the warehouse systematically, validate the data before mining it, and turn discovered patterns into interpretable business insights.**

The project will follow the lifecycle above in order. The Bronze–Silver–Gold architecture supports that lifecycle; it does not replace or reorder it.
