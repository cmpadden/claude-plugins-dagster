---
name: integrations-index
description: Comprehensive index of 82+ Dagster integrations including cloud platforms (AWS, Azure, GCP), data warehouses (Snowflake, BigQuery), ETL tools (dbt, Fivetran, Airbyte), AI/ML (OpenAI, Anthropic), data quality, monitoring, and more. Use when discovering integrations or finding the right tool for a use case.
---

# Dagster Integrations Index

Navigate 82+ Dagster integrations organized by category. Find cloud platforms, data warehouses, ETL tools, AI/ML integrations, and more.

## Quick Reference by Category

| Category | Integrations | Common Tools | Reference |
|----------|--------------|--------------|-----------|
| **Cloud Platforms** | 4 | AWS, Azure, GCP, Databricks | `references/cloud-platforms.md` |
| **Data Warehouses** | 10 | Snowflake, BigQuery, Redshift, Postgres | `references/data-warehouses.md` |
| **ETL/ELT Tools** | 8 | dbt, Fivetran, Airbyte, dlt, Sling | `references/etl-tools.md` |
| **Data Processing** | 8 | Spark, PySpark, Dask, Pandas, Polars | `references/data-processing.md` |
| **AI & ML** | 8 | OpenAI, Anthropic, MLflow, W&B | `references/ai-ml.md` |
| **Data Quality** | 2 | Great Expectations, Pandera | `references/data-quality.md` |
| **Monitoring & Alerting** | 9 | Slack, PagerDuty, Datadog, MS Teams | `references/monitoring.md` |
| **BI & Visualization** | 7 | Looker, Tableau, PowerBI, Sigma, Hex | `references/bi-tools.md` |
| **Storage & Databases** | 26 | DuckDB, Postgres, MySQL, MongoDB | `references/storage-databases.md` |

## Top 10 Most Popular Integrations

### 1. dbt
Transform data using SQL models with automatic dependency management and incremental updates.
- **Package**: `dagster-dbt`
- **Docs**: https://docs.dagster.io/integrations/libraries/dbt

### 2. Snowflake
Cloud data warehouse for analytics with IO managers for pandas, polars, and pyspark DataFrames.
- **Package**: `dagster-snowflake`
- **Docs**: https://docs.dagster.io/integrations/libraries/snowflake

### 3. AWS
Comprehensive AWS services including S3, Athena, Glue, ECS, EMR, and more.
- **Package**: `dagster-aws`
- **Docs**: https://docs.dagster.io/integrations/libraries/aws

### 4. Databricks
Unified analytics platform with PipesDatabricksClient for running code on Databricks clusters.
- **Package**: `dagster-databricks`
- **Docs**: https://docs.dagster.io/integrations/libraries/databricks

### 5. Slack
Send notifications and alerts to Slack channels for pipeline monitoring.
- **Package**: `dagster-slack`
- **Docs**: https://docs.dagster.io/integrations/libraries/slack

### 6. Fivetran
Orchestrate Fivetran connectors for automated data ingestion from SaaS applications.
- **Package**: `dagster-fivetran`
- **Docs**: https://docs.dagster.io/integrations/libraries/fivetran

### 7. OpenAI
Integrate OpenAI API for LLM-powered data processing and AI workflows.
- **Package**: `dagster-openai`
- **Docs**: https://docs.dagster.io/integrations/libraries/openai

### 8. Airbyte
Manage Airbyte connections for ELT data movement from various sources.
- **Package**: `dagster-airbyte`
- **Docs**: https://docs.dagster.io/integrations/libraries/airbyte

### 9. Great Expectations
Validate data quality with test suites and expectations.
- **Package**: `dagster-ge`
- **Docs**: https://docs.dagster.io/integrations/libraries/great-expectations

### 10. PySpark
Run distributed data processing jobs using Apache Spark.
- **Package**: `dagster-pyspark`
- **Docs**: https://docs.dagster.io/integrations/libraries/pyspark

## Finding the Right Integration

### I need to...

