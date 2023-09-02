A training pipeline is a program (typically written in Python) that takes as input features (and labels if it is supervised ML), trains a model using a ML framework and algorithm, and outputs a trained model that is saved to model registry. 

<img src="../../../../assets/images/concepts/fs/training-pipeline.svg">

Some of the steps involved in training a model include the:

* selection of the features and the range of data to be used to train the model, 
* splitting the training data into train/test/validation sets,
* encoding/scaling feature data before it is fed into the model for training,
* selection of a model architecture (tree-based, feedforward DNN, transformer, etc)
* discovery of good hyperparameters for the combination of prediction problem, training data, and model architecture,
* fitting the training data to the model (i.e., model training),
* model evaluation - validation/testing of the model's performance and checks for any model bias,
* registration of the trained model with a model registry.

Using a feature store in the training pipeline helps to achieve consistency across different training runs and ensures that the features used for training are of high quality and reproducible.

### Feature Selection and Feature Views

<img src="../../../../assets/images/concepts/fs/feature-view-join-tables.svg">

When you want to train a model, you should select the features (from the different feature groups) that will be used by your model (the same features are used in training and inference), along with a label/target if you will train your model with supervised ML.

<img src="../../../../assets/images/concepts/fs/feature-selection.svg"  width="600" height="500">

You start by selecting features from different feature groups and join them together to create a feature view. The feature view is the set of input features and label for your model, along with optional filters for feature values and transformations. You can also include extra helper columns in a feature view that are not features used by the model (such as the primary key, the event_time, and columns used by inference pipelines when storing predictions in external systems) - but be sure to drop those columns when creating training data and inference data using the feature view.


### Feature Encoding/Scaling
If you have categorical features, you may need to encode them (e.g., using one-hot encoding) before they are input to the model. Similarly, if you are using gradient-based ML algorithms, you may have to scale your numerical features to improve model performance. You can use ML-framework specific pipelines (such as Sk-Learn pipelines, or preprocessing pipelines for TensorFlow or Keras) to perform feature scaling/encoding, but you need to ensure the pipeline objects (that contain both these feature transformation and the model, itself) are saved to the model registry - not just the models. That is because, you need to make sure the same transformations (encoding/scaling) are performed in your inference pipeline. An alternative is to use declarative transformations in a feature view. In declarative transformations, you assign encoding/scaling transformations to features when you define your feature view, and when you reading training or inference data, the transformations are applied transparently in the feature store client library.
Declarative transformations in feature views


### Splitting Training Data
You can create training data as Pandas DataFrames or files using a feature view. You can also retrieve training data as features and labels, split into train/test/validation sets using the feature store APIs. Currently, random-splits and time-series splits are supported.
