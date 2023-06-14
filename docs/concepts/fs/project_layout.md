## ML Project Layout

We have preferred conventions for creating ML projects with Hopsworks that make it easier to navigate, modularize, and maintain the source code for ML Systems developed in Python (Pandas, PySpark, etc).

In this example layout below, we organize your code into directories containing

* feature functions - functions compute features and are in a Python module with the same name as the feature group they belong to
* pipelines - feature, training, and inference pipelines that transform raw data into features, features/labels into models, and features/models into predctions, respectively.
* tests - unit tests for feature functions, and integration tests for ML pipelines

<img src="../../../assets/images/concepts/fs/project_layout.svg">


The above image is a mono-repo (monolithic repository) where all the code for your ML pipelines is contained in a single project. This makes it easier to collaborate across the different ML pipeline stages and makes it easier to ensure that, in online inference pipelines, the same versions of feature functions are used as in the feature pipelines.

Here is an example <a href="https://github.com/logicalclocks/hopsworks-tutorials/tree/dev/loan_approval">loan approval ML project</a> that follows the above mono-repo project layout.  

<img src="../../../assets/images/concepts/fs/feature_library_layout.svg">

You can also have a more modulare project layout, where you place your feature, training, and inference pipelines in separate projects, and your feature functions in their own project. The advantage of this approach is that you can minimize the code size in your online inference pipelines, as you only need to include the feature functions, and not all the feature and training pipeline source code.
