Hopsworks is a **modular** MLOps platform with:

 - a feature store (available as standalone)
 - a data science and data engineering platform

<img src="../../assets/images/concepts/mlops/architecture.svg">

## Standalone Feature Store
Hopsworks was the first open-source and first enterprise feature store for ML. You can use Hopsworks as a standalone feature store with support for feature pipelines in Python, Spark, Flink, Beam, and DBT/SQL. Hopsworks enables you to retrieve feature data for training models and batch inference as either Pandas DataFrames or as files. For online inference, Hopsworks both language-level APIs (Python and Java) and a REST API for low-latency feature vector retrieval.

## Governance
Hopsworks provides a data-mesh architecture for managing ML assets and teams, with multi-tenant projects. Not unlike a GitHub repository, a project is a sandbox containing team members, data, and ML assets. In Hopsworks, all ML assets (features, models, training data) are versioned, taggable, lineage-tracked, and support free-text search. Data can be also be securely shared between projects.

## Data Science Platform
You can develop feature pipelines, batch/online inference pipelines, and training pipelines in Hopsworks. There is support for version control (GitHub, GitLab, BitBucket), Jupyter notebooks, a shared distributed file system, per project conda environments for managing python dependencies without needing to write Dockerfiles, jobs (Python, Spark, Flink), and job orchestration.

### Model Management
Hopsworks includes support for online models, with model deployments using [the KServe framework](https://github.com/kserve/kserve) and a model registry designed for both KServe and batch inference.

### Vector DB
Hopsworks provides a vector database, based on [OpenSearch kNN](https://opensearch.org/docs/latest/search-plugins/knn/index/) ([FAISS](https://ai.facebook.com/tools/faiss/) and [nmslib](https://github.com/nmslib/nmslib)). Hopsworks Vector DB includes out-of-the-box support for authentication, access control, filtering, backup-and-restore, and horizontal scalability. Hopsworks' Feature Store and vector database are often used together to build scalable recommender systems, such as ranking-and-retrieval for real-time recommendations. 

