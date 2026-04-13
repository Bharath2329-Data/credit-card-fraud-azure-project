# Credit Card Fraud Detection Pipeline (Azure)
![Fraud Pipeline](docs/images/fraud_thumbnail.png)

## Project Overview
This project builds an end-to-end data engineering pipeline to detect suspicious credit card transactions using Azure services.

## Architecture
Source → ADF → ADLS (Bronze) → Databricks (Silver/Gold) → SQL → Power BI

## Tools Used
- Azure Data Factory
- Azure Data Lake Storage Gen2
- Azure Databricks
- SQL
- Power BI

## Goal
Identify fraudulent transactions using rule-based detection and provide business insights.

## Project Timeline
- Day 1: Setup
- Day 2: Dataset
- Day 3: Ingestion
- Day 4: Cleaning
- Day 5: Fraud Logic
- Day 6: Gold Layer
- Day 7: SQL
- Day 8: Power BI
- Day 9: Orchestration
- Day 10: Final Polish

# Day 1 — Azure Project Setup & Architecture

## Objective

The goal of Day 1 is to set up the complete Azure environment and project structure required to build an end-to-end Credit Card Fraud Detection Data Pipeline.

## Architecture Overview

The project follows a modern data engineering architecture using Azure services:

**Source → Azure Data Factory → ADLS Gen2 (Bronze/Silver/Gold) → Azure Databricks → SQL Analytics → Power BI**


## Azure Services Created

### 1. Resource Group

* **Name:** rg-fraud-project
* Purpose: Logical container to manage all Azure resources together

---

### 2. Azure Data Lake Storage Gen2

* **Storage Account:** stfraud<yourname>
* **Feature Enabled:** Hierarchical Namespace (ADLS Gen2)

#### Containers Created:

* **bronze** → Raw data storage
* **silver** → Cleaned and transformed data
* **gold** → Business-ready aggregated data


### 3. Azure Data Factory

* **Name:** adf-fraud-project
* Purpose: Data ingestion and pipeline orchestration

### 4. Azure Databricks

* **Workspace:** dbx-fraud-project
* Purpose: Data transformation, fraud detection logic, and analytics

## 📁 Project Structure (GitHub)

```bash
credit-card-fraud-azure-project/
│
├── data/
│   ├── raw/
│   └── sample/
├── notebooks/
├── pipelines/
├── sql/
├── dashboard/
├── docs/
└── README.md
```


## Artifacts

Screenshots captured for documentation:

* Azure Resource Group
* Storage Account
* ADLS Containers (bronze, silver, gold)
* Azure Data Factory
* Azure Databricks Workspace

Stored in:

docs/screenshots/

## Key Concepts Learned

* Understanding of Azure resource organization
* Importance of ADLS Gen2 for data lake architecture
* Role of Data Factory in data ingestion
* Role of Databricks in data transformation
* Medallion architecture (Bronze → Silver → Gold)

## Cost Optimization Strategy

To ensure the project stays within budget:

* No Databricks clusters were created or run on Day 1
* No pipelines were executed
* Only essential resources were provisioned
* All services configured using basic/standard tiers

## Key Outcome

Successfully established the foundational infrastructure required to build a scalable and production-like fraud detection data pipeline on Azure.

# Day 2 — Dataset Creation for Fraud Detection

## Objective

The goal of Day 2 is to design and create realistic banking datasets that simulate credit card transactions, customers, and merchants. These datasets form the foundation for building a fraud detection pipeline.

---

## Datasets Created

Three core datasets were created:

### 1. transactions.csv (Core Dataset)

Contains transaction-level data used to detect fraudulent activity.

**Columns:**

* transaction_id
* customer_id
* merchant_id
* transaction_timestamp
* amount
* transaction_type
* merchant_category
* city
* state
* channel (POS / Online)
* card_type (Credit / Debit)

### 2. customers.csv

Contains customer profile information.

**Columns:**

* customer_id
* full_name
* age
* gender
* home_city
* home_state
* account_open_date
* customer_segment (Premium / Standard)

---

### 3. merchants.csv

Contains merchant details and risk classification.

**Columns:**

* merchant_id
* merchant_name
* merchant_category
* merchant_city
* merchant_state
* risk_level (Low / Medium / High)

## Fraud Simulation Logic

To make the dataset realistic and suitable for fraud detection, multiple fraud patterns were intentionally introduced:

### High-Value Transactions

Transactions with unusually high amounts (e.g., > $2000)

### Location Mismatch

Customer home state differs from transaction state

### Rapid Transactions

Multiple transactions within a short time window for the same customer

### High-Risk Merchant Categories

Transactions involving:

* Electronics
* Luxury
* Travel
* Online platforms

### Unusual Time Activity

