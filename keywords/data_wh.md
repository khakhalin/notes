# Data warehouse (aka Kimball vs Inmon)

Parents: [[01_Tools]], [[database]]
Children: [[medallion]], [[lakehouse]]
See also: [[nosql]]

#tools #db


**Data Warehouse** is an approach to storing data in an organized manner, balancing different conflicting goals and interests that every person and organization has, when working with data. Ultimately, about preserving records fully, but also making them available for quering and analysis.

Typically has relational DBs in the core, but potentially other stuff around it (non-relational, files, etc.) Typically relies on several "types" (logical caregories) of tables, and **ETL jobs** (Extract-Transform-Load) to move data across them, for analysis and reporting capabilities. Typically goes away from raw data, and towards certain semi-processed "themes", half-way towards future analyses (raw data → staging area → conversion into a usable structure → storage). 

* There's an argument for having a DWH **Non-volatile**, that is, a system where old data is at minimum never removed, or in the extreme case, not even altered once it was added. For example, a good implementation of [[gdpr]] involves not removing, but "blanking out" personal data, and then partially decoupling deleted customers, but not removing hashed IDs themselves, as doing so would invalidate other tables.
* On the other hand, there is a good argument for having the data amended (say, when an invoice was cancelled or adjusted some weeks after a business event).

In practice, a good DWH exists somewhere between these two extremes, maintaining a balance between structure consistency and data validity. But unlike [[datalake]]

**A Data WH typically consists of:**
1. Relational **Databases**:
2. **ETL jobs**: extracts info from sources, validates and cleans it (see [[data_cleaning]]), combines and integrates data from different sources, potentially pre-processes it (to make it more usable in the future), and loads it somewhere. A good example would be [[dbt]] jobs maintained by [[airflow]]
3. **Metadata**, or **Data Dictionary**, describing formats, origins, relationships, and usage of data (see below)
4. **Access tools**: quering, reporting, data mining, OLAP (Online analytical processing)
5. **Bus**, aka data mart - a way to partition data (_is it a scaling tool?_)

**Data Dictionary** typically describes:
* Field names, types, sizes
* ID structure
* Restrictions on values (can be used for validation; see [[data_cleaning]])
    * Ranges
    * Sets
    * Formats
* Default values
* Cross-references

A typical layered structure of databases in a DWH:
1. BIZ layer (internal)
    1. DIM tables - dimensions, tables that contan primary keys and that are massively referenced everywhere via secondary keys, such as `cust_sk`, `prod_sk`, `locat_sk` etc. The table has a suffix `_dim`. May not be versioned (🔥 ? is it true)
    2. FACT tables - tables with transactions. They also have keys of course, but these keys are often not referenced, aside for a case when some sort of transactions need to be linked and chained to each other. The table has a suffix `_fct`.
3. PUSR (Power User) layer - summarizes everything, but with full data, very limited access to be [[gdpr]]-compliant
4. REP - outward-facing layer, typically consists of views (see [[sql]]), and not actual tables, where filtering by access rights would happen. Personal data is typically also stripped out at this point.

When working with data science pipelines, the derivative temporary databases that are calculated based on these primary databases, are sometimes called **views** and get a suffix `_v`.

## Inmon model

Named after Bill Inmon. In this approach, databases are **normalized**, which means that main tables of the database store highly optimized keys, and push actual values to a bunch of peripheral tables; then join on retrieval. This minimizes data redundancy, as nothing is stored twice: helps with repeated values, and makes everything leaner and more structured. The central table stores **atomic data**.

This type of a Data WH is easier to understand, backup, maintain (except that you need a rather technical specialist to maintain it), and update. Analyses and reporting are infinitely flexible (data mine anything you want). But it also makes everything slower (because of joins that a necessary for any query); it's harder to develop; and  transformations (ETL) are needed at the analysis stage (between databases and users).

## Kimball model

Aka **Kimball model**. Introduced by Ralph Kimball in 1996 (by publishing a book named The Data Warehouse Toolkit). Aka **Dimensional Modeling Technique**. An approach to corporate data that is an opposite of Inmon (above). In this approach, data is first transformed (summarized) towards easily interpretable and business-relevant aspects (called **Dimensions**), and only then stored. It makes storage trickier (as data is duplicated, yet should match), but makes reporting and interpretation easier, as data is already stored in user-facing formats. The data is **denormalized**.

The way data is organized is called a **Star schema** (the name comes from the way it was usually drawn on a white board, with the central DB in the middle and all sorts of "habitual reports" around it). In this approach, before storage, raw data is first transformed into so-called **facts** (data tables that represent same underlying data in different ways):
* **transaction lists***
* **snapshots** (like, inventory snapshots, balances, etc)
* **accumulating tables** (total change since last snapshot).

All fact tables (facts) should use the same system of dimensions (so user id, region id, etc., should match across all of them). On top of these fact tables, we also have **dimension tables** - essentially like pivot tables in Excel, across different dimensions, such as clients, employees, regions, etc.

> It seems that, very roughly, in Inmon system you clean the data and store it, then transform it when you need a report. In Kimball approach, you clean the data, transform it for expected "standard" reporting, then store it.

The benefits of Kimball approach (compared to Inmon) is that it's simpler, cheaper computationally (no SQL joins), more relateable for users (if well aligned with business processes), most commonly used data takes less space (an inventory snapshot is much smaller than a raw inventory movement listing), has most relevant data easily available. However, the spirit of "one source of data" is lost (as we now have several fact tables that could in principle contradict each other); it may not be very flexible (hard to change the structure of fact tables later on); does not easily support data mining; does not easily support integration of legacy data.

Potential solution: [[nosql]]

# Refs

Wiki:
* https://en.wikipedia.org/wiki/Data_warehouse
* https://en.wikipedia.org/wiki/Dimensional_modeling
* https://en.wikipedia.org/wiki/Data_dictionary
* https://en.wikipedia.org/wiki/Data_mart - something like a smaller / older version of DataWH?
* https://en.wikipedia.org/wiki/Star_schema - a simplest kind of Data mart?

Kimball vs Inmon:
* https://tdan.com/data-warehouse-design-inmon-versus-kimball/20300
* https://www.computerweekly.com/tip/Inmon-or-Kimball-Which-approach-is-suitable-for-your-data-warehouse

Summary pdf for Kimball approach:
http://www.kimballgroup.com/wp-content/uploads/2013/08/2013.09-Kimball-Dimensional-Modeling-Techniques11.pdf

Inmon approach:
* https://en.wikipedia.org/wiki/Database_normalization