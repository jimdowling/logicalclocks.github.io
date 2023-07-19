# ML Pipelines and the Feature Store

Here are [some examples in Hopsworks of complete ML Systems](https://github.com/logicalclocks/hopsworks-tutorials/tree/master/), written in Python, with feature/training/inference pipelines:

* [loan approval](https://github.com/logicalclocks/hopsworks-tutorials/tree/master/loan_approval) - online application to predict whether a loan should be approved or not
* credit card fraud prediction ([batch](https://github.com/logicalclocks/hopsworks-tutorials/tree/master/fraud_batch) and [online](https://github.com/logicalclocks/hopsworks-tutorials/tree/master/fraud_online)) - predict whether to approve a credit card transaction or not
* [customer churn](https://github.com/logicalclocks/hopsworks-tutorials/tree/master/churn) - batch application to predict if customers will churn or not
* [nyc taxi fares](https://github.com/logicalclocks/hopsworks-tutorials/tree/master/advanced_tutorials/nyc_taxi_fares) - batch application to predict the fare amount for a taxi ride in New York City 

Many of the above examples have a user interface with the  inference pipeline (Streamlit or Gradio UI frameworks).

Some examples of feature pipelines with integrations with external systems:

* [DBT/SQL](https://github.com/logicalclocks/hopsworks-tutorials/tree/master/integrations/dbt_bq) - create features on GCP with Big Query (BQ) for External Feature Groups (offline store is BQ)
* [Snowflake](https://github.com/logicalclocks/hopsworks-tutorials/tree/master/integrations/snowflake) - create features in Python with Snowflake as an External Feature Group 
* [Redshift](https://github.com/logicalclocks/hopsworks-tutorials/tree/master/integrations/redshift) - create features in Python with Redshift as an External Feature Group
* [Weights & Biases (W&B)](https://github.com/logicalclocks/hopsworks-tutorials/tree/master/integrations/wandb) - training pipelines in W&B using Hopsworks Feature Store

