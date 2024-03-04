The term *ML pipelines* is often used to describe all types of programs that contribute to operating a ML system. 
In Hopsworks, a ML pipeline is an abstract term to describe the set of the three different types of pipelines used to build a ML system: feature pipelines, training pipelines, and inference pipelines, see figure below.

<img src="../../../../assets/images/concepts/fs/ml-pipelines-sets.svg">


When building a ML System, you typically have to develop 3 different **ML pipelines** (connected together by a feature store and a model registry):

 * __feature pipelines__ *transform raw data into features that are stored in feature groups*. Examples of transformations include aggregations, binning, and dimensionality reduction. The output are features (and labels/targets) that are stored in a *feature group*. Feature pipelines can be batch programs, run on a schedule, or streaming programs that run 24x7.
 * __training pipelines__ *train a model using features/labels and stores the model in a model registry*. The training data is created using a *feature view* (a selection of features from potentially many different feature groups). Model training requires a ML framework, such as Scikit-Learn, TensorFlow, or PyTorch.
 * __inference pipelines__ *takes input features and a trained model and produces predictions that are consumed by a ML-enabled system or application*. An inference pipeline will download a trained model from the model registry, precomputed feature data from the feature store, and call predict on the model using the input feature data. Inference pipelines can be batch programs, stream processing applications, or online request/response services (part of a model deployment behind a network endpoint that produces predictions in response to prediction requests).

<img src="../../../../assets/images/concepts/fs/ml-pipelines-ml-system.svg">

In the above figure, we can see the feature pipeline(s), training pipeline(s), and inference pipeline(s), and how they read and write to the feature store and the model registry.