# SQL

Parents: [[01_Tools]]
See also: [[database]]

#tools #db


Dialect-specific pages:
* [[snowflake]]
* [[postgres]]

# Generic select query

```sql
SELECT col1, MAX(col2) AS altname
FROM table1 AS t1
LEFT JOIN t2 ON (t1.id=t2.id) OR (t2.col2 IS NULL)
WHERE (col1=1 AND col2>2) 
  OR NOT (col5 IN ("cat","dog")) 
  OR col8 BETWEEN 3 AND 7
GROUP BY col3
HAVING COUNT(*)>1 AND SUM(col7)<100
ORDER BY col4 DESC, col5 ASC
LIMIT 10
```

##  Bells and whistles

Clauses:
* Depending on the version, SQL may use either `<>` or `!=` for 'not equal'. It seems that `!=` is preferred, but `<>` is older, and so is used as a legacy operator. At least in some versions, `!<` (not less than) and `!>` also exist. They seem to compare strings, in a case-sensitive manner. If a string is compared to a number, it seems there will be an attempt to convert them for comparison. It is also possible to cast a string to a different type, or to transform it (like with `LOWER`), including changing its encoding (see below). 🧿  Annoyingly, SQL uses `=` rather than `==`.
* `LIKE` -  goes inside WHERE conditions, to search for a pattern in text (similar to [[regex]]): 
    * Use: `WHERE col LIKE 'a%'`. Supports wildcards: 
    * `%` - any number of characters (including none)
    * `_` -  single character
    * `[ab]` -  single character that is either a or b (as in regular expressions)
    * `[^ab]` - any single character except a and b (not on all systems).
* `BETWEEN` - a really useful one for date: `where COL between '2023-01-01' and '2023-02-01'`
* Precedence: arithmetic (including bitwise `~&|`) > comparisons > `NOT` > `AND` > `OR` and its friends (`LIKE`, `IN`, `BETWEEN`, `ALL`, `ANY`, `SOME`) > assignment. (For more info on ALL and ANY, see "Subqueries" below)
* `IN` can use either a fixed list, or a subquery.

Outputs:
* `DISTINCT` - return unique results only. Can go right after select: `SELECT DISTINCT`, or can be put inside count, like in `COUNT (DISTINCT col2)`.
* `AVG`, `MIN`, `MAX` - other functions for grouped queries, similar to SUM. They can also go in both SELECT and WHERE blocks, and are typically combined with aliasing.
* `COALESCE (col1, col2, "default value")` - returns the first non-null element from this list. Is really useful for setting default values in `SELECT`

Other:
* `HAVING` is used for clauses on aggregate functions, as it is performed after WHERE (and after grouping).
* `ASC` is used with `ORDER BY` by default, and so can be omitted
* 🧿  Some systems use `TOP 10` immediately after `SELECT` (SQL Server, MS Access) or `ROWNUM <= 10` inside `WHERE` (Oracle) instead of `LIMIT 10` (mySQL). Those that use TOP also support a codeword `TOP 10 PERCENT`.
* `OFFSET 1` - a rather rare thing that goes in the same  part as `LIMIT`, and makes the query return not the rows that were found, but rows that are offset from this rows by this number. For most systems, see "Window functions" below.
* `UNION`, `INTERSECT`, `EXCEPT` - Logical operations on selects. Usage: `SELECT * FROM table1 UNION SELECT * FROM table 2;`. The tables should match in terms of their columns, otherwise it'll break (use JOINs if the tables don't match perfectly). The first table that was called defines column names for the entire output. By default `UNION` only returns distinct rows, but use `UNION ALL` if duplicates are needed.

## Expression-columns

There are lots of built-in functions; too many to list here, including math, trigonometry, string manipulation (like `LEFT`, `LEN`, `LOWER`, and so on), and what not. But in call cases you do something like `SELECT FUN(col1) AS new_col`.

**Case switch** is another way to create new columns on-the-fly. Typically goes inside the `SELECT` section, in which case you write `AS` right after `END` to name the column, and it looks a bit agrammatical:
```sql
select COL1, COL2,
    case
        when COL3 > 0 then 'Positive'
        when COL3 = 0 then 'Zero'
        else 'Haha now this is strange'
    end as COL3_AS_TEXT
```

