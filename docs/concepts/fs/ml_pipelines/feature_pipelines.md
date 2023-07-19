

A feature pipeline is a program that orchestrates the execution of a dataflow graph of feature functions (transformations on input data to create feature data). Examples of transformations include aggregations, dimensionality reduction (e.g., embeddings, PCA), and binning. 
 The computed features are saved to one or more feature groups. A feature pipeline can also validate the feature data using Great Expectations before it is saved to a feature group.

<img src="../../../../assets/images/concepts/fs/feature-pipelines.svg">

### Data Sources
Your feature pipeline needs to connect to some (external) data source to read the data to be processed. Python, Spark, Flink, and Beam have connectors to a huge number of different data sources, while SQL feature pipelines are often restricted to a single data source (for example, your connector to SnowFlake only runs SQL on SnowFlake). SparkSQL, in contrast, can be used over tables that originate in different data sources.

### Transformations
If you store transformed features in feature groups, the feature data is no longer useful for EDA (as it near to impossible for Data Scientists to understand the transformed values). It also makes it impossible for inference pipelines to log untransformed feature values and predictions for an operational model. There is one use case for storing transformed features in feature groups - when you need to have ultra low latency when reading precomputed features (and online transformations when reading features add too much latency for your use case). The figure below shows to include transformations in your feature pipelines. 

### Data Validation
In order to be able to train and serve models that you can rely on, you need clean, high quality features. Data validation operations include removing bad data, removing or imputing missing values, and identifying problems such as feature shift. HSFS supports Great Expectations to specify data validation rules that are executed in the client before features are written to the Feature Store. The validation results are collected and shown in Hopsworks.

### Aggregations

Aggregations are used to summarize large datasets into more concise, signal-rich features. Popular aggregations include count(), sum(), mean(), median(), stddev(), min(), and max(). These aggregations produce a single number (a numerical feature) that captures information about a potentially large dataset. Both numerical and categorical features are often transformed before being used to train or serve models.

### Dimensionality Reduction
If input data is impractically large or if it has a significant amount of redundancy, it can often be transformed into a reduced set of features with dimensionality reduction (often called feature extraction). Popular dimensionality algorithms include embedding algorithms, PCA, and TSNE.

### Model-Specific Transformations
Transformations are covered in more detail in [training/inference pipelines](../feature_view/training_inference_pipelines.md), as transformations typically happen after the feature store. If you store transformed features in feature groups, the feature data is no longer useful for EDA (as it near to impossible for Data Scientists to understand the transformed values). It also makes it impossible for inference pipelines to log untransformed feature values and predictions for an operational model. There is one use case for storing transformed features in feature groups - when you need to have ultra low latency when reading precomputed features (and online transformations when reading features add too much latency for your use case). The figure below shows to include transformations in your feature pipelines. 

<img src="../../../../assets/images/concepts/fs/feature-pipelines-with-transformations.svg">

