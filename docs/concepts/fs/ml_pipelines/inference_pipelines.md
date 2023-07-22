An inference pipeline is a program that takes input features and a trained model and produces predictions that are consumed by some ML-enabled system or application. The trained model is downloaded (and cached) from the model registry. Inference pipelines can be batch programs, stream processing applications, or online (part of a model deployment behind a network endpoint that produces predictions in response to prediction requests). The features can come from different sources: precomputed from the feature store, and for online models, they can also be computed from input request data and/or external APIs. 

‍An inference pipeline is a program that takes input data, optionally transforms that data, then makes predictions on that input data using a model. Inference pipelines can be either batch programs or online services. In general, if you need to apply a trained machine learning model to new data, you will need some type of inference pipeline.

An inference pipeline typically involves first an initialization step that includes downloading the trained model from a model registry, loading any necessary dependencies (such as importing libraries), and establishing a connection to a feature store. The inference step involves setting up the input data pipeline (retrieve precomputed features from the feature store, encode/scale features, compute on-demand features), making predictions on the input data using the model, and post-processing the predictions - sending predictions to a consumer that will use the prediction(s) to make intelligent deicisions.

### Batch Inference Pipeline
‍Batch inference pipelines enable scheduled, scalable inference on large volumes of data using a trained model. Batch inference pipelines are run on a schedule (e.g., weekly, daily or hourly) and are used to drive dashboards and operational ML systems (the predictions are stored in a database and subsequently read by the operational system - think "Spotify Weekly - a recommended list of songs generated once per week").
<img src="../../../../assets/images/concepts/fs/batch-inference-pipeline.svg">

### Online Inference Pipeline
For online predictions, an online application sends a prediction request to a model deployment that contains an *online inference pipeline* that processes the input request, computes any on-demand features, retrieves any precomputed features from a feature store, applies any feature encoding/scaling/imputations  before making a prediction on the model with the final feature vector. Often, predictions are post-processesed before the prediction is sent back to the client.

<img src="../../../../assets/images/concepts/fs/online-inference-pipeline.svg">

### Streaming Inference Pipeline
‍A streaming inference pipeline is a streaming application (such as Flink, Spark Streaming, Beam, or Bytewax) that makes real-time, non-interactive predictions triggered by the arrival of an event and outputs predictions to some sink (such as the event bus).

<img src="../../../../assets/images/concepts/fs/streaming-inference-pipeline.svg">