Some non-trivial functions:
* `LEAST` and `GREATEST` work across different columns within the same row, as opposed to MAX and MIN that compare rows within a column.
    * The only problem with `least(COL1, COL2)` is that for rows where one of the columns has a NULL in it, `least` will return NULL. If you want NULLs to be ignored (maybe, ideally, returned only for those rows where _all_ columns contain a NULL), you have several options (all of them annoying):
        1. `case when COL1<COL2 then COL1 when COL2<COL1 then COL2 else NULL end` - Hard-code a system of `case` explicitly comparing columns.  A comparison with nulls doesn't return true, so this should work (?🔥 will it?). But obviously it will get increasingly messy as the number of columns increases.
        2. `least(coalesce(COL1, 999), coalesce(COL2, 999))` - A large magic number solution + coalesce wrapper. This will obviously fail if all columns are NULL
        3. `least(coalesce(COL1, COL2), coalesce(COL2, COL1))` - works exactly as intended, but is hard to generalize to more columns, as one needs to consider all possible wrap-arounds of this sequence (every column should be mentioned )
        4. Do a `union` of these columns into one "virtual column", followed by grouping by some row id, and `min`.
        5. Do a bunch of `where ... is null` and then union the results.
* `ROUND` takes a second param for n digits, and it can be negative, so `ROUND(..., -3)` rounds to the nearest 1000, for example.
* There are also lots of system-specific operations on dates and times. See the manual: https://www.w3schools.com/sql/sql_ref_sqlserver.asp

Footnotes:
* https://modern-sql.com/concept/null
* https://stackoverflow.com/questions/29549036/sql-select-the-minimum-value-from-multiple-columns-with-null-values
* https://stackoverflow.com/questions/21313770/least-value-but-not-null-in-oracle-sql

# Subqueries

There are three type of subqueries:

The most common one goes inside `WHERE`, to compare records of this table with some info from some other table:
`select * from TABLE1 where COL in (select VAL from TABLE2 where ...)`.
  * Some  useful keywords here include `ALL` and `ANY`, to use as conditions on subqueries. (Some systems use`SOME` instead of `ANY`). For example: `WHERE num > ANY (SELECT ...)`. (Although in this example specifically, you could as well just do `max()` on the subquery, and then compare once with this max value)
  * `EXISTS`- to check whether a subquery returned anything: `WHERE EXISTS (SELECT ...)`.

Another common use pattern is to do a 2-stages-select, by putting a subquery into `FROM`: 
`SELECT * FROM (SELECT ...) AS table2`. In some dialects, this `AS` is required, as without it we will have ambiguity, as inner and outer tables have the same column names.
* In systems without windows and `OFFSET` one can use this trick to find the 2nd largest element with it, for example.
* It is also a must if you created a custom field (something like `SELECT lower(NAME) as lowly_name`) and now want to reference this new field in your `WHERE` clause. If you do it in the same clause, it won't work (will return an error), so you have to put the custom field calculation inside a sub-query, and then `SELECT *` from it.

Finally, you can place a subquery into `SELECT`, treating it as a type of a very complicated formula: 
`select COL1, select(...) as COL2 from ...`. Some systems only allow one query of this kind per entire SQL expression however, so maybe it's not the most common one.

# Joins

* `INNER JOIN` - only those records that exist in both tables. A simple `JOIN` without anything else, is an INNER JOIN by default.
* `LEFT JOIN` - all rows from the first table, and if possible - matching rows from the 2nd table. If not possible, it pads the output with nulls. Some manuals claim that one should add the word OUTER before JOIN, but it doesn't seem to be necessary.
* `RIGHT JOIN`- same as left, but reverse
* `FULL JOIN` - combines both tables (and thus acts same as UNION describe above).
* To imitate subtraction, do: `SELECT * FROM t1 LEFT JOIN t2 ON t1.key=t2.key WHERE t2.key is NULL;`. This way it will first try to LEFT JOIN, and maybe finds some matches, but for those rows of t1 that didn't get  a match in t2, it sets t2.key to NULL. And then we immediately filter these rows.
* Self-joins are also possible, using syntax with aliasing: `SELECT a.name AS name1, b.name AS name2 FROM table1 AS a, table1 AS b WHERE a.manager=b.id;`. In this case we'll have a list of all managerial relations between people in an organization, all from one self-referencing table.

