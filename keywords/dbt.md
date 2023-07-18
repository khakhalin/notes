# DBT = Data Build Tool

Parents: [[devops]]

#devops #stub


A tool to transform data. Mostly looks sql-like (with extra stuff), but now also supports python blocks that get a [[pandas]]-like dataframe (but is not actually pandas, but just something vaguely similar, yet [[snowflake]]-native, called a snowpack (🔥?) dataframe), and returns a dataframe. It's also possible to use pandas, but it's way slower.

dbt jobs can be run by Apache Airflow

# Refs

https://en.wikipedia.org/wiki/Data_build_tool

https://docs.getdbt.com/docs/building-a-dbt-project/building-models

See also:

https://en.wikipedia.org/wiki/Apache_Airflow