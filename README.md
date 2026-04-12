# Credit Card Fraud Detection Pipeline (Azure)

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





