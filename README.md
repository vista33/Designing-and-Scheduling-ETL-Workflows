# 🚀 ETL Workflow Design & Scheduling – Data Engineer Bootcamp

## 📘 Overview
This project is part of the **Data Engineer Bootcamp Assignment**, focused on building and scheduling an **ETL (Extract, Transform, Load)** workflow using **Apache Airflow**.  
The ETL pipeline automatically transfers and transforms data from a **PostgreSQL operational database** to a **MySQL data warehouse** for reporting and analytics.

---

## 🏢 Business Case – RetailCorp (E-commerce)

### Problem
The business team requires daily synchronization of customer, product, and order data from the operational PostgreSQL database to the MySQL data warehouse for reporting purposes.

### Objective
Build an automated ETL workflow that:
- Extracts data from PostgreSQL
- Transforms it according to business rules
- Loads it into MySQL
- Runs automatically **every 6 hours**

---

## 🎯 Learning Objectives

### 1. ETL Pipeline Concepts
- Understand the Extract–Transform–Load process in data engineering.
- Translate business requirements into automated workflows.
- Appreciate the role of automation in production pipelines.

### 2. Apache Airflow Mastery
- Create and configure DAGs (Directed Acyclic Graphs).
- Define task dependencies and orchestration logic.
- Use XCom for task communication.
- Implement retries and error handling.

### 3. Multi-Database Integration
- Connect to PostgreSQL with `PostgresHook`.
- Connect to MySQL with `MySqlHook`.
- Differentiate between OLTP (operational) and OLAP (analytical) systems.
- Perform CRUD and UPSERT operations.

### 4. Data Transformation
- Apply business logic during transformation.
- Clean and standardize data.
- Compute derived metrics (e.g. profit margin).
- Validate and handle data quality issues.

### 5. Best Practices
- Write clean, maintainable, PEP-8-compliant code.
- Implement logging and exception handling.
- Avoid hardcoded credentials.
- Ensure robust retry and recovery mechanisms.

---

## 🧠 Skills Practiced

**Technical Skills**
- Python programming for data engineering
- SQL queries (SELECT, JOIN, INSERT, UPDATE)
- Apache Airflow (DAGs, Operators, Hooks, XCom)
- Database connectivity and transactions
- Data validation and transformation logic

**Soft Skills**
- Problem solving
- Attention to detail
- Documentation and code readability
- Debugging and troubleshooting

---

## 🧩 Assignment Steps

### 1. DAG Configuration (20 Points)
**File:** `/airflow/dags/postgres_to_mysql_etl.py`

| Parameter | Value |
|------------|--------|
| DAG Name | `postgres_to_mysql_etl` |
| Schedule | Every 6 hours |
| Owner | `data-engineering-team` |
| Retries | 2 |
| Retry Delay | 5 minutes |
| Catchup | Disabled |
| Tags | `['etl', 'postgresql', 'mysql', 'data-pipeline']` |

**Example:**
```python
from datetime import timedelta
from airflow import DAG
from airflow.utils.dates import days_ago

default_args = {
    'owner': 'data-engineering-team',
    'retries': 2,
    'retry_delay': timedelta(minutes=5),
}

dag = DAG(
    'postgres_to_mysql_etl',
    default_args=default_args,
    schedule_interval=timedelta(hours=6),
    start_date=days_ago(1),
    catchup=False,
    tags=['etl', 'postgresql', 'mysql', 'data-pipeline'],
)
