# Redshift Postgres RDS

Parents: [[devops]], [[aws]]
Related: snowflake 

#devops


MPP postgreSQL-based solution from Amazon (AWS). Competes with Google BigQuery and Snowflake. As of 2022 it has a bigger market share, but it is also one of the oldest, and not growing anymore. Snowflake is apparently the youngest, and growing the fastest.

Postgres seems to have some automatic gridlock resolution, but it doesn't always work.

# Practical Postgres

To avoid [[gridlock]]s, you may try someting like this (or pieces from it):

```sql
SET lock_timeout TO '300s' --5 minutes
BEGIN;
LOCK target_table IN EXCLUSIVE MODE; --As of now target_table can be read, but cannot be written to
CREATE TABLE temp_table (LIKE target_table INCLUDING ALL);
\COPY temp_table FROM 'new_data.csv' WITH CSV; --Slow operation
DROP TABLE target_table;
ALTER TABLE temp_table RENAME TO target_table;
COMMIT; --This also releases the table
DROP TABLE temp_table;
```
My understanding is that in the code above, locking the `target_table` for writes actually comes a too late, as we haven't blocked it in the time it took us to prepare `new_data.csv`, supposedly. If new data was based on the old contents of the table, it should have been locked before we read its contents, because either way it will be now overwritten and lost. But I guess it preserves at least some writes, and in some cases this manual locking may be even more useful.

Another potential optimization: instead of dropping and creating the `temp_table` just clean it at the end using `TRUNCATE temp_table`, and keep an empty table always in place.

🔥 Homework: so it is true that this code above would be faster than something like that below, if we add locks?
```sql
truncate {target_table};
insert into {target_table} select * from {temp_table};
truncate {temp_table};
```

Footnotes:
* On gridlocks: https://www.citusdata.com/blog/2018/02/22/seven-tips-for-dealing-with-postgres-locks/

`\COPY` command can do `\COPY table TO 'file_name' CSV`, or in the opposite direction, if `FROM` is used instead of `TO`.  For some reason some manuals use `WITH` keyword before `CSV`, while some don't.

🔥 What is the exact difference between `COPY` and `\COPY` again? This aws doc below calles it a "meta-command"; is this word supposed to have some precise and relevant meaning?

Footnotes:
* Doesn't use WITH: https://skyvia.com/blog/complete-guide-on-how-to-import-and-export-csv-files-to-postgresql
* Uses WITH: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/PostgreSQL.Procedural.Importing.Copy.html
* https://www.postgresql.org/docs/current/sql-copy.html

# Misc maintenance

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

# Refs

Postgres mainetnance cheatsheet (useful commands):
https://gist.github.com/rgreenjr/3637525