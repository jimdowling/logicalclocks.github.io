Hopsworks has conventions for the source code layout of Python-based ML projects (Pandas, PySpark, Polars) that make it easier to navigate, modularize, and maintain your ML systems.

<img src="../../../../assets/images/concepts/fs/feature_library_layout.svg">

You can have a modular source-code layout, where you place your feature, training, and inference pipelines in separate projects, and your feature functions in their own project. The advantage of this approach is that you can minimize the code size in your online inference pipelines, as you only need to include the feature functions, and not all the feature and training pipeline source code.


Another possible source-code layout, shown below, organizes your source code into directories containing

* feature functions - functions compute features and are in a Python module with the same name as the feature group they belong to
* pipelines - feature, training, and inference pipelines that transform raw data into features, features/labels into models, and features/models into predctions, respectively.
* tests - unit tests for feature functions, and integration tests for ML pipelines

<img src="../../../../assets/images/concepts/fs/project_layout.svg">

The above image is what is known as a mono-repo (a monolithic source-code repository) where all the code for your ML pipelines (feature, training, inference) is contained in a single project. This makes it easier to collaborate across the different ML pipeline stages and makes it easier to prevent <a href="https://www.hopsworks.ai/mlops-dictionary/online-offline-skew">offline-online skew</a>. For an example of such a mono-repo layout, see this <a href="https://github.com/logicalclocks/hopsworks-tutorials/tree/dev/loan_approval">loan approval ML system</a>.  

