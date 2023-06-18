ML Systems built on a feature store decompose the problem of transforming input data into models and predictions into 3 different ML pipelines

<img src="../../../../assets/images/concepts/fs/ml-pipelines-outputs.svg">


 - __feature pipelines__ transform raw data into features using aggregations, binning, dimensionality reduction, and so on. The output are features (and labels/targets) that are stored in feature groups.
 - **training pipelines** trains a model using an ML framework and training data retrieved using feature view that creates a point-in-time correct snapshot of the features/labels. A training pipeline produces as output a model that is stored in a model registry.
 - __inference pipelines__ receive/create features, download a trained model, and call predict on the model using the feature data. Inference pipelines produce predictions as output that are consumed by an AI-enabled application or user.

<img src="../../../../assets/images/concepts/fs/ml-pipelines-ml-system.svg">

In the above figure, we can see the feature pipeline(s), training pipeline(s), and batch/online inference pipeline(s), and how they read and write to the feature store and the model registry. Hopsworks provides high availability for both its feature store and model registry, but if you have an existing model registry, you can use that instead.