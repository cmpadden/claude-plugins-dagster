# Cloud Platforms

Comprehensive integrations with major cloud providers for compute, storage, and managed services.

---

### AWS
**Package:** `dagster-aws` | **Support:** Dagster-supported

Comprehensive Amazon Web Services integration covering S3, Athena, Glue, ECS, EMR, Lambda, Redshift, Secrets Manager, and more.

**Use cases:**
- Store and retrieve files from S3 buckets
- Query data using Athena SQL engine
- Run containerized tasks on ECS or EMR clusters
- Manage secrets with AWS Secrets Manager

**Quick start:**
```python
from dagster_aws.s3 import S3Resource

s3 = S3Resource(
    region_name="us-west-2"
)

@dg.asset
def my_data(s3: S3Resource):
    return s3.get_client().get_object(
        Bucket="my-bucket",
        Key="data.csv"
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/aws

---

### Azure
**Package:** `dagster-azure` | **Support:** Dagster-supported

Microsoft Azure integration for Blob Storage, Data Lake Storage Gen2, Azure Databricks, and Synapse Analytics.

**Use cases:**
- Store data in Azure Blob Storage
- Access Azure Data Lake for large-scale analytics
- Run Databricks jobs on Azure infrastructure
- Integrate with Azure Synapse for data warehousing

**Quick start:**
```python
from dagster_azure.adls2 import ADLS2Resource

adls2 = ADLS2Resource(
    storage_account="myaccount",
    credential=dg.EnvVar("AZURE_CREDENTIAL")
)

@dg.asset
def azure_data(adls2: ADLS2Resource):
    return adls2.get_client().read_file(
        "my-container", "data.parquet"
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/azure

---

### GCP (Google Cloud Platform)
**Package:** `dagster-gcp` | **Support:** Dagster-supported

Google Cloud Platform services including Cloud Storage (GCS), BigQuery, Dataproc, and Cloud Run.

**Use cases:**
- Store files in Google Cloud Storage
- Query data with BigQuery
- Run Spark jobs on Dataproc clusters
- Deploy Dagster code on Cloud Run

**Quick start:**
```python
from dagster_gcp import GCSResource

gcs = GCSResource(
    project="my-project"
)

@dg.asset
def gcs_data(gcs: GCSResource):
    return gcs.get_client().download_as_bytes(
        "my-bucket", "data.json"
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/gcp

**Related:**
- **BigQuery** - Separate integration for Google's data warehouse (see Data Warehouses)
- **Dataproc** - Managed Spark/Hadoop service integration

---

### Databricks
**Package:** `dagster-databricks` | **Support:** Dagster-supported

Unified analytics platform integration using PipesDatabricksClient to run Python code on Databricks clusters.

**Use cases:**
- Execute notebooks on Databricks clusters
- Run Spark jobs for large-scale data processing
- Integrate ML workloads with Databricks ML
- Use Delta Lake for reliable data storage

**Quick start:**
```python
from dagster_databricks import PipesDatabricksClient

databricks = PipesDatabricksClient(
    host=dg.EnvVar("DATABRICKS_HOST"),
    token=dg.EnvVar("DATABRICKS_TOKEN")
)

@dg.asset
def databricks_job(
    context: dg.AssetExecutionContext,
    databricks: PipesDatabricksClient
):
    return databricks.run(
        context=context,
        task_key="my-task",
        cluster={"cluster_id": "1234-567890-abc123"},
        python_file="dbfs:/scripts/process.py"
    ).get_results()
```

**Docs:** https://docs.dagster.io/integrations/libraries/databricks

---

## Cloud Provider Selection Guide

| Need | AWS | Azure | GCP |
|------|-----|-------|-----|
| Object storage | S3 | Blob Storage | Cloud Storage (GCS) |
| Data warehouse | Redshift | Synapse | BigQuery |
| Serverless compute | Lambda | Functions | Cloud Functions/Run |
| Container orchestration | ECS | AKS | GKE |
| Managed Spark | EMR | Databricks | Dataproc |

## Common Patterns

### File Storage Pattern
```python
# All cloud providers follow similar patterns
@dg.asset
def load_from_cloud(cloud_resource: CloudResource):
    # Download file
    data = cloud_resource.download("bucket/path/to/file.csv")
    return pd.read_csv(data)

@dg.asset
def save_to_cloud(cloud_resource: CloudResource, data: pd.DataFrame):
    # Upload file
    cloud_resource.upload(
        data.to_csv(),
        "bucket/path/to/output.csv"
    )
```

### Cluster Compute Pattern
```python
# Databricks, EMR, Dataproc
@dg.asset
def distributed_processing(
    context: dg.AssetExecutionContext,
    pipes_client: PipesClient
):
    return pipes_client.run(
        context=context,
        script="process_large_dataset.py",
        cluster_config={...}
    ).get_results()
```

## Tips

- **Credentials**: Use environment variables or secret managers, never hardcode
- **Regions**: Specify regions close to your data for lower latency
- **Cost optimization**: Use spot/preemptible instances for non-critical workloads
- **Cross-cloud**: Different integrations can be used in the same Dagster deployment
