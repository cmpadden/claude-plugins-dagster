# Data Warehouses

Cloud and on-premise data warehouses optimized for analytics queries and large-scale storage.

---

### Snowflake
**Package:** `dagster-snowflake` | **Support:** Dagster-supported

Cloud data warehouse with IO managers for pandas, polars, and PySpark DataFrames. Store and query large-scale analytics data.

**Use cases:**
- Store processed analytics tables for BI tools
- Query large datasets with SQL
- Integrate with dbt for SQL transformations
- Use as persistent storage for Dagster assets

**Quick start:**
```python
from dagster_snowflake import SnowflakeResource
from dagster_snowflake_pandas import SnowflakePandasIOManager

snowflake = SnowflakeResource(
    account="abc12345.us-east-1",
    user=dg.EnvVar("SNOWFLAKE_USER"),
    password=dg.EnvVar("SNOWFLAKE_PASSWORD"),
    database="analytics",
    schema="public"
)

# Use as IO Manager to auto-save DataFrames
defs = dg.Definitions(
    assets=[...],
    resources={
        "snowflake": snowflake,
        "io_manager": SnowflakePandasIOManager(
            resource=snowflake
        )
    }
)
```

**Docs:** https://docs.dagster.io/integrations/libraries/snowflake

---

### BigQuery
**Package:** `dagster-gcp` | **Support:** Dagster-supported

Google's serverless data warehouse with IO managers for pandas and PySpark. Automatically scales for large queries.

**Use cases:**
- Run SQL analytics on petabyte-scale data
- Store structured data for analysis
- Query data with standard SQL
- Integrate with GCP data pipeline

**Quick start:**
```python
from dagster_gcp import BigQueryResource
from dagster_gcp_pandas import BigQueryPandasIOManager

bigquery = BigQueryResource(
    project="my-project"
)

@dg.asset
def query_bigquery(bigquery: BigQueryResource):
    return bigquery.get_client().query(
        "SELECT * FROM `project.dataset.table` LIMIT 1000"
    ).to_dataframe()
```

**Docs:** https://docs.dagster.io/integrations/libraries/gcp

---

### Postgres
**Package:** `dagster-postgres` | **Support:** Dagster-supported

PostgreSQL relational database integration with IO managers for storing DataFrames and executing SQL queries.

**Use cases:**
- Store relational data with ACID guarantees
- Execute transactional queries
- Store Dagster asset data in tables
- Use as backend for Dagster instance storage

**Quick start:**
```python
from dagster_postgres import PostgresResource
from dagster_postgres_pandas import PostgresPandasIOManager

postgres = PostgresResource(
    host="localhost",
    port=5432,
    user=dg.EnvVar("POSTGRES_USER"),
    password=dg.EnvVar("POSTGRES_PASSWORD"),
    database="analytics"
)

@dg.asset
def postgres_table(postgres: PostgresResource):
    with postgres.get_connection() as conn:
        return pd.read_sql("SELECT * FROM users", conn)
```

**Docs:** https://docs.dagster.io/integrations/libraries/postgres

---

### MySQL
**Package:** `dagster-mysql` | **Support:** Dagster-supported

MySQL relational database integration for executing queries and storing data.

**Use cases:**
- Connect to existing MySQL databases
- Query transactional data
- Store pipeline results in MySQL tables
- Integrate with MySQL-backed applications

**Quick start:**
```python
from dagster_mysql import MySQLResource

mysql = MySQLResource(
    host="localhost",
    port=3306,
    user=dg.EnvVar("MYSQL_USER"),
    password=dg.EnvVar("MYSQL_PASSWORD"),
    database="production"
)

@dg.asset
def mysql_data(mysql: MySQLResource):
    with mysql.get_connection() as conn:
        return pd.read_sql("SELECT * FROM orders", conn)
```

**Docs:** https://docs.dagster.io/integrations/libraries/mysql

---

### Redshift
**Package:** `dagster-aws` | **Support:** Dagster-supported

AWS managed data warehouse based on PostgreSQL, optimized for OLAP workloads.

**Use cases:**
- Store large analytics datasets on AWS
- Query data with PostgreSQL-compatible SQL
- Integrate with AWS data ecosystem
- Run complex analytical queries

**Quick start:**
```python
from dagster_aws.redshift import RedshiftResource

redshift = RedshiftResource(
    host="my-cluster.redshift.amazonaws.com",
    port=5439,
    user=dg.EnvVar("REDSHIFT_USER"),
    password=dg.EnvVar("REDSHIFT_PASSWORD"),
    database="analytics"
)

@dg.asset
def redshift_query(redshift: RedshiftResource):
    with redshift.get_connection() as conn:
        return pd.read_sql(
            "SELECT * FROM sales_summary",
            conn
        )
```