Misc wisdom:
* `ON` statement can technically contain any type of logic, including logical functions. So one can do something like `ON (t1.id=t2.id OR t2.name='cat')`
* For inner and left joins, `ON` and `WHERE` are equivalent, and the order in which they are written is irrelevant. It's better to think of them as being performed after joining (even though optimizer will mostly likely choose to actually perform them before joining if it doesn't change the output)

**Lateral Joints** (analogous to `merge_asof` in [[pandas]]):
* 🔥 for now I couldn't make it work

Alternatives to Lateral Joints:
A common pattern for lateral joints is to take a timestamped tableA, and then for each row in this tableA, find the next row from a timestamped tableB. While also joining on some field or fields. For example, for each person, find consecutive pairs of events A and B. This can be imitated with window functions.
* One roundabout way is to do two selects (from both tables), cast them into compatible table structures, mark data source for each of them with a constant column, union these two tables, sort by "merge columns" followed by a timestamp, use a `lead` window function to identify rows that go in a correct sequence (have matching "merge columns", and then data sources A and B followint each other), do `where` on this column to only keep matching pairs, use `lead` to manually "pivot" both sources into one row (pulling source B into new columns of rows coming from cource A), and finally ditch rows from source B (using another `where`).
* A simpler solution is to inner join all later rows from tableB to tableA, then sort them, enumerate them, and only keep the 1st row from every list (this requares a subquery):
```sql
select * from (
    select KEY_A, KEY_B, ta.PERSON as PERSON
    rownumber() over (partition by KEY_A order by TIMESTAMP) as ROWNUM
    from TABLE_A as ta
    inner join TABLE_B as tb
        on ta.PERSON = tb.PERSON
        and tb.TIMESTAMP > ta.TIMESTAMP
    )
whre ROWNUM=1
```

# Writing and changing data

## Insert data

`INSERT INTO table1 (col1, col2) VALUES (1, "dog");`. If you know the order of columns, you can also skip the first bracket, and just do `INSERT INTO table1 VALUES (...);`. If not all columns are specified, all remaining will be set to null.

Interesting way to copy some selected stuff from one table to another: `INSERT INTO table1 SELECT * FROM table2 WHERE condition;`. Interestingly, the condition may reference both tables, allowing for interesting data movements. If the table doesn't exist yet, use a different syntax: `SELECT * INTO table1 FROM table2 ...;`.

## Update rows

Like a simple select query (without grouping and other fancy things obviously), just instead of SELECT you do: `UPDATE table1 SET col1=1, col2="dog" WHERE id=0;`. Be extra careful to always have a `WHERE` statement there, as without it the statement will still be valid; just it will update all records 😱. Updates can directly reference table fields, so things like `col1 = col1+1` are normal. Updates can also reference other tables by running a subquery, or doing a JOINT, or (in some versions) even by having a FROM sequence directly following the UPDATE part.

