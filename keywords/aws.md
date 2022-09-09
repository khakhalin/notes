# AWS

Parents: [[system_design]], [[data_wh]]
See also: , databricks, snowflake

#devops

Subtopics:
* [[postgres]] - technically, not uniquely related to AWS, but in practice often used on AWS Redshift RDS


# Maintenance of PostgreSQL RDS

To find all queries running on RDS that mention a certain table, or a certain dangerous (slow) command, login as root (a separate user-login pair that is used only for this purpose, essentially), then run this sql:
```sql
SELECT * FROM pg_stat_activity where query like '%target_keyword%' ORDER BY backend_start
```
Then you can kill the unwanted (blocking) query using this:
`SELECT pg_germinate_backend(<pid>)`, where `pid` is the id from the `pid` column in the table above.

A version of the same query with a more readable output:
```sql
SELECT datname, pid, usename, backend_start, wait_event_type, wait_event, state, query
FROM pg_stat_activity where query like '%target_keyword%' ORDER BY backend_start
```

Footnotes:
* https://aws.amazon.com/premiumsupport/knowledge-center/rds-postgresql-running-queries/

