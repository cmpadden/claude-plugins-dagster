# Data Processing

DataFrame libraries and distributed processing frameworks for data transformations at any scale.

---

### PySpark
**Package:** `dagster-pyspark` | **Support:** Dagster-supported

Python API for Apache Spark enabling distributed data processing on large datasets across clusters.

**Use cases:**
- Process datasets too large for memory
- Distributed transformations across clusters
- Machine learning at scale with MLlib
- Join and aggregate massive tables

**Quick start:**
```python
from dagster_pyspark import PySparkResource
from pyspark.sql import DataFrame

pyspark = PySparkResource(
    spark_config={
        "spark.executor.memory": "4g",
        "spark.executor.cores": "2"
    }
)

@dg.asset
def process_large_data(pyspark: PySparkResource) -> DataFrame:
    spark = pyspark.spark_session
    df = spark.read.parquet("s3://bucket/large-dataset")
    return df.groupBy("category").agg({"amount": "sum"})
```

**Docs:** https://docs.dagster.io/integrations/libraries/pyspark

---

### Pandas
**Package:** `dagster-pandas` | **Support:** Dagster-supported

In-memory DataFrame library for data manipulation and analysis with type validation.

**Use cases:**
- Small to medium dataset transformations
- Data cleaning and preparation
- Exploratory data analysis
- CSV/Excel file processing

**Quick start:**
```python
from dagster_pandas import PandasColumn, create_dagster_pandas_dataframe_type
import pandas as pd

# Define DataFrame schema
EventDataFrame = create_dagster_pandas_dataframe_type(
    name="EventDataFrame",
    columns=[
        PandasColumn.integer_column("user_id", min_value=0),
        PandasColumn.string_column("event_type"),
        PandasColumn.datetime_column("timestamp")
    ]
)

@dg.asset
def events() -> EventDataFrame:
    return pd.DataFrame({
        "user_id": [1, 2, 3],
        "event_type": ["click", "view", "click"],
        "timestamp": pd.date_range("2024-01-01", periods=3)
    })
```

**Docs:** https://docs.dagster.io/integrations/libraries/pandas

---

### Polars
**Package:** `dagster-polars` | **Support:** Dagster-supported

Fast DataFrame library with columnar storage and lazy evaluation, often 5-10x faster than pandas.

**Use cases:**
- Fast in-memory transformations
- Processing medium to large datasets
- Lazy evaluation for query optimization
- Alternative to pandas with better performance

**Quick start:**
```python
from dagster_polars import PolarsDataFrame
import polars as pl

@dg.asset
def polars_transform() -> pl.DataFrame:
    return pl.DataFrame({
        "a": [1, 2, 3],
        "b": [4, 5, 6]
    }).with_columns(
        (pl.col("a") * pl.col("b")).alias("product")
    )

# Use lazy evaluation for large datasets
@dg.asset
def lazy_polars() -> pl.LazyFrame:
    return (
        pl.scan_parquet("large-file.parquet")
        .filter(pl.col("value") > 100)
        .group_by("category")
        .agg(pl.col("amount").sum())
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/polars

---

### Spark
**Package:** `dagster-spark` | **Support:** Dagster-supported

Apache Spark integration for distributed data processing across clusters (lower-level than PySpark).

**Use cases:**
- Manage Spark jobs and sessions
- Submit Spark applications
- Configure Spark clusters
- Low-level Spark control

**Quick start:**
```python
from dagster_spark import spark_resource

spark = spark_resource.configured({
    "spark_conf": {
        "spark.executor.memory": "2g"
    }
})

@dg.op(required_resource_keys={"spark"})
def spark_job(context):
    spark_session = context.resources.spark.spark_session
    df = spark_session.read.csv("data.csv")
    return df.count()
```

**Docs:** https://docs.dagster.io/integrations/libraries/spark

---

### Dask
**Package:** `dagster-dask` | **Support:** Dagster-supported

Parallel computing library with pandas-like API for larger-than-memory datasets.

**Use cases:**
- Scale pandas code to larger datasets
- Parallel computation on single machine or cluster
- Out-of-core processing
- Alternative to Spark for Python users

**Quick start:**
```python
from dagster_dask import dask_resource
import dask.dataframe as dd

dask = dask_resource.configured({
    "cluster": {"local": {"n_workers": 4}}
})