Transactions occurring during late-night hours

---

## Dataset Size

| Dataset          | Records |
| ---------------- | ------- |
| customers.csv    | ~100    |
| merchants.csv    | ~30     |
| transactions.csv | ~1000   |

---

## Design Considerations

* Simulated real-world banking behavior
* Included both normal and suspicious transactions
* Balanced dataset size for performance and cost efficiency
* Designed to support downstream transformations and analytics

---

## Project Structure Update

```bash
data/
└── raw/
    ├── customers.csv
    ├── merchants.csv
    └── transactions.csv
```

---

## Artifacts

* Dataset screenshots stored in: `docs/screenshots/`
* Data dictionary created in: `docs/data_dictionary.md`

## Key Outcome

A realistic, fraud-oriented dataset is now ready for ingestion into Azure Data Lake (Bronze layer) using Azure Data Factory in the next phase.

# 📅 Day 3 — Data Ingestion using Azure Data Factory (Bronze Layer)

## 🎯 Objective

The goal of Day 3 is to ingest raw CSV datasets into Azure Data Lake Storage Gen2 (Bronze layer) using Azure Data Factory pipelines.

---

## 🧠 Overview

In this phase, raw datasets created on Day 2 are moved into the cloud storage layer without any transformations. This ensures data is preserved in its original format for traceability and reprocessing.

---

## ⚙️ Tools Used

* Azure Data Factory
* Azure Data Lake Storage Gen2 (ADLS Gen2)

---

## 📊 Data Sources

The following datasets were ingested:

* `customers.csv`
* `merchants.csv`
* `transactions.csv`

---

## 🏗️ Implementation Steps

### 1. Data Upload to ADLS (Bronze)

* Created directories in Bronze container:

  * `customers/`
  * `merchants/`
  * `transactions/`
* Uploaded CSV files into respective folders

---

### 2. Linked Service Creation

* Created connection to ADLS Gen2:

  * Name: `ls_adls_fraud`
* Used Account Key authentication

---

### 3. Dataset Creation

Created source and sink datasets for each file:

| Dataset Type | Dataset Name         |
| ------------ | -------------------- |
| Source       | ds_customers_src     |
| Sink         | ds_customers_sink    |
| Source       | ds_merchants_src     |
| Sink         | ds_merchants_sink    |
| Source       | ds_transactions_src  |
| Sink         | ds_transactions_sink |

---

### 4. Pipeline Development

Created pipelines using Copy Activity:

* `pl_ingest_customers`
* `pl_ingest_merchants`
* `pl_ingest_transactions`

Each pipeline:

* Reads data from Bronze folder
* Writes a copy within Bronze (validation step)

---

### 5. Pipeline Execution

* Pipelines executed using Debug mode
* Verified successful execution in ADF Monitor

---

## Data Flow

Local Files → ADLS Bronze → ADF Copy → Bronze (validated copy)

---

## Output Structure

```bash
bronze/
 ├── customers/customers.csv
 ├── customers/customers_copy.csv
 ├── merchants/merchants.csv
 ├── merchants/merchants_copy.csv
 ├── transactions/transactions.csv
 └── transactions/transactions_copy.csv
```

---

## Artifacts

Screenshots captured:

* ADF pipeline canvas
* Copy activity configuration
* Pipeline execution results
* Bronze storage output

Stored in:

```
docs/screenshots/
```

## Key Learnings

* Understanding of Azure Data Factory components:

  * Linked Services
  * Datasets
  * Pipelines
  * Activities
* Data ingestion patterns in cloud environments
* Importance of Bronze layer in Medallion architecture

## Challenges & Fixes

* Schema import issues resolved by setting "Import Schema" to "From connection" or "None"
* File path errors corrected by verifying directory structure
* Ensured header row was correctly detected

---

## Outcome

Successfully built ingestion pipelines to move raw data into the Bronze layer, establishing the foundation for downstream transformations.

# Day 4 — Data Transformation using Azure Databricks (Silver Layer)

## Objective

The goal of Day 4 is to clean, standardize, and transform raw data from the Bronze layer into structured and optimized datasets in the Silver layer using Azure Databricks.

---

## Overview

Raw data ingested in Day 3 is processed to improve data quality and consistency. This step ensures that datasets are ready for business logic and analytics.

---

## Tools Used

* Azure Databricks
* Apache Spark (PySpark)
* Azure Data Lake Storage Gen2

---

## Input Data (Bronze Layer)

* `customers_copy.csv`
* `merchants_copy.csv`
* `transactions_copy.csv`

---

## Transformation Steps

### 1. Data Ingestion

* Read CSV files from Bronze container using Spark

---

### 2. Data Cleaning

#### ✔ Remove Duplicates

* Eliminated duplicate records from all datasets

