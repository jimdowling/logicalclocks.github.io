ML Systems built on a feature store decompose the problem of transforming input data into models and predictions into 3 different **ML pipelines**:

 * __feature pipelines__ *transform raw data into features that are stored in feature groups*. Examples of transformations include aggregations, binning, and dimensionality reduction. The output are features (and labels/targets) that are stored in feature groups.
 * __training pipelines__ *train a model using features/labels and stores the model in a model registry*. The training data is created using feature view (a selection of features from potentially many different feature groups). Model training requires a ML framework, such as Scikit-Learn, TensorFlow, or PyTorch.
 * __inference pipelines__ *takes input features and a trained model and produces predictions that are consumed by a ML-enabled system or application*. An inference pipeline will download a trained model, and call predict on the model using the input feature data. Inference pipelines can be batch programs, stream processing applications, or part of a model deployment behind a network endpoint that produces predictions in response to prediction requests.

<img src="../../../../assets/images/concepts/fs/ml-pipelines-ml-system.svg">

In the above figure, we can see the feature pipeline(s), training pipeline(s), and (batch/streaming/online) inference pipeline(s), and how they read and write to the feature store and the model registry. Hopsworks provides high availability for both its feature store and model registry, but if you have an existing model registry, you can use that instead.

### ML Pipeline Project Layout

In Hopsworks, ML Pipelines are implemented with source code in programming languages such as Python, SQL, or Java. Feature pipelines, training pipelines, and inference pipelines can all be implemented using different languages/frameworks, enabling you to choose the best language/framework for the ML pipeline at hand. For example, you might use DBT/SQL for a simple feature pipeline that processes large volumes of data in the Data Warehouse. Or you may have more complex feature logic that requires a feature pipeline implemented in PySpark. Training pipelines are most often implemented in Python, while batch inference pipelines are often implemented in Python when data volumes are low and PySpark for larger data volumes. Each ML pipeline can be independently developed, deployed, and orchestrated.

#### Source Code Layout for ML Pipelines 
ML Pipelines can be developed as standalone artifacts using separate source code repositories. Sometimes, however, it is benefical to put all source code for the ML pipelines for a system in the same monolithic source-code repository, a so-called mono-repo. For example, if your feature pipeline is in PySpark, but training and inference pipelines are in Python, and you have some on-demand features, then the same feature functions in Python can be reused in feature pipelines in PySpark and online inference pipelines in Python.

#### Mono-Repo Project Layout

For Python/PySpark projects, a typical mono-repo layout, shown below, is the easiest way to organize your source code into directories containing

* feature functions - functions that compute features that may be reused in different ML pipelines
* ML pipelines - feature, training, and inference pipelines 
* tests - unit tests for feature functions, and integration tests for ML pipelines

<img src="../../../../assets/images/concepts/fs/project_layout.svg">

The mono-repo layout makes it easier to reuse on-demand feature functions between feature pipelines and inference pipelines, preventing <a href="https://www.hopsworks.ai/mlops-dictionary/online-offline-skew">offline-online skew</a>. For an example of such a project with a mono-repo layout, see this <a href="https://github.com/logicalclocks/hopsworks-tutorials/tree/dev/loan_approval">loan approval ML system</a>.  

Other languages/frameworks, such as DBT/SQL for feature pipelines and Airflow DAGs for orchestrating ML pipelines, can also be included in your mono-repo, if needed.

#### Reusable Feature Functions Project Layout

If you are reusing feature functions in Python over many different ML systems, you may want to manage then in their own repository, with a versioned pip installable module that can them be imported into your different feature pipelines and inference pipelines.

<img src="../../../../assets/images/concepts/fs/feature_library_layout.svg">

Another advantage of this approach is that it makes it easier to produce containerized ML pipeline artifacts.