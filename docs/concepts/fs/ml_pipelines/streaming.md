
### Flink, Spark-Streaming, or Beam

<img src="../../../../assets/images/concepts/fs/streaming_feature_pipeline.svg">


### Time-Window Aggregations

The most common types of features computed by streaming feature pipelines are windowed count and rate-based features. 

### Feature Freshness

There is a time-sync to setup in your stream-processing engine - how often should it write to the feature store.
There will also be a ~500ms delay between writing the feature and it being available for querying from the feature store - <a href="https://www.hopsworks.ai/post/hopsworks-online-feature-store-fast-access-to-feature-data-for-ai-applications">read here for details</a>.