#### ✔ Trim Whitespace

* Removed leading and trailing spaces from string columns

---

### 3. Data Type Standardization

* Converted:

  * `amount` → Double
  * `transaction_timestamp` → Timestamp
  * `age` → Integer

---

### 4. Null Handling

* Replaced null values in key fields (e.g., amount = 0)

---

### 5. Data Standardization

* Converted state values to uppercase
* Ensured consistency across datasets

---

### 6. Data Validation

* Checked row counts before and after transformation
* Verified schema and data types

---

## Output Data (Silver Layer)

Cleaned datasets stored in Parquet format:

```bash
silver/
 ├── customers/
 ├── merchants/
 └── transactions/
```

---

## Why Parquet Format?

* Columnar storage → faster queries
* Compression → reduced storage cost
* Optimized for analytics workloads

---

## Artifacts

Captured:

* Databricks notebook transformations
* Data preview outputs
* Silver layer storage structure

 Stored in:

```
docs/screenshots/
```

---

## Key Learnings

* Data cleaning techniques in PySpark
* Importance of schema enforcement
* Benefits of columnar storage (Parquet)
* Role of Silver layer in Medallion architecture

---

## Challenges & Fixes

* Timestamp conversion issues fixed using proper casting
* Storage access resolved using account key configuration
* Path errors corrected by verifying `abfss://` format

---

##  Outcome

Successfully transformed raw Bronze data into clean, structured Silver datasets ready for fraud detection and analytics.

# Day 5 — Fraud Detection Logic & Risk Scoring (Gold Layer)

## Objective

The goal of Day 5 is to implement fraud detection logic using cleaned datasets from the Silver layer and generate a fraud-enriched dataset for analytics in the Gold layer.

---

## Overview

In this phase, transaction, customer, and merchant data are combined to identify suspicious activity using rule-based logic. A risk scoring system is applied to classify transactions into different fraud risk levels.

---

## Tools Used

* Azure Databricks
* Apache Spark (PySpark)
* Azure Data Lake Storage Gen2

---

## Input Data (Silver Layer)

* `silver/customers/`
* `silver/merchants/`
* `silver/transactions/`

---

## Implementation Steps

### 1. Data Ingestion

* Read cleaned datasets from Silver layer using Spark

---

### 2. Data Integration

* Joined:

  * Transactions
  * Customers
  * Merchants

* Enriched transaction data with:

  * Customer demographics
  * Merchant risk levels

---

### 3. Fraud Rule Engineering

The following fraud indicators were created:

#### High Transaction Amount

* Flag transactions with amount > $2000

#### Location Mismatch

* Customer home state differs from transaction state

#### High-Risk Merchant

* Merchant classified as "High" risk

#### Night-Time Activity

* Transactions between 12 AM and 5 AM

#### Rapid Repeat Transactions

* Multiple transactions by same customer within the same hour

---

## Risk Scoring Model

Each rule contributes to a weighted risk score:

| Rule               | Score |
| ------------------ | ----- |
| High Amount        | 40    |
| Location Mismatch  | 25    |
| High-Risk Merchant | 15    |
| Night Transaction  | 10    |
| Rapid Repeat       | 20    |

---

### Fraud Classification

* **High Risk** → Score ≥ 50
* **Medium Risk** → Score 25–49
* **Low Risk** → Score < 25

---

## Output Columns Created

* `is_high_amount`
* `is_location_mismatch`
* `is_high_risk_merchant`
* `is_night_transaction`
* `is_rapid_repeat`
* `risk_score`
* `fraud_flag` (Yes/No)
* `risk_level_category` (Low/Medium/High)
* `fraud_reason` (human-readable explanation)

---

## Output Data (Gold Layer)

Fraud-enriched datasets stored in:

```bash
gold/
 ├── suspicious_transactions/
 └── suspicious_transactions_only/
```

---

## Sample Use Cases

* Identify high-risk transactions
* Monitor fraud trends
* Analyze risky customers and merchants
* Support fraud investigation workflows

---

## Artifacts

Captured:

* Databricks notebook logic
* Fraud flag outputs
* Sample suspicious transactions
* Gold layer storage verification

 Stored in:

```
docs/screenshots/
```

---

## Key Learnings

* Rule-based fraud detection design
* Data enrichment through joins
* Window functions for behavioral patterns
* Risk scoring methodology
* Building analytics-ready datasets

---

## Challenges & Fixes

* Ensured correct timestamp format for time-based rules
* Verified merchant risk values for consistency
* Handled null values before applying rules
* Simplified rapid transaction logic using hourly grouping

---

## Outcome

Successfully developed a fraud detection framework that identifies suspicious transactions using rule-based scoring and prepares data for business analytics and reporting.

---







