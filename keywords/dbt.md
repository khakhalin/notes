# DBT = Data Build Tool

Parents: [[devops]]
See also: [[dwh]], [[airflow]]

#devops #etl


A tool to transform data, roughly equivalent to oldschool ETL jobs, but better (at least for simpler transformations that don't include fancy modeling / ML). Loks mostly SQL-like, with extra features and operations, but also supports python blocks with [[pandas]]-like dataframes (but not actually pandas: at least when working on [[snowflake]], it's some snowflake-native thing, called a snowpack dataframe 🔥?). It's also possible to use actual pandas, but it would be way slower.

dbt jobs can be run by Apache [[airflow]]

# Refs

https://en.wikipedia.org/wiki/Data_build_tool

https://docs.getdbt.com/docs/building-a-dbt-project/building-models

See also:

https://en.wikipedia.org/wiki/Apache_Airflow