# Storage & Databases

Relational databases, NoSQL stores, file systems, and specialized storage systems for data persistence.

This category includes databases covered in other sections:
- **Data warehouses** (Snowflake, BigQuery, etc.) - See `data-warehouses.md`
- **Cloud storage** (S3, GCS, Azure Blob) - See `cloud-platforms.md`

---

## Relational Databases

### DuckDB
**Package:** `dagster-duckdb` | **Support:** Dagster-supported

In-process SQL analytics database, excellent for local development and single-machine analytics.

**Use cases:**
- Local development without external database
- Fast SQL queries on parquet/CSV files
- Embedded analytics in applications
- Testing and prototyping

**Quick start:**
```python
from dagster_duckdb import DuckDBResource
from dagster_duckdb_pandas import DuckDBPandasIOManager

duckdb = DuckDBResource(database="analytics.duckdb")

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

### Postgres (PostgreSQL)
**Package:** `dagster-postgres` | **Support:** Dagster-supported

Open-source relational database with ACID compliance and rich feature set.

**Use cases:**
- Transactional data storage
- Relational data modeling
- Application backend database
- Dagster instance storage

**Docs:** https://docs.dagster.io/integrations/libraries/postgres

_(See data-warehouses.md for full details)_

---

### MySQL
**Package:** `dagster-mysql` | **Support:** Dagster-supported

Popular open-source relational database management system.

**Use cases:**
- Web application databases
- Transactional workloads
- Legacy system integration
- Read replicas for analytics

**Docs:** https://docs.dagster.io/integrations/libraries/mysql

_(See data-warehouses.md for full details)_

---

### MongoDB
**Package:** `dagster-mongo` | **Support:** Community-supported

Document-oriented NoSQL database for flexible schema and high scalability.

**Use cases:**
- Store semi-structured data
- High-write workloads
- Flexible schemas
- Document storage

**Quick start:**
```python
from dagster_mongo import MongoResource

mongo = MongoResource(
    host="localhost",
    port=27017,
    database="mydb"
)

@dg.asset
def mongo_data(mongo: MongoResource):
    client = mongo.get_client()
    collection = client["mydb"]["users"]
    return list(collection.find({"status": "active"}))
```

**Docs:** https://docs.dagster.io/integrations/libraries/mongo

---

## Vector & Search Databases

### Weaviate
**Package:** `dagster-weaviate` | **Support:** Community-supported

Vector database for AI-powered search and semantic similarity.

**Use cases:**
- Store and search embeddings
- Semantic search applications
- Recommendation systems
- RAG (Retrieval Augmented Generation)

**Quick start:**
```python
from dagster_weaviate import WeaviateResource

weaviate = WeaviateResource(
    url="http://localhost:8080",
    auth_api_key=dg.EnvVar("WEAVIATE_API_KEY")
)

@dg.asset
def store_embeddings(
    embeddings: list[list[float]],
    weaviate: WeaviateResource
):
    client = weaviate.get_client()
    # Store vectors in Weaviate
    for i, vector in enumerate(embeddings):
        client.data_object.create(
            {"text": f"document_{i}"},
            "Document",
            vector=vector
        )
```

**Docs:** https://docs.dagster.io/integrations/libraries/weaviate

---

### Chroma
**Package:** `dagster-chroma` | **Support:** Community-supported

Open-source embedding database for AI applications and vector search.

**Use cases:**
- Store document embeddings
- Build RAG applications
- Semantic search
- AI memory systems

**Quick start:**
```python
from dagster_chroma import ChromaResource

chroma = ChromaResource(
    host="localhost",
    port=8000
)

