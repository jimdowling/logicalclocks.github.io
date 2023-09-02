Hopsworks provides a complete self-service development environment for feature engineering and model training. 

<!-- You can develop programs as Jupyter notebooks or jobs, you can manage the Python libraries in a project using its conda environment, you can manage your source code with Git, and you can orchestrate jobs with Airflow. -->
<!-- <img src="../../../assets/images/concepts/dev/dev-inside.svg"> -->

### Source Code Control

Hopsworks provides source code control support using Git (GitHub, GitLab or BitBucket). You can securely checkout code into your project and commit and push updates to your code to your source code repository. 

### Conda Environment per Project

Hopsworks supports the self-service installation of Python libraries using PyPi, Conda, Wheel files, or GitHub URLs. The Python libraries are installed in a Conda environment linked with your project. Each project has a base Docker image and its own conda environment. Jobs and Jupyter notebooks are run as Docker images. The Docker images are compiled transparently for you when you update your Conda environment. There is no need to write a Dockerfile, users install Python libraries in the UI or with the API. You can setup custom development and production environments by creating new projects, each with their own conda environment and Docker base image.

### Jupyter Notebooks

Hopsworks provides a Jupyter notebook development environment. You can also develop in your IDE (PyCharm, IntelliJ, etc), test locally, and then run your programs locally or as Jobs in Hopsworks. Jupyter notebooks can also be run as Jobs in Hopsworks.

### Jobs

In Hopsworks, a Job is a schedulable program that is allocated compute and memory resources. You can run a Job in Hopsworks:

* from the UI;
* programmatically with the Hopsworks SDK (Python, Java) or REST API;
* from Airflow with the Hopsworks Airflow operator;
* from your IDE using a plugin ([PyCharm/IntelliJ plugin](https://plugins.jetbrains.com/plugin/15537-hopsworks));

<!-- ### Orchestration

Airflow comes out-of-the box with Hopsworks, but you can also use an external Airflow cluster (with the Hopsworks Job operator) if you have one. Airflow can be used to schedule the execution of Jobs, individually or as part of Airflow DAGs. -->