**Docs:** https://docs.dagster.io/integrations/libraries/aws

---

### Teradata
**Package:** `dagster-teradata` | **Support:** Community-supported

Enterprise data warehouse platform for large-scale analytics and parallel processing.

**Use cases:**
- Connect to enterprise Teradata deployments
- Execute parallel queries on large datasets
- Integrate Teradata with modern data stack
- Migrate from Teradata to cloud warehouses

**Quick start:**
```python
from dagster_teradata import TeradataResource

teradata = TeradataResource(
    host="teradata.company.com",
    user=dg.EnvVar("TERADATA_USER"),
    password=dg.EnvVar("TERADATA_PASSWORD")
)

@dg.asset
def teradata_data(teradata: TeradataResource):
    return teradata.execute_query(
        "SELECT * FROM enterprise_data"
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/teradata

---

### DuckDB
**Package:** `dagster-duckdb` | **Support:** Dagster-supported

In-process SQL analytics database, excellent for local development and small-to-medium datasets.

**Use cases:**
- Local analytics without external database
- Fast SQL queries on parquet/CSV files
- Development and testing of data pipelines
- Single-machine analytics workloads

**Quick start:**
```python
from dagster_duckdb import DuckDBResource
from dagster_duckdb_pandas import DuckDBPandasIOManager

duckdb = DuckDBResource(
    database="analytics.duckdb"
)

# Use as IO Manager
defs = dg.Definitions(
    assets=[...],
    resources={
        "io_manager": DuckDBPandasIOManager(
            database="analytics.duckdb"
        )
    }
)
```

**Docs:** https://docs.dagster.io/integrations/libraries/duckdb

---

### Delta Lake
**Package:** `dagster-deltalake` | **Support:** Dagster-supported

Open-source storage layer providing ACID transactions and time travel for data lakes.

**Use cases:**
- Reliable data lake storage with ACID guarantees
- Time travel and data versioning
- Schema evolution for data lakes
- Integration with Spark and Databricks

**Quick start:**
```python
from dagster_deltalake import DeltaLakeResource

deltalake = DeltaLakeResource(
    storage_options={
        "AWS_ACCESS_KEY_ID": dg.EnvVar("AWS_KEY"),
        "AWS_SECRET_ACCESS_KEY": dg.EnvVar("AWS_SECRET")
    }
)

@dg.asset
def delta_table(deltalake: DeltaLakeResource):
    return deltalake.read_table(
        "s3://bucket/delta/table"
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/deltalake

---

### Iceberg
**Package:** `dagster-iceberg` | **Support:** Community-supported

Apache Iceberg table format for large analytic datasets with schema evolution and partition evolution.

**Use cases:**
- Manage large analytics tables in data lakes
- Schema and partition evolution
- Time travel queries
- Multi-engine table access (Spark, Trino, etc.)

**Quick start:**
```python
from dagster_iceberg import IcebergResource

iceberg = IcebergResource(
    catalog_uri="thrift://localhost:9083",
    warehouse="s3://my-bucket/warehouse"
)

@dg.asset
def iceberg_table(iceberg: IcebergResource):
    return iceberg.read_table(
        "database.table_name"
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/iceberg

---

## Warehouse Selection Guide

| Feature | Snowflake | BigQuery | Redshift | Postgres | DuckDB |
|---------|-----------|----------|----------|----------|---------|
| **Scale** | Petabytes | Petabytes | Petabytes | Terabytes | GBs-TBs |
| **Cost model** | Storage + compute | On-demand queries | Provisioned clusters | Self-hosted | Free |
| **Best for** | Enterprise analytics | GCP-native | AWS-native | Transactional | Local/dev |
| **Concurrency** | Excellent | Excellent | Good | Good | Single-process |

## IO Manager Pattern

Most warehouse integrations provide IO managers to automatically persist DataFrames:

```python
from dagster_<warehouse>_pandas import <Warehouse>PandasIOManager

defs = dg.Definitions(
    assets=[my_asset],
    resources={
        "io_manager": <Warehouse>PandasIOManager(
            # connection config
        )
    }
)

# Assets automatically save to warehouse
@dg.asset
def my_table() -> pd.DataFrame:
    return pd.DataFrame({"col": [1, 2, 3]})
    # Automatically saved to warehouse as table
```

## Tips

- **Development**: Use DuckDB for local development, production warehouse in deployment
- **Cost**: BigQuery charges by query size, Snowflake by compute time
- **Performance**: Use partitioning and clustering for large tables
- **Schema**: Define schemas explicitly to avoid type inference issues
