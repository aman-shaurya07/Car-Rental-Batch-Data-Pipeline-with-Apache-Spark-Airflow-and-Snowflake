# Car Rental Data Batch Ingestion Project

This project implements a batch data ingestion pipeline for a car rental service. It utilizes tools such as Apache Airflow, PySpark, and Snowflake to process, transform, and store data.

---

## Project Structure

```
Car-Rental-Batch-Ingestion/
├── airflow_dag.py              # Airflow DAG to orchestrate the pipeline
├── spark_job.py                # PySpark job for data transformation
├── snowflake_dwh_setup.sql     # Snowflake DWH schema setup script
├── customers_*.csv             # Sample customer data files
├── car_rental_*.json           # Sample car rental data files
```

---

## Features

- **Airflow DAG**: Orchestrates the pipeline to process data, perform ETL, and load data into Snowflake.
- **PySpark Job**: Validates, transforms, and aggregates raw data from GCS.
- **Snowflake Schema**: Contains dimension and fact tables to store processed data.

---

## Requirements

Ensure the following dependencies are installed:

1. **Python Libraries**:
   - Apache Airflow (`pip install apache-airflow`)
   - Snowflake Connector (`pip install snowflake-connector-python`)
   - PySpark (`pip install pyspark`)

2. **Snowflake Setup**:
   - Snowflake account with warehouse, database, and schema access.

3. **Google Cloud Setup**:
   - GCS bucket with the raw data files.
   - Dataproc cluster for PySpark jobs.

---

## Setup

### 1. Snowflake Data Warehouse
Run `snowflake_dwh_setup.sql` in Snowflake to set up the schema and staging area.

```bash
snowsql -f snowflake_dwh_setup.sql
```

### 2. Configure Airflow
- Define connections in Airflow:
  - `snowflake_conn_v2`: Connection to Snowflake.
  - `google_cloud_default`: Connection to GCS.

- Add the DAG `airflow_dag.py` to your Airflow directory:

```bash
cp airflow_dag.py $AIRFLOW_HOME/dags/
```

### 3. Run the Pipeline
Start the Airflow scheduler and trigger the DAG:

```bash
airflow scheduler
airflow dags trigger car_rental_data_pipeline
```

---

## Execution Instructions

### 1. Data Processing with PySpark
Run `spark_job.py` for standalone data processing:

```bash
spark-submit --jars <path_to_snowflake_jars> --py-files dependencies.zip spark_job.py --date=20240703
```

### 2. Airflow DAG Workflow
- Airflow extracts the data from GCS.
- Performs transformations using PySpark.
- Loads the data into Snowflake dimensions and fact tables.

### 3. Verifying Data in Snowflake
Use the following queries to validate the data:

```sql
SELECT * FROM customer_dim;
SELECT * FROM rentals_fact;
```

---

## Notes
- Ensure GCS bucket and Snowflake integration is properly configured.
- Use Airflow's UI to monitor DAG execution and debug any errors.

---

## Project Insights
This pipeline demonstrates a scalable architecture for batch data ingestion, transformation, and loading. Future enhancements could include:

- Adding real-time processing.
- Implementing data quality checks.
- Automating error handling and retries.

