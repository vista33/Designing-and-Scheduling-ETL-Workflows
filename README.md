🎯 Learning Objectives

After completing this assignment, you will be able to:

1. Understand ETL Pipeline Concepts

Comprehend the Extract, Transform, Load process in data engineering.

Translate business requirements into an automated ETL workflow.

Recognize the importance of automation in production pipelines.

2. Master Apache Airflow

Create and configure DAGs (Directed Acyclic Graphs).

Define task dependencies and use XCom for inter-task communication.

Implement retry mechanisms and error handling.

3. Handle Multi-Database Integration

Connect to PostgreSQL using PostgresHook.

Connect to MySQL using MySqlHook.

Understand differences between OLTP (operational) and OLAP (analytical) databases.

Perform CRUD and UPSERT operations across databases.

4. Perform Data Transformation

Apply business logic transformations.

Clean and standardize data.

Compute derived metrics (e.g., profit margin).

Validate and ensure data quality.

5. Follow Data Engineering Best Practices

Write clean, maintainable, and well-documented code.

Implement proper logging and error handling.

Avoid hardcoding credentials (use Airflow connections).

Ensure robust recovery and retry mechanisms.

🧠 Technical Skills Practiced

Python programming for data engineering

SQL queries (SELECT, JOIN, INSERT, UPDATE)

Apache Airflow (DAGs, Operators, Hooks, XCom)

Database connectivity & transactions

Data transformation and validation logic

💡 Soft Skills Practiced

Analytical problem-solving in data contexts

Attention to detail in transformation and validation

Documentation and readability

Debugging and troubleshooting

🧩 Assignment Steps
1. DAG Configuration

File Path: /airflow/dags/postgres_to_mysql_etl.py

Requirements:

DAG Name: postgres_to_mysql_etl

Schedule: Every 6 hours

Owner: data-engineering-team

Retries: 2

Retry Delay: 5 minutes

Catchup: Disabled

Tags: ['etl', 'postgresql', 'mysql', 'data-pipeline']

Example:

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

2. Extract Data from PostgreSQL 

Create three extraction functions using PythonOperator and PostgresHook:

2.1 Extract Customers 

Query: SELECT * FROM raw_data.customers WHERE updated_at >= CURRENT_DATE - INTERVAL '1 day';

Convert results to a list of dictionaries

Push to XCom with key 'customers_data'

2.2 Extract Products 

Query:

SELECT p.*, s.supplier_name 
FROM raw_data.products p 
JOIN raw_data.suppliers s ON p.supplier_id = s.supplier_id 
WHERE p.updated_at >= CURRENT_DATE - INTERVAL '1 day';


Push to XCom with key 'products_data'

2.3 Extract Orders 

Query: SELECT * FROM raw_data.orders WHERE updated_at >= CURRENT_DATE - INTERVAL '1 day';

Push to XCom with key 'orders_data'

3. Transform and Load into MySQL 

Create three transformation and loading functions using MySqlHook:

3.1 Customers 

Transformations:

Format phone numbers: (XXX) XXX-XXXX

Convert state codes to UPPERCASE

Loading:

Use UPSERT (INSERT ... ON DUPLICATE KEY UPDATE) into dim_customers

3.2 Products 

Transformations:

Calculate profit margin: ((price - cost) / price) * 100

Convert category to Title Case

Loading:

UPSERT into dim_products

3.3 Orders 

Transformations:

Convert status to lowercase

Validate total amount: if negative, set to 0 and log a warning

Loading:

UPSERT into fact_orders

4. Task Dependencies 

Define dependencies to ensure proper execution order:

extract_customers >> transform_and_load_customers
extract_products >> transform_and_load_products
extract_orders >> transform_and_load_orders

5. Code Quality 

Follow PEP 8 standards

No hardcoded credentials

Include docstrings and comments

Implement logging and exception handling

Close database connections properly

🛠 Tools & Environment

Apache Airflow

Docker

Visual Studio Code

PostgreSQL (source)

MySQL (destination)

Python 3.8+

🧾 Deliverables

By the end of this assignment, you should have:

✅ A fully functional Airflow DAG (postgres_to_mysql_etl.py)
✅ Automated ETL pipeline running every 6 hours
✅ Implemented data transformations meeting business rules
✅ Clear documentation and logging
✅ Robust error handling and retry mechanisms



🧠 Tips

Test each extraction and transformation function independently before connecting them in Airflow.

Use Airflow Connections (UI > Admin > Connections) for database credentials.

Add meaningful logs for each step for easier debugging.
