# Data Lake

Parents: [[database]], [[system_design]]
See also: [[data_wh]], [[lakehouse]]

#db #systems


Central location for raw data. Flat, not hierarchical. Timestamped, versioned, never overwritten (exception: GDPR compliance). ID + tags for simplified extraction. Data may be partially structured (by schemas?) but also quite desparate between schemas, or even completely unstructured.

Storage is typically quite cheap. Reading is typically rather expensive, because of low indexing, high compute.

Performance is usually slow. For any analytics one should build a [[data_wh]] drawing from a datalake, with a flow of data validation, cleaning, and aggregation. 

An interesting consideration for privacy and GDPR: if most users have access to the Data Warehouse (DWH), and not to the datalake (in the extreme case, only ETL jobs have access to the datalake), one can perform most of the masking at the interface between the datalake and the DWH. If datalake is rutinely used for data retrieval however, masking has to happen before data is stored in the lake (which in a way violates the very idea of datalake storing raw data).

# Products

* Cloud proprietary:
    * Amazon S3 (aka Simple Storage Service)
    * Azure ADLS
    * Google Cloud Storage
* Open-Source:
    * Hadoop Distributed File System (HDFS) in combo with Apache Hadoop
    * Apache Hive

# Refs