@dg.asset
def chroma_collection(chroma: ChromaResource):
    client = chroma.get_client()
    collection = client.create_collection("documents")
    collection.add(
        documents=["doc1", "doc2"],
        embeddings=[[1, 2, 3], [4, 5, 6]],
        ids=["id1", "id2"]
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/chroma

---

### Qdrant
**Package:** `dagster-qdrant` | **Support:** Community-supported

High-performance vector similarity search engine.

**Use cases:**
- Large-scale vector search
- Recommendation engines
- Image similarity search
- Neural search applications

**Quick start:**
```python
from dagster_qdrant import QdrantResource

qdrant = QdrantResource(
    url="http://localhost:6333",
    api_key=dg.EnvVar("QDRANT_API_KEY")
)

@dg.asset
def qdrant_vectors(qdrant: QdrantResource):
    client = qdrant.get_client()
    # Create collection and add vectors
    client.create_collection(
        collection_name="documents",
        vectors_config={"size": 384, "distance": "Cosine"}
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/qdrant

---

## File & Object Storage

### LakeFS
**Package:** `dagster-lakefs` | **Support:** Community-supported

Git-like version control for data lakes with branching and merging.

**Use cases:**
- Version control for data lakes
- Data experimentation with branches
- Reproducible data pipelines
- Data rollback capabilities

**Quick start:**
```python
from dagster_lakefs import LakeFSResource

lakefs = LakeFSResource(
    endpoint_url="http://localhost:8000",
    access_key_id=dg.EnvVar("LAKEFS_ACCESS_KEY"),
    secret_access_key=dg.EnvVar("LAKEFS_SECRET_KEY")
)

@dg.asset
def lakefs_data(lakefs: LakeFSResource):
    client = lakefs.get_client()
    # Read from specific branch
    data = client.read_object(
        repository="my-repo",
        ref="main",
        path="data/file.parquet"
    )
    return data
```

**Docs:** https://docs.dagster.io/integrations/libraries/lakefs

---

### Obstore
**Package:** `dagster-obstore` | **Support:** Community-supported

Universal object store abstraction supporting S3, GCS, Azure, and local files.

**Use cases:**
- Cloud-agnostic object storage
- Unified API for multiple clouds
- Local development with production parity
- Multi-cloud deployments

**Quick start:**
```python
from dagster_obstore import ObstoreResource

obstore = ObstoreResource(
    store_url="s3://my-bucket/path"
    # or "gs://bucket", "az://container", "file:///local/path"
)

@dg.asset
def cloud_agnostic_storage(obstore: ObstoreResource):
    # Works with any cloud
    data = obstore.read("data.parquet")
    return pd.read_parquet(data)
```

**Docs:** https://docs.dagster.io/integrations/libraries/obstore

---

## Table Formats

### Delta Lake
**Package:** `dagster-deltalake` | **Support:** Dagster-supported

ACID transactions and time travel for data lakes built on Parquet.

**Docs:** https://docs.dagster.io/integrations/libraries/deltalake

_(See data-warehouses.md for full details)_

---

### Iceberg
**Package:** `dagster-iceberg` | **Support:** Community-supported

Apache Iceberg table format for large analytic datasets.

**Docs:** https://docs.dagster.io/integrations/libraries/iceberg

_(See data-warehouses.md for full details)_

---

## Metadata & Catalog

### DataHub
**Package:** `dagster-datahub` | **Support:** Community-supported

Metadata catalog for data discovery, lineage, and governance.

**Use cases:**
- Publish Dagster lineage to DataHub
- Data discovery and search
- Metadata management
- Data governance

**Quick start:**
```python
from dagster_datahub import DataHubResource

datahub = DataHubResource(
    server_url="http://localhost:8080",
    token=dg.EnvVar("DATAHUB_TOKEN")
)

@dg.asset
def publish_to_datahub(datahub: DataHubResource):
    # Publish metadata to DataHub
    datahub.emit_metadata(
        dataset_urn="urn:li:dataset:my_table",
        metadata={"description": "User data"}
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/datahub

---

### Atlan
**Package:** `dagster-atlan` | **Support:** Community-supported

Modern data catalog for discovery, collaboration, and governance.

**Use cases:**
- Data catalog integration
- Metadata synchronization
- Data governance
- Lineage tracking

**Quick start:**
```python
from dagster_atlan import AtlanResource

atlan = AtlanResource(
    base_url="https://company.atlan.com",
    api_key=dg.EnvVar("ATLAN_API_KEY")
)

@dg.asset
def sync_to_atlan(atlan: AtlanResource):
    client = atlan.get_client()
    # Sync Dagster assets to Atlan catalog
    client.create_or_update_asset(
        name="my_asset",
        type="TABLE",
        metadata={...}
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/atlan

---

### Secoda
**Package:** `dagster-secoda` | **Support:** Community-supported

Data discovery and documentation platform.

**Use cases:**
- Automated documentation
- Data lineage visualization
- Team collaboration on data
- Data search and discovery

**Quick start:**
```python
from dagster_secoda import SecodaResource

secoda = SecodaResource(
    api_key=dg.EnvVar("SECODA_API_KEY"),
    workspace_url="https://company.secoda.co"
)

@dg.asset
def publish_to_secoda(secoda: SecodaResource):
    # Publish Dagster metadata to Secoda
    secoda.sync_assets()
```

**Docs:** https://docs.dagster.io/integrations/libraries/secoda

---

## Other Storage Systems

### Census
**Package:** `dagster-census` | **Support:** Community-supported

Reverse ETL platform for syncing warehouse data to business tools.

**Use cases:**
- Sync warehouse data to CRMs
- Populate marketing tools with customer data
- Activate data in business applications
- Operational analytics

**Quick start:**
```python
from dagster_census import CensusResource

census = CensusResource(
    api_key=dg.EnvVar("CENSUS_API_KEY")
)

@dg.asset
def trigger_census_sync(census: CensusResource):
    # Trigger Census sync from warehouse to Salesforce
    census.get_client().trigger_sync(
        sync_id="sync-123"
    )
```

**Docs:** https://docs.dagster.io/integrations/libraries/census

---

### SharePoint
**Package:** `dagster-sharepoint` | **Support:** Community-supported

Microsoft SharePoint integration for file storage and collaboration.

**Use cases:**
- Read files from SharePoint
- Upload reports to SharePoint
- Enterprise file system integration
- Microsoft 365 integration

**Quick start:**
```python
from dagster_sharepoint import SharePointResource

sharepoint = SharePointResource(
    site_url="https://company.sharepoint.com/sites/data",
    client_id=dg.EnvVar("SP_CLIENT_ID"),
    client_secret=dg.EnvVar("SP_CLIENT_SECRET")
)

@dg.asset
def sharepoint_file(sharepoint: SharePointResource):
    client = sharepoint.get_client()
    file_content = client.download_file(
        library="Documents",
        file_path="data/report.xlsx"
    )
    return pd.read_excel(file_content)
```

**Docs:** https://docs.dagster.io/integrations/libraries/sharepoint

---

## Storage Selection Guide

| Type | Best For | Examples | Scale |
|------|----------|----------|-------|
| **Relational** | Structured, transactional | Postgres, MySQL | Small-Large |
| **Analytics** | Analytical queries | DuckDB, Data warehouses | Any |
| **Document** | Semi-structured | MongoDB | Medium-Large |
| **Vector** | Embeddings, AI | Weaviate, Chroma, Qdrant | Medium-Large |
| **Object** | Files, unstructured | S3, GCS, Azure Blob | Any |
| **Table format** | Data lake tables | Delta, Iceberg | Large |
| **Metadata** | Catalogs, lineage | DataHub, Atlan | N/A |

## Common Patterns

### Local Development with DuckDB
```python
# Development
local_io_manager = DuckDBPandasIOManager(
    database="dev.duckdb"
)

# Production
prod_io_manager = SnowflakePandasIOManager(...)

defs = dg.Definitions(
    assets=[...],
    resources={
        "io_manager": (
            local_io_manager if dev_mode
            else prod_io_manager
        )
    }
)
```

### Vector Storage for AI
```python
@dg.asset
def embeddings(openai: OpenAIResource) -> list[list[float]]:
    # Generate embeddings
    return openai.create_embeddings(documents)

@dg.asset
def vector_index(
    embeddings: list[list[float]],
    weaviate: WeaviateResource
):
    # Store in vector database
    weaviate.store_vectors(embeddings)
```

### Multi-Storage Pattern
```python
@dg.asset
def raw_data() -> pd.DataFrame:
    return extract_data()

@dg.asset
def warehouse_table(raw_data: pd.DataFrame, snowflake: SnowflakeResource):
    # Store in warehouse for analytics
    snowflake.write_dataframe(raw_data, "analytics.raw")

@dg.asset
def app_database(raw_data: pd.DataFrame, postgres: PostgresResource):
    # Store in Postgres for application
    postgres.write_dataframe(raw_data, "public.users")

@dg.asset
def search_index(raw_data: pd.DataFrame, weaviate: WeaviateResource):
    # Create search index
    weaviate.index_documents(raw_data)
```

## Tips

- **Development**: Use DuckDB/SQLite for local development, switch to cloud for production
- **Vector DBs**: Choose based on scale - Chroma for small, Qdrant/Weaviate for large
- **Object storage**: Use cloud-native (S3/GCS/Azure) over databases for large files
- **Table formats**: Delta/Iceberg add ACID to data lakes, worth the complexity
- **Catalogs**: DataHub/Atlan provide discoverability for large data platforms
- **Partitioning**: Use partitioning for large tables to improve query performance
- **Compression**: Parquet with compression saves storage and improves performance
- **Indexes**: Add indexes on frequently queried columns
