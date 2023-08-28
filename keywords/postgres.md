# Redshift Postgres RDS

Parents: [[devops]], [[database]], [[aws]], [[sql]]
See also: [[snowflake]] 

#devops #db


MPP postgreSQL-based solution from Amazon (AWS). Competes with Google BigQuery and Snowflake. As of 2022 it has a bigger market share, but it is also one of the oldest, and not growing anymore. Snowflake is apparently the youngest, and growing the fastest.

Postgres seems to have some automatic gridlock (aka "deadlock"?) resolution, but it doesn't always work (or maybe timeouts are typically set too long?). But in principle, it shoudl identify pairs of gridlocked transactions and eventually cancel one of them (see [[aws]] for some useful tricks)

# Concurrency control

**Concurrency** is achieved via **snapshots**: each transaction runs against a "frozen" (snapshotted) database; it cannot see the results of anything that runs together with it (except itself). Which means that while rows are changed, both old and new rows co-exist, but are visible to different transactions. Then old rows are eventually removed. This is called `VACUUM` and amounts to removing rows that are no longer visible to anyone. This also explains why long transactions are bad even if they are read-only: while a long transaction is running, newly written rows accumulate in memory, and all rows are not deleted (as they were snapshotted by the reader), so memory fills up.

To make a bunch of changes and then commit them at once, use a block that starts with a `BEGIN` command, and ends with either `COMMIT` or `ROLLBACK` command. In terms of **writing**, a `BEGIN-COMMIT` block acts kinda as a single transactions: changed rows are not committed until the very end of a block.

In terms of **reading**, by default, each transaction with the block takes its own snapshot (aka **read committed** mode), and so consecutive transactions may see slightly different versions of the DB if writing is happening concurrently. To make the entire block work on the same "forked" version of the DB, we need to use a **serializable** mode, like that:
```sql
BEGIN;
SET TRANSACTION ISOLATION LEVEL SERIALIZABLE;
...
COMMIT;
```
It is also possible to set it for all blocks within a session using `SET DEFAULT TRANSACTION ISOLATION TO SERIALIZABLE;`. Overall, it is a good idea to isolate in serializable mode if several tables are read in a sequence, and they need to be consistent with each other.

Writing in a table doesn't lock the entire table (by default), but rather locks individual rows. As long as two writers write to different rows, they can write concurrently. Writers and readers also never block each other. If two writers write to the same row, the second writer waits; then, if they run in `READ COMMITTED` mode, it uses the new version of the row for writing into it (and actually, it first rechecks if it still satisfies the `WHERE` clause, coz maybe in the meanwhile it stopped being applicable!). In `SERIALIZABLE` mode, it fails (aborts) with "Can't serialize" error (similar to a merge conflict, except there's no one there to resolve it). Which shows the strengths and weaknesses of two modes.
* "Read-committed" concurrent writers never fail; they just gracefully get into a queue to wait for each other, and eventually update everything. However they all see slightly different versions of the table, so there is no consistency in what they see. 
* "Serializable" writers work on the same snapshot, so you can for example first read something, then update it, and write back. But as a downside, this operation would fail if somebody happens to write into these rows while you calculated the update. Which means that this mode requires some extra coding, to handle unsuccessful transactions.

To make serializable mode more resilient, on top of working with a snapshot of a table as an input, we can **lock a set of rows** within a `BEGIN-COMMIT` block, using `SELECT * FOR UPDATE` in the first transaction after `BEGIN`. This is better than locking the entire table, but more pessimistic than updating in serializable mode and just retrying until it succeeds. In this case, different parts of the table may still be updated concurrently. But at the same time, we can never believe any global properties of a table (say, some `COUNT` of rows), as we'll be calculating it based on a snapshot, not on the actual state of the table (once we commit our locked rows).

It seems that essentially we have a ladder of fixing the environment:
* Just a bunch of commands - fix input states within each transaction; commit results after each transaction
* `BEGIN-COMMIT` block + `READ COMMIT` mode - fix input state only within each transaction; wait for rows to free if they are being read to; commit everything at once at the end
* `SERIALIZABLE` - fix input state for a bunch of transactions; work on this forked version of the table; commit all at once; fail if "merge conflict"
* `SERIALIZABLE` + `SELECT FOR UPDATE` - fix input state, but also block some rows from the table until commit
* `LOCK` - lock the entire table.

