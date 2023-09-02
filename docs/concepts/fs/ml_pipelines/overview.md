When building a ML System, you typically have to develop 3 different **ML pipelines**:

 * __feature pipelines__ *transform raw data into features that are stored in feature groups*. Examples of transformations include aggregations, binning, and dimensionality reduction. The output features are stored in a *feature group*.
 * __training pipelines__ *train a model using features/labels and stores the model in a model registry*. The training data is created using a *feature view* (a selection of features from feature groups). Model training requires a ML framework, such as Scikit-Learn, TensorFlow, or PyTorch.
 * __inference pipelines__ *takes input features and a trained model and produces predictions that are consumed by a ML-enabled system or application*. An inference pipeline makes predictions by downloading a trained model, loading precomputed features from the feature store, and calling predict on the model using the input features. Inference pipelines can be batch, stream processing, or online applications.
<img src="../../../../assets/images/concepts/fs/ml-pipelines-ml-system.svg">

In the above figure, we can see the feature pipeline(s), training pipeline(s), and (batch/streaming/online) inference pipeline(s), and how they read and write to the feature store and the model registry.