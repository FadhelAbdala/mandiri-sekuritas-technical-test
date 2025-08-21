# Transaction Analytics Dashboard

This project provides an interactive **Transaction Analytics Dashboard** built with **Looker Studio** using **BigQuery** as the data source.  
It enables stakeholders to monitor key metrics, identify trends, and filter results by various customer, merchant, and transaction dimensions.

---

## 📊 Features

### **1. KPIs**
- **Transaction Count** – total number of transactions in the selected date range
- **Total Value** – sum of transaction amounts
- **Average Ticket Value** – total value ÷ transaction count
- **Active Users** – distinct customers who transacted in the period

### **2. Trend Analysis**
- **Daily / Monthly Trends** for transaction count and value
- **Channel Usage** (Chip vs. Swipe)
- **Merchant Category (MCC) Performance**
- **Customer Segmentation** (by Age Band, Income Band, Credit Band)
- **Risk Indicators**:
  - Refunds
  - Chip-capable but swiped
  - Card on dark web

### **3. Filtering & Drilldowns**
Global filters include:
- Date range
- Channel
- MCC
- Merchant State / City
- Age, Income, Credit Bands
- Risk Flags

---

## 🗂 Data Model

The dashboard is powered by a **single enriched fact table** in BigQuery:

| Field Category      | Examples |
|---------------------|----------|
| Transaction Time    | `trade_date`, `month_start`, `hh` |
| Transaction Info    | `txn_id`, `amount`, `is_refund` |
| Customer Info       | `client_id`, `age_band`, `income_band`, `credit_band` |
| Card Info           | `card_id`, `card_brand`, `has_chip`, `chip_capable_but_swiped` |
| Merchant Info       | `merchant_id`, `mcc`, `merchant_state`, `merchant_city` |
| Risk Flags          | `is_refund`, `card_on_dark_web`, `chip_capable_but_swiped` |

---

## 🛠 Tech Stack
- **BigQuery** – SQL-based data transformation & enrichment
- **Looker Studio** – Visualization & dashboarding
- **Google Cloud Platform (GCP)** – Data hosting & access control

---

## 🚀 Getting Started

### 1️⃣ Prerequisites
- Access to Google BigQuery
- Access to the `fact_txn_enriched` dataset/view (ask admin for permissions)
- Looker Studio account

### 2️⃣ Connect to BigQuery
1. In Looker Studio, create a new data source
2. Select **BigQuery** connector
3. Choose your `project_id.dataset_id.table_id`
4. Set credentials to **Owner's credentials** or share dataset with viewers

### 3️⃣ Set Up the Dashboard
- Import the provided Looker Studio template or recreate the charts using the SQL and field definitions in this repo
- Add global filters for key dimensions

---

## 📈 Example Queries

**Monthly KPIs**
```sql
SELECT
  FORMAT_DATE('%Y-%m', month_start) AS month,
  COUNT(*) AS txn_count,
  SUM(amount) AS total_value,
  SAFE_DIVIDE(SUM(amount), COUNT(*)) AS avg_ticket,
  COUNT(DISTINCT client_id) AS active_users
FROM `project.dataset.fact_txn_enriched`
GROUP BY month
ORDER BY month;