Updating some rows in one column is possible ([ref](https://stackoverflow.com/questions/36872053/update-a-single-column-on-multiple-rows-with-one-sql-query)), but it seems cumbersome enough, so in practice it's probably easier to download these rows, update them locally, then within one query, delete them remotely, and re-insert.

## Create tables

* Create database: `CREATE DATABASE db_name;`
* Create a table: `CREATE TABLE table1 (col1 type, col2 type ...);`. There are many types, but key ones are: INT (medium, 4 bytes), BIGINT, DOUBLE, BOOL, CHAR(size) - fixed length, TEXT - variable length, LONGTEXT, ENUM(val1,val2,...) - a limited list of options, DATE, TIME. These really depend on the system though, so need to be researched.
* Columns can have **constraints** that are specified during creation, like that: `col_name data_type constraint`, where constraints come from the following list: `NOT NULL` - cannot be nulll; `UNIQUE` - all records are unique; `PRIMARY KEY` - both not null and unique (often combined with `AUTO INCREMENT`, which is not technically a constraint, but a built-in resolution of it); `FOREIGN KEY REFERENCES table2(keycol) ` - a key for a diff table; `CHECK (col_name>0)` - requires a condition to be met; `DEFAULT value` - sets a default value; `INDEX` - makes retrieval faster. If a constraint like CHECK or UNIQUE isn't met, it triggers an error, and in a real system it shoudl be handled properly (see below for control structures). Also the syntax of constraints differs a bit between systems.
* Another way to build an index: `CREATE INDEX ind ON table1 (col1);`. More than one column may be indexed, which may make reading faster, but it will also make writing slower. Ideally, indices should match selects, but maybe almost anti-match insertions, deletions, and updates.

## Change table structure

`ALTER TABLE tb ADD col_name type;`, or `ALTER TABLE tb DROP COLUMN col_name`, or `ALTER TABLE tb MODIFY COLUMN col_name type` (some systems use ALTER COLUMN instead of MODIFY). Constraints may be removed from columns using `ALTER TABLE tb DROP CONSTRAINT constraint;` (slightly diff syntax in MySQL).

## Delete stuff

* Delete a row: `DELETE FROM table WHERE condition;`. As usual, without WHERE would delete everything.
* Delete (drop) a whole table: `DROP TABLE table1;`. Simple and brutal.
Apparently it's possible not to grant users permissions to delete rows and drop tables (something like `REVOKE DELETE ON table TO username;` for this particular user), and it is possible to create server-wide triggers and auto-rollback on dropping; and in some editions. There may also be a way to protect tables using a schema, but I'm not sure about that.
* `BACKUP DATABASE dbname` also exists.

# Window Functions

```sql
select 
    COL, 
    sum(COL) over (order by DATETIME) as COL_CUMSUM,
    sum(COL) over (partition by GROUP) as COL_GROUPED, -- same result as in reverse-joined group by
    sum(COL) over (partition by GROUP order by DATETIME) as COL_CUMSUM_PER_GROUP --resets at each GROUP
from TABLE where ...
```

Possible aggregations:
* `sum, avg, count` - intuitive
* `row_number()` - naive rank, resetting at each paritition. Gets no column inside, as it is ordering / partition that serves as an argument
* `dense_rank()` - same, but identical values get identical ranks
* `rank()` - same, but also after a draw, some ranks are skipped
* `ntile(100)` - percentile and alike; the number inside is the number of buckets to apply to ordering.
* `lag(COL, n_steps, default_value)` - previous row (or rather, the one `n_steps` before), in this order
* `lead(COL, n_steps, default_value)` - next row (or `n_steps` after)

When using the same window for many calculations, create an alias for this window:
```sql
select
    sum(COL) over my_window as COL,
    ntile(5) over my_window as NTILE
from TABLE where ...
window my_window as (partition by GROUP order by COL)
order by GROUP, COL -- without this, values will be correct, but not properly ordered
```

Footnotes:
* https://mode.com/sql-tutorial/sql-window-functions/

# Views

`CREATE VIEW view_name AS SELECT * FROM table1 WHERE coll="whatever";`. Can also be updated by `CREATE OR REPLACE VIEW view_name`. After you are done with this virtual table, it can be dropped using `DROP VIEW view_name`.

For security-related views (policy views), see [[snowflake]]

# Misc

* Escape sequence for apostrophe `'` is double apostrophe `''`. So it would be `O''Hara` if you wanted to search for this name.
* Some versions of SQL (Oracle?) refuse to sort by a Boolean value (some versions do type coercion, but some don't). To treat Booleans as numbers, use `CASE WHEN condition THEN 1 ELSE 0 END`. You can obviously do any other values here.
* Some versions (Oracle again?) cannot do `UNION` on sorted  queries, as they treat `UNION` as a set operation, which invalidates sorting. You can only sort things after union.

# Control structures

Many dialects of SQL (like **SQLServer**, **Transact-SQL** from Microsoft) have their own control structures, with IF, ELSE, BEGIN TRY, BEGIN CATCH, WHILE, and what not. (see individual dialect pages)

# Refs

Intros
* Main reference / tutorial: https://www.w3schools.com/sql/default.asp
* Basic intro tutorial with some nice quizzes: https://sqlzoo.net/
* Confetti also has a whole section on SQL: https://www.confetti.ai/curriculum
* https://sqlbolt.com/

Exercises and practice questions
* https://datalemur.com/ - people seem to like it
* https://www.stratascratch.com/ - also found via recommendations
* PostgreSQL Exercises: https://pgexercises.com/
* https://advancedsqlpuzzles.com/
* https://www.hackerrank.com/