Note that locking in the middle of a block in serializable mode is weird and kinda unpreditable. Either lock at the beginning (then it doesn't matter what the mode was), or lock in the middle of a "read commit" mode (then at least you know that the table was internally consistent when you locked it). But better to always lock in the beginning.

Footnotes:
* https://www.postgresql.org/files/developer/concurrency.pdf - very nice and rich lecture slides on concurrency in PostgreSQL
* https://www.postgresql.org/docs/current/sql-begin.html

# Connection leak

Connection leak is a situation when apps open connections, but never close them, letting them time-out. As every server has an upper limit on the number of connections it can have open concurrently (typically in the order of a few hundreds), with a fast-running leaking service, it's possible to throttle the server, leading to global outage of all services relying on this server.

When using [[python]], a good way to avoid connection leak is to always use `with` to create a temp connection object, and then work with this object (calling `pandas.read_sql` on it etc), as `with` will cleanup after it is executed, and for a connection object cleanup includes deleting the cursor and closing the connection.

```python
with psycopg2.connect(CONNECTION_STRING) as conn:
    cursor = conn.cursor()
    cursor.execute(sql_query + ';')
    conn.commit()
```



# Practical Combos

## Replacing an entire column

We may try something like that:
```sql
ALTER TABLE t DROP column s, ADD column s int;
```

## Swapping an entire table

To avoid [[gridlock]]s, you may try someting like this (or pieces from it):

```sql
SET lock_timeout TO '10s' -- Actual time should be about 1 s for a 10 000 - rows multicol table
BEGIN;
LOCK target_table IN EXCLUSIVE MODE; --As of now target_table can be read, but cannot be written to
CREATE TABLE temp_table (LIKE target_table INCLUDING ALL);
\COPY temp_table FROM 'new_data.csv' WITH CSV; --Slow operation
DROP TABLE target_table;
ALTER TABLE temp_table RENAME TO target_table;
COMMIT; --This also releases the table
DROP TABLE temp_table;
```
> `\COPY` is a real command, the backslash is not a typo. 🔥🔥🔥 — What is the exact difference between `COPY` and `\COPY` again? This aws doc below calles it a "meta-command"; is this word supposed to have some precise and relevant meaning?

My understanding is that in the code above, locking the `target_table` for writes actually comes too late, as we haven't blocked it in the time it took us to prepare `new_data.csv`, supposedly. If new data was based on the old contents of the table, it should have been locked before we read its contents, because either way it will be now overwritten and lost. But I guess it preserves at least some writes, and in some cases this manual locking may be even more useful. (🔥 would be nice to get a review on it)

Another potential optimization: instead of dropping and creating the `temp_table` just clean it at the end using `TRUNCATE temp_table`, and keep an empty table always in place.

And then one can **catch timeouts** in python, and postpone microservice resurrection by prolonging the life of this instance (the scheduler typically won't create new instances if the old one is still running):
```python
try:
    postgresql("SET lock_timeout TO '5s'; ...")
    _log.degub("Success")
except psycopg2.OperationalError as error: # This exception is thrown if a query times-out
    _log.warning("Failure; see error message: " + str(error))
    # Any clean-up if needed
    time.sleep(60) # Let the script wait for a few minutes
```

Footnotes:
* On gridlocks: https://www.citusdata.com/blog/2018/02/22/seven-tips-for-dealing-with-postgres-locks/

## Uploading data

`\COPY` command can do `\COPY table TO 'file_name' CSV`, or in the opposite direction, if `FROM` is used instead of `TO`.  For some reason some manuals use `WITH` keyword before `CSV`, while some don't.

Footnotes:
* Doesn't use WITH: https://skyvia.com/blog/complete-guide-on-how-to-import-and-export-csv-files-to-postgresql
* Uses WITH: https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/PostgreSQL.Procedural.Importing.Copy.html
* https://www.postgresql.org/docs/current/sql-copy.html

# Control structures

```sql
IF ... THEN ... END IF;
IF ... THEN ... ELSE ... END IF;
ELSEIF -- instead of the first ELSE (works just as one would expect)

CASE <what> 
        WHEN <value1>, <value2> THEN -- Where of course "value1" can be a complex expression 
            <statements> 
        WHEN <value3> THEN
            <statements>
        ELSE
            <statements>
END CASE;
```

For example, if you want to only run something when a table exists:
```sql
IF
    EXISTS (SELECT 1 FROM information_schema.tables WHERE table_name='table_name')
THEN    
    ...
END IF;
```

Footnotes:
* A summary: https://www.w3resource.com/PostgreSQL/pl-pgsql-control-structures.php
* https://stackoverflow.com/questions/50678559/execute-select-statement-if-table-exists

# Refs

Postgres mainetnance cheatsheet (useful commands):
https://gist.github.com/rgreenjr/3637525

A table of all command-command conflicts that cause a lock on postgres: https://pglocks.org