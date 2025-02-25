# Databricks

Parents: [[database]]
See also: [[mlflow]], [[spark]]

#db #devops


# Notebooks

Parameters: 🔥 

🔥 scheduling

# Setting compute

In terms of [[spark]], databricks really optimizes everything itself (and does it in a compute-aware way, which becomes critical if you're using the same spark pipeline on several different computes). There's lots of advice online about how to configure spark to be fast, and most of this advice should be ignored, as typically, at least for DBR notebooks, a bare-bones (practically empty) config runs exactly as fast as the best manually-configured version.