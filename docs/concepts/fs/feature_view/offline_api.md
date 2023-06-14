The feature view provides a *Batch API* to create point-in-time consistent snapshots of:

 * training data
 * batch inference data

You can apply filters when retrieving a batch of feature data. This is useful if you want to train many versions of the same model that only different by some attribute, such as one model per customer, a different model for each geographic region, or a different model depending on the type of device used by a user.

<img src="../../../../assets/images/concepts/fs/batch-scoring-data.svg">

## Training Data

You create training data with a feature view. Training data can be either:

 - in-memory Pandas DataFrames, useful when you have a small amount of training data;
 - materialized in files, in a file format of your choice (such as .tfrecord, .csv, or .parquet).

### Train-test(-validation) split

You can specify how to split your training data into the train-set and test-set (and optionally validation-set), when reading training data:

 - train/test random split (e.g., fv.train_test_split(test=0.2) returns 80% in the train-set, 20% in the test-set)
 - train/test/validation random split (e.g., fv.train_validation_test_split(test=0.15, validation=0.15) returns 70% in the train-set, 15% in the validation-set, and 15% in the test-set)
 - time-series split (e.g., define start-time and end-time for the train-set, test-set, and validation-set, respectively);

#### Evaluation sets for model validation
Test data can be further split into evaluation sets using filters to help evaluate a model for potential bias. First, you have to identify the classes of samples that could be at risk of bias, and generate *evaluation sets* from your unseen test set - one evaluation set for each group of samples at risk of bias. For example, if you have a feature group of users, where one of the features is gender, and you want to evaluate the risk of bias due to gender, you can use filters to generate 3 evaluation sets from your test set - one for male, female, and non-binary. Then you score your model against all 3 evaluation sets to ensure that the prediction performance is comparable and non-biased across all 3 gender.



### Spine Dataframes

The left side of the point-in-time join is typically the set of training entities/primary key values for which the relevant features need to be retrieved. This left side of the join can also be replaced by a [spine group](../feature_group/spine_group.md).
When using feature groups also so save labels/prediction targets, it can happen that you end up with the same entity multiple times in the training dataset depending on the cadence at which the label group was updated and the length of the event time interval
that is being used to generate the training dataset. This can lead to bias in the training dataset and should be avoided. To avoid this kind of situation, users can either narrow down the event time interval during training dataset creation or use a spine
in order to precisely define the entities to be included in the training dataset. This is just one example where spines are helpful.


## Batch Inference Data

Batch data for inference (scoring models) is created using a feature view. Similar to training data, you can create batch data as either:

 - in-memory Pandas DataFrames, useful when you have a small amount of inference data;
 - materialized data in files, in a file format of your choice (such as .tfrecord, .csv, or .parquet)

The API support two general approaches to retrieving batch inference data:

 - a time-range for inference data (specify `start_time` and `end_time` (default is the now) for the data you want to score with your model)
 - provide a set of IDs and timestamps for the feature data you want to retrieve using a Spine DataFrame (e.g., get the latest feature data for all users, providing the IDs for all users in the Spine DataFrame)


### Spine DataFrames

Similar to training dataset generation, it might be helpful to specify a spine when retrieving features for batch inference. The only difference in this case is that the spine dataframe doesn't
need to contain the label, as this will be the output of the inference pipeline.
A typical use case is the handling of opt-ins, where certain customers have to be excluded from an inference pipeline due to a missing marketing opt-in.

## Point-in-time Correct Snapshots of Feature Data

When you create training data from features in different feature groups, it is possible that the feature groups are updated at different cadences. For example, maybe one feature group is updated hourly, while another feature group is updated daily. It is very complex to write code that joins features together from such feature groups and ensures there is no data leakage in the resultant training data. HSFS hides this complexity by performing the point-in-time JOIN transparently, similar to the illustration below:


<img src="../../../../assets/images/concepts/fs/feature-view-training-data.svg">

HSFS uses the event_time columns on both feature groups to determine the most recent (but not newer) feature values that are joined together with the feature values from the feature group containing the label. That is, the features in the feature group containing the label are the observation times for the features in the resulting training data, and we want feature values from the other feature groups that have the most recent timestamps, but not newer than the timestamp in the label-containing feature group.

## Filters
You can apply filters when getting a batch of data using a feature view. For example,

 - create training data only for customers from a particular country
 - get a batch of inference data for users who registered in the last 24 hours using a particular type of device

Note that filters are not applied when retrieving feature vectors using feature views, as we only look up features for a specific entity, like a customer. In this case, the application should know that predictions for this customer should be made on the model trained on customers in USA, for example.

