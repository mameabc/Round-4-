# 🛒 Retail & E-Commerce Data Pipeline (Production-Level)

> **Graduation Project — Digital Egypt Pioneers Initiative (DEPI) · Batch 4 · Data Engineering Track**  
> Ministry of Communications & Information Technology · Egypt · 2025

---

![Python](https://img.shields.io/badge/Python-3.11-3776AB?style=flat-square&logo=python&logoColor=white)
![Snowflake](https://img.shields.io/badge/Snowflake-DWH-29B5E8?style=flat-square&logo=snowflake&logoColor=white)
![Power BI](https://img.shields.io/badge/Power%20BI-Dashboard-F2C811?style=flat-square&logo=powerbi&logoColor=black)
![Pandas](https://img.shields.io/badge/Pandas-ETL-150458?style=flat-square&logo=pandas&logoColor=white)
![GitHub Actions](https://img.shields.io/badge/GitHub%20Actions-CI%2FCD-2088FF?style=flat-square&logo=githubactions&logoColor=white)
![License](https://img.shields.io/badge/License-MIT-green?style=flat-square)

---

## 📌 Project Description

A **production-grade, end-to-end Data Engineering pipeline** built on the [Olist Brazilian E-Commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle).

The pipeline covers the complete data journey:

```
Kaggle API → ETL (Python/Pandas) → Snowflake (Bronze → Silver → Gold) → Power BI Dashboard
                                        ↑
                          Automated by Snowflake Tasks (CRON schedule)
```

The project demonstrates real-world data engineering practices including **Medallion Architecture**, **automated orchestration**, **Star Schema design**, and **business intelligence** — all version-controlled and documented on GitHub.

---

## 🏗️ Architecture

```
┌─────────────────────────────────────────────────────────────────────────────┐
│                    Retail & E-Commerce Data Pipeline                        │
│                      (Production-Level Architecture)                        │
└─────────────────────────────────────────────────────────────────────────────┘

  [Kaggle API]                                                    [Power BI]
  9 CSV Files       Python + Pandas ETL        Snowflake DWH      Dashboard
  126 MB total  ──►  extract.py             ──►  Bronze Layer  ──►  3 Pages
                     transform.py          ──►  Silver Layer      15+ Visuals
                     16 Parquet files      ──►  Gold Layer
                                                (Star Schema)
                                                    │
                                          ┌─────────▼──────────┐
                                          │  Snowflake Tasks   │
                                          │  (CRON: 06:00 UTC) │
                                          │  ingest → clean    │
                                          │  → build → refresh │
                                          └────────────────────┘
                                                    │
                                          ┌─────────▼──────────┐
                                          │  GitHub Actions    │
                                          │  CI/CD on push     │
                                          │  lint → test       │
                                          │  → validate → docs │
                                          └────────────────────┘
```

---

## 🛠️ Technology Stack

| Layer | Tool / Technology | Purpose |
|-------|-------------------|---------|
| **Data Source** | Kaggle API | Download raw Brazilian E-Commerce dataset |
| **ETL** | Python 3.11 + Pandas + Parquet | Extract, clean & transform raw data |
| **Warehousing** | Snowflake Cloud DWH | Bronze / Silver / Gold Medallion layers |
| **Orchestration** | Snowflake Tasks (CRON) | Automated pipeline scheduling |
| **BI & Analytics** | Power BI Desktop | Interactive dashboards & KPI visualization |
| **CI/CD** | GitHub Actions | Automated linting, testing & validation |
| **Version Control** | Git + GitHub | Source code & documentation management |

---

## 📁 Repository Structure

```
retail-ecommerce-pipeline/
│
├── data/
│   ├── raw/                        ← Original Kaggle CSV files (9 files, 126 MB)
│   └── processed/                  ← Cleaned Parquet files (16 files output)
│
├── etl/
│   ├── extract.py                  ← Kaggle API download & validation
│   ├── transform.py                ← Pandas cleaning pipeline
│   └── profiling.py                ← Data quality profiling report
│
├── snowflake/
│   ├── bronze.sql                  ← Raw ingestion DDL (COPY INTO)
│   ├── silver.sql                  ← Cleaning & standardization SQL
│   ├── gold.sql                    ← Star schema DDL (fact + dim tables)
│   ├── views.sql                   ← KPI-ready analytical views
│   └── tasks.sql                   ← Snowflake Tasks orchestration DAG
│
├── powerbi/
│   └── retail_dashboard.pbix       ← Power BI dashboard file
│
├── docs/
│   ├── architecture.png            ← Pipeline architecture diagram
│   ├── dashboard_screenshots/      ← Power BI page screenshots
│   └── data_dictionary.md          ← Column definitions & schema docs
│
├── .github/
│   └── workflows/
│       └── ci.yml                  ← GitHub Actions CI/CD workflow
│
├── requirements.txt                ← Python dependencies
├── .env.example                    ← Environment variable template
└── README.md                       ← This file
```

---

## ⚙️ Setup & Installation

### Prerequisites

- Python 3.11+
- Kaggle account & API key
- Snowflake account (free trial works)
- Power BI Desktop
- Git

### 1. Clone the Repository

```bash
git clone https://github.com/your-org/retail-ecommerce-pipeline.git
cd retail-ecommerce-pipeline
```

### 2. Install Python Dependencies

```bash
pip install -r requirements.txt
```

**requirements.txt includes:**
```
pandas==2.1.0
kaggle==1.6.17
pyarrow==14.0.0
snowflake-connector-python==3.6.0
python-dotenv==1.0.0
pytest==7.4.0
flake8==6.1.0
```

### 3. Configure Environment Variables

```bash
cp .env.example .env
```

Edit `.env` with your credentials:

```env
# Kaggle
KAGGLE_USERNAME=your_kaggle_username
KAGGLE_KEY=your_kaggle_api_key

# Snowflake
SNOWFLAKE_ACCOUNT=your_account
SNOWFLAKE_USER=your_user
SNOWFLAKE_PASSWORD=your_password
SNOWFLAKE_WAREHOUSE=COMPUTE_WH
SNOWFLAKE_DATABASE=RETAIL_DWH
SNOWFLAKE_ROLE=SYSADMIN
```

### 4. Run the ETL Pipeline

```bash
# Step 1: Extract raw data from Kaggle
python etl/extract.py

# Step 2: Clean & transform to Parquet
python etl/transform.py

# Step 3: Load Bronze layer into Snowflake
snowsql -f snowflake/bronze.sql

# Step 4: Run Silver transformation
snowsql -f snowflake/silver.sql

# Step 5: Build Gold Star Schema
snowsql -f snowflake/gold.sql

# Step 6: Create KPI views
snowsql -f snowflake/views.sql

# Step 7: Set up Snowflake Tasks (automated scheduling)
snowsql -f snowflake/tasks.sql
```

### 5. Open Power BI Dashboard

1. Open `powerbi/retail_dashboard.pbix` in Power BI Desktop
2. Update the Snowflake connection to your account
3. Refresh all data sources
4. Publish to Power BI Service (optional)

---

## 🗄️ Data Warehouse Design

### Medallion Architecture

```
Bronze Layer (Raw)          Silver Layer (Clean)         Gold Layer (Star Schema)
─────────────────          ──────────────────────        ────────────────────────
olist_orders_raw      ──►  orders_clean           ──►   fact_orders
olist_order_items_raw ──►  order_items_clean      ──►   fact_order_items
olist_customers_raw   ──►  customers_clean        ──►   dim_customers
olist_products_raw    ──►  products_clean         ──►   dim_products
olist_payments_raw    ──►  payments_clean         ──►   dim_sellers
olist_reviews_raw         geolocation_clean             dim_date
olist_sellers_raw                                        dim_payments
olist_geolocation_raw
```

### Star Schema (Gold Layer)

```
                    ┌──────────────┐
                    │ dim_date     │
                    │─────────────│
                    │ date_id (PK) │
                    │ year, month  │
                    │ quarter, day │
                    └──────┬───────┘
                           │
┌──────────────┐    ┌──────▼────────────┐    ┌──────────────────┐
│ dim_customers│    │  fact_orders       │    │  dim_products    │
│─────────────│    │───────────────────│    │─────────────────│
│customer_id  │◄───│ order_id (PK)      │───►│ product_id (PK) │
│customer_city│    │ customer_id (FK)   │    │ category_name   │
│customer_state│   │ product_id (FK)   │    │ weight_g        │
│zip_code     │    │ seller_id (FK)    │    │ dimensions      │
└─────────────┘    │ date_id (FK)      │    └──────────────────┘
                    │ payment_id (FK)   │
                    │ price             │    ┌──────────────────┐
┌──────────────┐    │ freight_value     │    │  dim_sellers     │
│ dim_payments │    │ delivery_days     │    │─────────────────│
│─────────────│    │ delivery_status   │───►│ seller_id (PK)  │
│payment_id   │◄───│ review_score      │    │ seller_city     │
│payment_type │    └───────────────────┘    │ seller_state    │
│installments │                             └──────────────────┘
│total_value  │
└─────────────┘
```

---

## 🔄 Orchestration — Snowflake Tasks

The full pipeline runs automatically via **Snowflake Tasks** on a daily CRON schedule:

```sql
-- Root Task: Ingest Bronze
CREATE OR REPLACE TASK TASK_INGEST_BRONZE
  WAREHOUSE = COMPUTE_WH
  SCHEDULE  = 'USING CRON 0 6 * * * UTC'
AS CALL SP_INGEST_BRONZE();

-- Task 2: Build Silver (runs after Bronze)
CREATE OR REPLACE TASK TASK_BUILD_SILVER
  WAREHOUSE = COMPUTE_WH
  AFTER TASK_INGEST_BRONZE
AS CALL SP_BUILD_SILVER();

-- Task 3: Build Gold (runs after Silver)
CREATE OR REPLACE TASK TASK_BUILD_GOLD
  WAREHOUSE = COMPUTE_WH
  AFTER TASK_BUILD_SILVER
AS CALL SP_BUILD_GOLD();

-- Task 4: Refresh Analytics Views (runs after Gold)
CREATE OR REPLACE TASK TASK_REFRESH_VIEWS
  WAREHOUSE = COMPUTE_WH
  AFTER TASK_BUILD_GOLD
AS CALL SP_REFRESH_ANALYTICS_VIEWS();

-- Activate all tasks
ALTER TASK TASK_INGEST_BRONZE  RESUME;
ALTER TASK TASK_BUILD_SILVER   RESUME;
ALTER TASK TASK_BUILD_GOLD     RESUME;
ALTER TASK TASK_REFRESH_VIEWS  RESUME;
```

**Task DAG Flow:**
```
TASK_INGEST_BRONZE → TASK_BUILD_SILVER → TASK_BUILD_GOLD → TASK_REFRESH_VIEWS
     06:00 UTC            ~06:10               ~06:25              ~06:40
```

---

## 📊 Key KPIs & Results

| KPI | Value |
|-----|-------|
| 💰 Total Revenue | R$ 15.8 Million |
| 📦 Total Orders Processed | 112,647 orders |
| 🛒 Average Order Value (AOV) | R$ 140 |
| 🚚 Average Delivery Time | 12.01 days |
| ✅ On-Time Delivery Rate | 90.09% |
| 📍 Cities Covered | 4,119 Brazilian cities |
| 🏆 Top Category | Health & Beauty (R$ 1.65M) |

---

## 📈 Power BI Dashboard

The dashboard consists of **3 analytical pages**:

| Page | Content |
|------|---------|
| **Page 1: Sales Overview** | Total sales, orders over time, revenue by category, freight cost analysis |
| **Page 2: Order & Delivery Analysis** | Delivery time by state, on-time rate, funnel: placed → approved → shipped → delivered |
| **Page 3: Customer & Payment Insights** | Top customers (treemap), payment type distribution, RFM-based segmentation |

> 📸 See `/docs/dashboard_screenshots/` for full page previews.

---

## 🧪 Data Quality Checks

The ETL pipeline enforces the following quality rules:

| Check | Rule |
|-------|------|
| **Null Handling** | Delivery timestamps kept NULL for undelivered orders (logical NULL, not error) |
| **Date Standardization** | All timestamps converted from `object` → `datetime64` format |
| **Deduplication** | Geolocation file reduced by 95%+ (one record per zip code) |
| **Referential Integrity** | Ghost orders (no associated items) removed before load |
| **Payment Aggregation** | Multi-row payments per order collapsed to single row (sum) |
| **Category Translation** | Portuguese product names translated to English via lookup table |

---

## 🔁 CI/CD — GitHub Actions

Every push to `main` triggers the following automated workflow:

```yaml
# .github/workflows/ci.yml
name: Data Pipeline CI

on:
  push:
    branches: [ main ]
  pull_request:
    branches: [ main ]

jobs:
  validate:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3

      - name: Set up Python
        uses: actions/setup-python@v4
        with:
          python-version: '3.11'

      - name: Install dependencies
        run: pip install -r requirements.txt

      - name: Lint ETL scripts (flake8)
        run: flake8 etl/ --max-line-length=120

      - name: Run data validation tests
        run: pytest tests/ -v

      - name: Validate Snowflake SQL syntax
        run: python scripts/validate_sql.py snowflake/
```

---

## 👥 Team

| Role | Responsibilities |
|------|----------------|
| **Data Engineer — ETL** | Kaggle extraction, Pandas cleaning, Parquet output |
| **Data Engineer — Warehouse** | Snowflake schema design, Bronze/Silver/Gold SQL |
| **BI / Analytics Engineer** | Power BI dashboard, KPI definitions, metric validation |
| **DevOps / Platform Engineer** | Snowflake Tasks, GitHub Actions, README & documentation |

---

## 📚 Data Source

**Brazilian E-Commerce Public Dataset by Olist**  
- 🔗 [Kaggle Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce)  
- 📏 Size: 126.19 MB (Version 2)  
- 📅 Period: September 2016 – August 2018  
- 📜 License: [CC BY-NC-SA 4.0](https://creativecommons.org/licenses/by-nc-sa/4.0/)

---

## 🎓 About DEPI

This project was built as a **graduation deliverable** for the [Digital Egypt Pioneers Initiative (DEPI)](https://depi.gov.eg/) — a free, government-funded program by Egypt's Ministry of Communications & Information Technology, designed to equip Egyptian youth with advanced ICT and data skills.

- **Track:** Data Engineering  
- **Batch:** 4  
- **Year:** 2025

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.

---

<div align="center">

**Built with ❤️ by DEPI Batch 4 — Data Engineering Team · Egypt · 2025**

</div>