### Feature Engineering in Python/PySpark
Python is the most widely used framework for feature engineering due to its extensive library support for aggregations (Pandas), data validation (Great Expectations), and dimensionality reduction (embeddings, PCA), and transformations (in Scikit-Learn, TensorFlow, PyTorch). Python also supports open-source feature engineering frameworks used for automated feature engineering, such as [featuretools](https://www.featuretools.com/) that supports relational and temporal sources.

PySpark is popular as a feature engineering framework as it can scale to process larger volumes of data than Python, and provides native support for aggregations, and it supports many of the same data validation (Great Expectations), and dimensionality reduction algorithms (embeddings, PCA) as Python. Spark also has native support for transformations, which are useful for analytical models (batch scoring). 

#### Feature Functions in Python
A feature function takes as input raw data and outputs a feature. Feature functions are currently only supported in Python, and can used in Python and PySpark feature pipelines and inference pipelines. You can define a feature function as a Python UDF (user-defined function) or a Pandas UDF. In general, it is best practice to use Pandas UDFs, as they are more scalable than Python UDFs (they can run natively in PySpark feature pipelines) and they add minimal overhead in online inference pipelines.

### Feature Engineering in SQL

SQL has grown in popularity for performing heavy lifting in feature pipelines - computing aggregates on data - when the input data already resides in a data warehouse. Data warehouses also support data validation, for example, through Great Expectations in DBT. However, SQL is not mature as a platform for transformations and dimensionality reductions, where UDFs are applied row-wise.

You can do aggregation in SQL for data in your data warehouse or database.


### Feature Engineering in Flink
Apache Flink is a powerful and flexible framework for stateful feature computation operations over unbounded and bounded data streams. It is used for feature engineering when you need very fresh features computed in real-time. Flink provides a rich set of operators and functions such as time windows and aggregation operations that can be applied to keyed and/or global window streams. Flink’s stateful operations allow users to maintain and update state across multiple data records or events, which is particularly useful for feature engineering tasks such as sessionization and/or maintaining rolling aggregates over a sliding window of data.

Flink feature engineering pipelines are supported in Java/Scala only.


### Feature Engineering in Beam
Beam feature engineering pipelines are supported in Java/Scala only. 
>>>>>>> c3e867f (updates)

<img src="../../../../assets/images/concepts/fs/feature-pipelines-with-transformations.svg">

While feature encoding/scaling/imputations are also transformations that are performed on features, they are typically not implemented in feature pipelines, as they hinder feature reuse over different models and exploratory data analysis (encoded feature values are not easy to analyze). Instread, they are implemented consistentently in both [training/inference pipelines](../feature_view/training_inference_pipelines.md). 

### Aggregations
Aggregations are used to summarize large datasets into more concise, signal-rich features. Popular aggregations include count(), sum(), mean(), median(), stddev(), min(), and max(). These aggregations produce a single number (a numerical feature) that captures information about a potentially large dataset. Similarly, windowed aggregations that compute rate-based features are widely used.

### Dimensionality Reduction
If input data is impractically large or if it has a significant amount of redundancy, it can often be transformed into a reduced set of features with dimensionality reduction. Popular dimensionality algorithms include embedding algorithms, PCA, and TSNE.

### Data Validation
In order to be able to train and serve models that you can rely on, you need clean, high quality features. Data validation operations include removing bad data, removing or imputing missing values, and identifying problems such as feature shift. HSFS supports Great Expectations to specify data validation rules that are executed in the client before features are written to the Feature Store. The validation results are collected and shown in Hopsworks.

#### Precomputed vs On-Demand Features

In model inference, input features can be either computed when the prediction is requested (an *on-demand feature*) or *pre-computed* and retrieved from the feature store using an entity identifier provided as part of the request. 

For online models, when a feature can only be computed with data from the input prediction request, it is an on-demand feature, as as most online model serving environments are in Python, on-demand features are implemented in Python as Python or Pandas user-defined functions (UDFs).

A precomputed feature is computed in a batch feature pipeline or a streaming feature pipeline. An on-demand feature is used in an online inference pipeline and it can only be computed at request-time, because the data used to compute it is only available at request-time. In Hopsworks, both precomputed and on-demand features can be computed in Python user-defined functions (UDFs) or, for higher performance, Pandas UDFs. Pre-computed features can also be computed in stream processing applications and in SQL. 

### Batch Feature Pipelines: Python, PySpark, DBT/SQL

A batch feature pipelne is a program that takes raw data as input, computes features, and writes them to a feature group. In Python and PySpark, features are written as DataFrames (Pandas and PySpark DataFrames, respectively) to Hopsworks. It is also possible to compute and store offline features in external tables (e.g., stored in Snowflake, BigQuery, or Redshift) using DBT/SQL. A batch feature pipelne needs to be scheduled to run at a user-specified cadence using an orchestrator, such as Hopsworks Job Scheduler or Airflow. Feature pipelines should be provisioned or dynamically scaled so that they can handle the largest input data size during an execution run.

### Streaming Feature Pipelines: Flink, Spark-Streaming, Beam or Bytewax
‍A streaming feature pipeline is a program that continuously processes incoming data in real-time, extracting and computing features and writing those features to a feature store. Streaming feature pipelines publish very fresh pre-computed features in the feature store, enabling classes of ML systems that work best on very fresh (recently computed) feature data. Examples of features computed by streaming feature pipelines are windowed count and rate-based features. 


<img src="../../../../assets/images/concepts/fs/streaming_feature_pipeline.svg">

Hopsworks supports streaming feature pipelines in Flink, Spark Streaming, and Apache Beam/Dataflow on GCP.

#### Streaming features versus On-Demand Features

When the input data to compute a feature is only available at request-time, then you need an on-demand feature. However, may you want a feature such as how many times a user has clicked on a webpage in the last 30 seconds, then you can use a streaming feature pipeline. When the user clicks on the webpage, an event will be sent to a message bus, such as Kafka/Kineseis, and the streaming pipeline will update the feature values within seconds and write them to the online feature store. There is a time-sync to setup in your streaming feature pipeline - how often should it write to the online feature store. There will also be a ~500ms delay between writing the feature and it being available for querying from the online feature store - <a href="https://www.hopsworks.ai/post/hopsworks-online-feature-store-fast-access-to-feature-data-for-ai-applications">read here for details</a>.



### Choice of Language/Framework for Feature Engineering
You can run feature pipelines as scheduled jobs or streaming jobs in Hopsworks or you can run them as self-managed jobs in any external Python/Airflow/Spark/Flink/Beam/SQL environment.

<img src="../../../../assets/images/concepts/fs/feature-pipeline-frameworks.svg">

### Feature Engineering in Python/PySpark
Python is the most widely used framework for feature engineering due to its extensive library support for aggregations (Pandas), data validation (Great Expectations), and dimensionality reduction (embeddings, PCA), and transformations (in Scikit-Learn, TensorFlow, PyTorch). Python also supports open-source feature engineering frameworks used for automated feature engineering, such as [featuretools](https://www.featuretools.com/) that supports relational and temporal sources.

PySpark is popular as a feature engineering framework as it can scale to process larger volumes of data than Python, and provides native support for aggregations, and it supports many of the same data validation (Great Expectations), and dimensionality reduction algorithms (embeddings, PCA) as Python. Spark also has native support for transformations, which are useful for analytical models (batch scoring). 

#### Feature Functions in Python
A feature function takes as input raw data and outputs a feature. Feature functions are currently only supported in Python, and can used in Python and PySpark feature pipelines and inference pipelines. You can define a feature function as a Python UDF (user-defined function) or a Pandas UDF. In general, it is best practice to use Pandas UDFs, as they are more scalable than Python UDFs (they can run natively in PySpark feature pipelines) and they add minimal overhead in online inference pipelines.

### Feature Engineering in SQL

SQL has grown in popularity for performing heavy lifting in feature pipelines - computing aggregates on data - when the input data already resides in a data warehouse. Data warehouses also support data validation, for example, through Great Expectations in DBT. However, SQL is not mature as a platform for transformations and dimensionality reductions, where UDFs are applied row-wise.

You can do aggregation in SQL for data in your data warehouse or database.


### Feature Engineering in Flink
Apache Flink is a powerful and flexible framework for stateful feature computation operations over unbounded and bounded data streams. It is used for feature engineering when you need very fresh features computed in real-time. Flink provides a rich set of operators and functions such as time windows and aggregation operations that can be applied to keyed and/or global window streams. Flink’s stateful operations allow users to maintain and update state across multiple data records or events, which is particularly useful for feature engineering tasks such as sessionization and/or maintaining rolling aggregates over a sliding window of data.

Flink feature engineering pipelines are supported in Java/Scala only.


### Feature Engineering in Beam
Beam feature engineering pipelines are supported in Java/Scala only. 

