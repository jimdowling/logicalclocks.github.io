

A feature pipeline is a program that orchestrates the execution of a dataflow graph of feature functions (transformations on input data to create feature data). The computed features are saved to one or more feature groups. A feature pipeline can also validate the feature data using Great Expectations before it is saved to a feature group.

### Data Sources

Feature pipelines read their input data from data sources such as data warehouses, message buses, databases, object stores, and Http APIs. 


#### Precomputed vs On-Demand Features

In model inference, features are the inputs to models that, in turn, return predictions. The input features can be either computed when the prediction is requested (an *on-demand feature*) or *pre-computed* in a feature pipeline, stored in the feature store, and then retrieved from the feature store when needed to make one or more predicitons.

A precomputed feature is computed in a batch feature pipeline or a streaming feature pipeline. An on-demand feature is used in an online inference pipeline and it can only be computed at request-time, because the data used to compute it is only available at request-time. In Hopsworks, both precomputed and on-demand features can be computed in Python user-defined functions (UDFs) or, for higher performance, Pandas UDFs. Pre-computed features can also be computed in stream processing applications and in SQL. 

### Batch Feature Pipelines: Python, PySpark, DBT/SQL

A batch feature pipelne is a program that takes raw data as input, computes features, and writes them to a feature group. In Python and PySpark, features are written as DataFrames (Pandas and PySpark DataFrames, respectively) to Hopsworks. A batch feature pipelne needs to be scheduled to run at a user-specified cadence using an orchestrator, such as Airflow. Feature pipelines should be provisioned or dynamically scaled so that they can handle the largest input data size during an execution run.

### Streaming Feature Pipelines: Flink, Spark-Streaming, or Beam
‍A streaming feature pipeline is a program that continuously processes incoming data in real-time, extracting and computing features and writing those features to a feature store. Streaming feature pipelines publish very fresh pre-computed features in the feature store, enabling classes of ML systems that work best on very fresh (recently computed) feature data. Examples of features computed by streaming feature pipelines are windowed count and rate-based features. 


<img src="../../../../assets/images/concepts/fs/streaming_feature_pipeline.svg">

Hopsworks supports streaming feature pipelines in Flink, Spark Streaming, and Apache Beam/Dataflow on GCP.

#### Streaming features versus On-Demand Features

When the input data to compute a feature is only available at request-time, then you need an on-demand feature. However, may you want a feature such as how many times a user has clicked on a webpage in the last 30 seconds, then you can use a streaming feature pipeline. When the user clicks on the webpage, an event will be sent to a message bus, such as Kafka/Kineseis, and the streaming pipeline will update the feature values within seconds and write them to the online feature store. There is a time-sync to setup in your streaming feature pipeline - how often should it write to the online feature store. There will also be a ~500ms delay between writing the feature and it being available for querying from the online feature store - <a href="https://www.hopsworks.ai/post/hopsworks-online-feature-store-fast-access-to-feature-data-for-ai-applications">read here for details</a>.