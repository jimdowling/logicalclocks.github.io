When building a ML System, you typically have to develop 3 different **ML pipelines** (connected together by a feature store and a model registry):

 * __feature pipelines__ *transform raw data into features that are stored in feature groups*. Examples of transformations include aggregations, binning, and dimensionality reduction. The output are features (and labels/targets) that are stored in a *feature group*.
 * __training pipelines__ *train a model using features/labels and stores the model in a model registry*. The training data is created using a *feature view* (a selection of features from potentially many different feature groups). Model training requires a ML framework, such as Scikit-Learn, TensorFlow, or PyTorch.
 * __inference pipelines__ *takes input features and a trained model and produces predictions that are consumed by a ML-enabled system or application*. An inference pipeline will download a trained model from the model registry, precomputed feature data from the feature store, and call predict on the model using the input feature data. Inference pipelines can be batch programs, stream processing applications, or online (part of a model deployment behind a network endpoint that produces predictions in response to prediction requests).

<img src="../../../../assets/images/concepts/fs/ml-pipelines-ml-system.svg">

In the above figure, we can see the feature pipeline(s), training pipeline(s), and (batch/streaming/online) inference pipeline(s), and how they read and write to the feature store and the model registry. Hopsworks provides high availability for both its feature store and model registry (if you have an existing model registry, you can use that instead).
