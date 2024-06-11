# Medallion architecture

Parents: [[data_wh]], [[lakehouse]]
See also: [[datalake]], [[databricks]]

#db


Allegedly, an alternative to Data Warehouses (DWH, [[data_wh]]), coined and promoted by [[databricks]]. Three layers: bronse (raw data), silver, and gold. As data propagates through them, some [[data_cleaning]] and validation takes place.

* **Bronse layer**: "as-is" data, versioned (historicized)
* **Silver layer**: data is validated, cleanes, merged, joined, and enriched. Most business applications feed from this layer. Typically, instead of ETLs (with large loads), it uses ELTs (Extract-Load-Transform; see below).
* **Gold layer**: project- and analytics-oriented, highly aggregated and transformed, Kimball-style start-schema architecture (see [[data_wh]]).

Now, **ELT** (as opposed to ETL) is about first copying the data, then improving it:
* Ideally, you want to be listening to a queue, but if it's impossible - you periodically request recently changed lines.
* Then you write to your structured DB whatever fields that you can.
* And only then, for these half-ready records, you perform cleansing, filtering, joining. In most cases, doing it "on the fly", as you propagate the data from one layer to another.

> Personal opinion: it's not really an alternative to DataLake+DWH, but a different approach to data cleansing and transformation (very small loads). It also makes the first layer (bronse) more expensive (compared to datalake), as we sift through it more often. And we pay more for storage, as all data is duplicated. But most importaintly, the difference is not _that_ great to give it a separate name, I think?

# Refs

https://www.databricks.com/glossary/data-lakehouse
https://www.databricks.com/glossary/medallion-architecture

https://learn.microsoft.com/en-us/azure/databricks/lakehouse/medallion

https://dataengineering.wiki/Concepts/Medallion+Architecture

https://www.altexsoft.com/blog/elt-extract-load-transform/