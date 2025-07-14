# 🧠 OLAP Data Modeling with Databricks & Medallion Architecture

This project simulates a real-world data engineering workflow. It focuses on building a complete **OLAP data model** using the **Medallion architecture (Bronze, Silver, Gold layers)** on **Databricks**, while implementing best practices like **ETL pipelines**, **parameterized SQL**, and **Slowly Changing Dimensions (SCD Type 1 & Type 2)**.

---

## 🚀 Objective

To simulate a production-like scenario where a business brings in new data into an existing system. You, as the Data Engineer, are responsible for handling:
- Data ingestion (incremental)
- Transformation
- Dimensional modeling
- SCD implementation
- ETL orchestration in notebooks

---

## 🧱 Architecture Overview

### 🔹 Bronze Layer
- Ingests raw data incrementally.
- Uses a transient layer for fast temporary processing.
- `bronze_table` is created using parameterized SQL based on `last_load_date`.

### 🔸 Silver Layer
- Light transformations applied (e.g., name splitting).
- Temp views are promoted to a permanent table with a `MERGE INTO` (Upsert) strategy.
- Maintains data freshness without duplication.

### 🥇 Gold Layer
- Dimensional modeling (Star Schema).
- Surrogate keys for dimension tables (for optimized joins).
- SCD Type 1 & Type 2 implementations for historical tracking.

---

## 📚 Notebooks

### 1. `SourceNotebook`
- Creates a simulated pre-existing source: `datamodeling.default.source_data`
- Adds new data later to test incremental loads.

### 2. `Bronze`
- Incremental ingestion with date tracking.
- Creates `bronze_table` using temp views and transformation logic.

### 3. `Silver`
- Splits customer names into `first_name`, `last_name`.
- Adds `processDate`.
- Upserts to `silver_table`.

### 4. `Gold`
- Creates dimension tables:
  - `DimProducts`, `DimCustomers`, `DimSales`, `DimPayments`, `DimRegion`
- Creates fact table `FactSales` by joining dimension keys.
- Star schema fully implemented.

### 5. `SCDs`
- Implements:
  - `SCD Type 1`: Overwrites old values with new
  - `SCD Type 2`: Tracks changes with start_date, end_date, and `is_current` flag

---

## 🧮 Fact Table Types

| Type | Description |
|------|-------------|
| Transactional | Records facts at atomic transaction level (e.g. `FactSales`) |
| Periodic | Captures snapshot at regular intervals |
| Accumulating | Tracks progress across workflow stages (e.g. order lifecycle) |
| Factless | No numeric metrics, used for tracking events or coverage |

---

## 🗃️ Dimension Types

| Type | Description |
|------|-------------|
| Conformed | Shared across multiple fact tables (e.g. `DimCustomer`) |
| Degenerate | Appears in fact table without related dimension table (e.g. `order_id`) |
| Junk | Combines low-cardinality attributes (e.g. status flags) |
| Role-Playing | Single dimension used in different roles (e.g. `Date` used as `OrderDate`, `ShipDate`) |

---

## 🛠 Tech Stack

- 🧠 **Databricks (SQL & PySpark)**
- 🗃️ Delta Lake
- 🧱 Medallion Architecture (Bronze/Silver/Gold)
- 📦 Notebook-based ETL
- 📈 OLAP + Star Schema

---

## 📂 Project Structure

```
├── notebooks/
│   ├── SCDsipynb
│   ├── Gold.ipynb
│   ├── Silver.ipynb
│   ├── Bronze.ipynb
│   ├── SourceNotebook.ipynb
├── README.md
```
---

## ✅ Future Improvements
- Automate with workflows (Databricks Jobs or Airflow)
- Integrate CI/CD using GitHub Actions
- Add unit tests for transformation logic

---

## 🧑‍💻 Author
**Dimakatso Malopemojapelo**  
_Data Engineer in training, focused on building robust, scalable data pipelines._



