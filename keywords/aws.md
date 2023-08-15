# AWS

Parents: [[system_design]], [[data_wh]]
See also: [[databricks]], [[snowflake]]

#devops


Subtopics:
* [[postgres]] - technically, not uniquely related to AWS, but in practice often used on AWS Redshift RDS
* [[kubernetes]] - what we usually do on AWS

# AWS session

Logging in:
* Using okta verification: `aws-okta select master-$DOMAIN-$ENV-$ROLE` 
* Using Boost: `boost auth --domain $DOMAIN --stage $ENV --role $ROLE`

Here `$ENV` is probably `prod` etc.

Note that unlike for [[docker]], you are not "transported" to a different virtual environment; you are still you your original machine, but running [[kubernetes]] commands for a different machine.

Not sure how to "officially" close a session.


# Maintenance of PostgreSQL RDS

To find all queries running on RDS that mention a certain table, or a certain dangerous (slow) command, login as root (a separate user-login pair that is used only for this purpose, essentially), then run this sql:
```sql
SELECT * FROM pg_stat_activity where query like '%target_keyword%' ORDER BY backend_start
```
Then you can kill the unwanted (blocking) query using this:
`SELECT pg_terminate_backend(<pid>)`, where `pid` is the id from the `pid` column in the table above.

A version of the same query with a more readable output:
```sql
SELECT datname, pid, usename, backend_start, wait_event_type, wait_event, state, query
FROM pg_stat_activity where query like '%target_keyword%' ORDER BY backend_start
```

Footnotes:
* https://aws.amazon.com/premiumsupport/knowledge-center/rds-postgresql-running-queries/