@dg.op(required_resource_keys={"dask"})
def dask_computation(context):
    # Read larger-than-memory CSV
    df = dd.read_csv("large-file-*.csv")
    result = df.groupby("category").amount.sum().compute()
    return result
```

**Docs:** https://docs.dagster.io/integrations/libraries/dask

---

### Docker
**Package:** `dagster-docker` | **Support:** Dagster-supported

Execute code in Docker containers using Dagster Pipes for isolated and reproducible environments.

**Use cases:**
- Run Python code in isolated environments
- Use different package versions per asset
- Execute non-Python code (R, Julia, etc.)
- Reproducible computation environments

**Quick start:**
```python
from dagster_docker import PipesDockerClient

docker = PipesDockerClient()

@dg.asset
def docker_computation(
    context: dg.AssetExecutionContext,
    docker: PipesDockerClient
):
    return docker.run(
        context=context,
        image="python:3.11",
        command=["python", "script.py"]
    ).get_results()
```

**Docs:** https://docs.dagster.io/integrations/libraries/docker

---

### Kubernetes
**Package:** `dagster-k8s` | **Support:** Dagster-supported

Execute code on Kubernetes pods using Pipes for scalable cloud-native execution.

**Use cases:**
- Run jobs on Kubernetes clusters
- Scale computation horizontally
- Use spot/preemptible instances
- Cloud-native data processing

**Quick start:**
```python
from dagster_k8s import PipesK8sClient

k8s = PipesK8sClient()

@dg.asset
def k8s_job(
    context: dg.AssetExecutionContext,
    k8s: PipesK8sClient
):
    return k8s.run(
        context=context,
        image="my-image:latest",
        command=["python", "process.py"],
        namespace="data-pipelines"
    ).get_results()
```

**Docs:** https://docs.dagster.io/integrations/libraries/k8s

---

### Celery
**Package:** `dagster-celery` | **Support:** Dagster-supported

Distributed task queue for executing Dagster ops across multiple workers.

**Use cases:**
- Distribute ops across worker nodes
- Queue-based job execution
- Scale computation horizontally
- Existing Celery infrastructure

**Quick start:**
```python
from dagster_celery import celery_executor

defs = dg.Definitions(
    assets=[...],
    jobs=[
        dg.define_asset_job(
            name="celery_job",
            executor_def=celery_executor
        )
    ]
)
```

**Docs:** https://docs.dagster.io/integrations/libraries/celery

---

## Processing Framework Selection

| Framework | Best For | Scale | Memory Model | Learning Curve |
|-----------|----------|-------|--------------|----------------|
| **Pandas** | Small datasets | < 10GB | In-memory | Low |
| **Polars** | Medium datasets | < 100GB | In-memory/streaming | Low |
| **Dask** | Larger datasets | 100GB - 1TB | Out-of-core | Medium |
| **PySpark** | Big data | > 1TB | Distributed | High |
| **Docker** | Isolated execution | Any | Depends on code | Medium |
| **K8s** | Cloud-native | Any | Distributed | High |

## Common Patterns

### In-Memory Processing (Pandas/Polars)
```python
@dg.asset
def transform_data() -> pd.DataFrame:
    df = pd.read_csv("input.csv")
    return df[df["value"] > 100].groupby("category").sum()
```

### Distributed Processing (Spark/Dask)
```python
@dg.asset
def distributed_transform(spark: PySparkResource):
    df = spark.spark_session.read.parquet("s3://large-dataset")
    return df.filter(df.value > 100).groupBy("category").sum()
```

### Containerized Processing (Docker/K8s)
```python
@dg.asset
def containerized_job(
    context: dg.AssetExecutionContext,
    pipes_client: PipesClient
):
    return pipes_client.run(
        context=context,
        image="my-processor:v1",
        command=["python", "process.py"]
    ).get_results()
```

## Tips

- **Start small**: Use Pandas/Polars until you hit memory limits
- **Lazy evaluation**: Polars and Spark support lazy evaluation for optimization
- **Partitioning**: Use partitioned assets for parallel processing
- **Testing**: Use small datasets locally, scale up in production
- **File formats**: Parquet is generally faster and more efficient than CSV
- **Caching**: Enable Spark caching for reused DataFrames
