# Lakehouse

Parent: [[data_wh]]
See also: [[medallion]], [[databricks]], [[datalake]], [[kafka]]

#db


Apparently, an alternative to [[datalake]] and data warehouses (DWH, [[data_wh]]) ptomoted by [[databricks]]. What the claim is that with caching in-memory, working with datalake may be cheap and fast enough to eliminate the need in fancy ETLs.

From the technical pov, a heavy reliance on [[parquet]] files for some reason.

The ads for Lakehouse claim that ETLs and DWHs make data stale. Intead, at leats [[databricks]] promote a certain [[medallion]] architecture, where data is also progressively improved, but apparently not consolidated?

# Refs

https://www.databricks.com/glossary/data-lakehouse

https://www.databricks.com/discover/data-lakes
Even tho the link says "datalakes", actually all they do is say bad things about [[datalake]]s and [[data_wh]]s, and claim that Lakehouses are the only heroes of this story.  But really, mostly fighting strawmen here. Like, why DWH has to be proprietary? Why?? Who said that?