**Load data from external sources**
- SaaS applications → [ETL Tools](#etl-tools) (Fivetran, Airbyte)
- Files/databases → [ETL Tools](#etl-tools) (dlt, Sling, Meltano)
- Cloud storage → [Cloud Platforms](#cloud-platforms) (AWS S3, GCS, Azure Blob)

**Transform data**
- SQL transformations → [ETL Tools](#etl-tools) (dbt)
- DataFrame operations → [Data Processing](#data-processing) (Pandas, Polars, PySpark)
- Large-scale processing → [Data Processing](#data-processing) (Spark, Dask)

**Store data**
- Cloud data warehouse → [Data Warehouses](#data-warehouses) (Snowflake, BigQuery, Redshift)
- Relational database → [Storage & Databases](#storage-databases) (Postgres, MySQL)
- File storage → [Cloud Platforms](#cloud-platforms) (S3, GCS, Azure)
- Analytics database → [Storage & Databases](#storage-databases) (DuckDB)

**Validate data quality**
- Schema validation → [Data Quality](#data-quality) (Pandera)
- Quality checks → [Data Quality](#data-quality) (Great Expectations)

**Run ML workloads**
- LLM integration → [AI & ML](#ai-ml) (OpenAI, Anthropic, Gemini)
- Experiment tracking → [AI & ML](#ai-ml) (MLflow, W&B)
- Model training → [Data Processing](#data-processing) (PySpark, Dask)

**Monitor pipelines**
- Team notifications → [Monitoring & Alerting](#monitoring) (Slack, MS Teams)
- Incident management → [Monitoring & Alerting](#monitoring) (PagerDuty, Datadog)
- Metrics tracking → [Monitoring & Alerting](#monitoring) (Prometheus, Datadog)

**Visualize data**
- BI dashboards → [BI & Visualization](#bi-tools) (Looker, Tableau, PowerBI)
- Analytics platform → [BI & Visualization](#bi-tools) (Sigma, Hex, Evidence)

## Integration Categories

### Cloud Platforms

Major cloud providers with comprehensive service integrations for compute, storage, and managed services.

**Key integrations:**
- **AWS** - S3, Athena, Glue, ECS, EMR, Lambda, Redshift
- **Azure** - Blob Storage, Data Lake, Synapse, Databricks
- **GCP** - Cloud Storage, BigQuery, Dataproc, Cloud Run
- **Databricks** - Unified analytics with Pipes integration

See `references/cloud-platforms.md` for all cloud platform integrations.

### Data Warehouses

Cloud and on-premise data warehouses optimized for analytics queries and large-scale storage.

**Key integrations:**
- **Snowflake** - Cloud data warehouse with IO managers
- **BigQuery** - Google's serverless data warehouse
- **Redshift** - AWS managed data warehouse
- **Postgres** - Open-source relational database
- **Teradata** - Enterprise data warehouse

See `references/data-warehouses.md` for all data warehouse integrations.

### ETL Tools

Extract, transform, and load tools for data ingestion and orchestration.

**Key integrations:**
- **dbt** - SQL-based transformation with automatic dependencies
- **Fivetran** - Automated SaaS data ingestion
- **Airbyte** - Open-source ELT platform
- **dlt** - Python-based data loading tool
- **Sling** - High-performance data replication
- **Meltano** - ELT for the modern data stack

See `references/etl-tools.md` for all ETL/ELT integrations.

### Data Processing

DataFrame libraries and distributed processing frameworks for data transformations.

**Key integrations:**
- **PySpark** - Python API for Apache Spark
- **Pandas** - In-memory DataFrame library
- **Polars** - Fast DataFrame library with columnar storage
- **Dask** - Parallel computing with pandas-like API
- **Spark** - Distributed data processing engine

See `references/data-processing.md` for all data processing integrations.

### AI & ML

Machine learning platforms, LLM APIs, and experiment tracking tools.

**Key integrations:**
- **OpenAI** - GPT models and embeddings API
- **Anthropic** - Claude AI models
- **Gemini** - Google's multimodal AI
- **MLflow** - Experiment tracking and model registry
- **Weights & Biases** - ML experiment tracking
- **NotDiamond** - LLM routing and optimization

See `references/ai-ml.md` for all AI/ML integrations.

### Data Quality & Testing

Validation frameworks for ensuring data quality and schema compliance.

**Key integrations:**
- **Great Expectations** - Data validation with expectations
- **Pandera** - Statistical data validation for DataFrames

See `references/data-quality.md` for all data quality integrations.

### Monitoring & Alerting

Notification systems and monitoring platforms for pipeline observability.

**Key integrations:**
- **Slack** - Team messaging and alerts
- **PagerDuty** - Incident management
- **Datadog** - Monitoring and observability
- **MS Teams** - Microsoft Teams notifications
- **Twilio** - SMS and voice notifications
- **Prometheus** - Metrics collection

See `references/monitoring.md` for all monitoring integrations.

### BI & Visualization

Business intelligence and visualization platforms for data exploration.

**Key integrations:**
- **Looker** - Google's BI platform
- **Tableau** - Interactive dashboards
- **PowerBI** - Microsoft's BI tool
- **Sigma** - Cloud analytics platform
- **Hex** - Collaborative notebooks
- **Evidence** - Markdown-based BI

See `references/bi-tools.md` for all BI integrations.

### Storage & Databases

Relational databases, NoSQL stores, and specialized storage systems.

**Key integrations:**
- **DuckDB** - In-process SQL analytics
- **Postgres** - Relational database
- **MySQL** - Open-source database
- **MongoDB** - Document database
- Many more specialized storage systems

See `references/storage-databases.md` for all storage integrations.

## References

- **Cloud Platforms**: `references/cloud-platforms.md`
- **Data Warehouses**: `references/data-warehouses.md`
- **ETL/ELT Tools**: `references/etl-tools.md`
- **Data Processing**: `references/data-processing.md`
- **AI & ML**: `references/ai-ml.md`
- **Data Quality**: `references/data-quality.md`
- **Monitoring & Alerting**: `references/monitoring.md`
- **BI & Visualization**: `references/bi-tools.md`
- **Storage & Databases**: `references/storage-databases.md`

## Using Integrations

Most Dagster integrations follow a common pattern:

1. **Install the package**:
   ```bash
   pip install dagster-<integration>
   ```

2. **Import and configure a resource**:
   ```python
   from dagster_<integration> import <Integration>Resource

   resource = <Integration>Resource(
       config_param=dg.EnvVar("ENV_VAR")
   )
   ```

3. **Use in your assets**:
   ```python
   @dg.asset
   def my_asset(integration: <Integration>Resource):
       # Use the integration
       pass
   ```

For component-based integrations (dbt, Fivetran, dlt, Sling), see the specific reference files for scaffolding and configuration patterns.
