A feature function takes as input raw data and outputs a feature.

Feature functions are currently only supported in Python, and can used in Python and PySpark feature pipelines and inference pipelines. You can define a feature function as a Python UDF (user-defined function) or a Pandas UDF. In general, it is best practice to use Pandas UDFs, as they are more scalable than Python UDFs (they can run natively in PySpark feature pipelines) and they add minimal overhead in online inference pipelines.
