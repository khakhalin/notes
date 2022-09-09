# SQL

#tools

Parents: [[01_Tools]]
See also: [[database]]

# Generic query

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

* `DISTINCT` - return unique results only. Can go right after select: `SELECT DISTINCT`, or can be put inside count, like in `COUNT (DISTINCT col2)`.
* `LIKE` - search for a pattern in text; goes inside the condition: `WHERE col LIKE 'a%'`. Supports wildcards: `%` for any number of characters (including none), `_` for a single character; `[ab]` for either a or b (as in regular expressions), `[^ab]` for any character except a and b (not on all systems). In general, wildcards seem to differ a bit across systems, so double-check.
* Depending on the version, SQL may use either `<>` or `!=` for 'not equal'. It seems that `!=` is preferred, but `<>` is older, and so is used as a legacy operator. At least in some versions, `!<` (not less than) and `!>` also exist. They seem to compare strings, in a case-sensitive manner. If a string is compared to a number, it seems there will be an attempt to convert them for comparison. It is also possible to cast a string to a different type, or to transform it (like with `LOWER`), including changing its encoding (see below). 🧿  Note that SQL uses `=` rather than `==`.
* Precedence: arithmetic (including bitwise `~&|`) > comparisons > `NOT` > `AND` > `OR` and its friends (`LIKE`, `IN`, `BETWEEN`, `ALL`, `ANY`, `SOME`) > assignment.
* `IN` can use either a fixed list, or a subquery.
* `AVG`, `MIN`, `MAX` - other functions for grouped queries, similar to SUM. They can also go in the SELECT block (to be returned, instead of conditioned), and combined with aliasing.
* `COALESCE (col1, col2, "default value")` - returns the first non-null element from this list. Is really useful for setting default values in `SELECT`
* `CASE` - a whole case switch system for binning values on-the fly, and returning the bin name. Uses `WHEN`, `THEN`, and `ELSE` as keywords inside the CASE structure. Read the manual if needed.
* `HAVING` is used for clauses on aggregate functions, as it is performed after WHERE (and after grouping).
* `ASC` is used with `ORDER BY` by default, and so can be omitted
* 🧿  Some systems use `TOP 10` immediately after `SELECT` (SQL Server, MS Access) or `ROWNUM <= 10` inside `WHERE` (Oracle) instead of `LIMIT 10` (mySQL). Those that use TOP also support a codeword `TOP 10 PERCENT`.
* `OFFSET 1` - a rather rare thing that goes in the same  part as `LIMIT`, and makes the query return not the rows that were found, but rows that are offset from this rows by this number.
* `UNION`, `INTERSECT`, `EXCEPT` - Logical operations on selects. Usage: `SELECT * FROM table1 UNION SELECT * FROM table 2;`. The tables should match in terms of their columns, otherwise it'll break (use JOINs if the tables don't match perfectly). The first table that was called defines column names for the entire output. By default `UNION` only returns distinct rows, but use `UNION ALL` if duplicates are needed.

## Functions on data

There are lots of built-in functions; too many to list here, including math, trigonometry, string manipulation (like `LEFT`, `LEN`, `LOWER`, and so on), and what not. Some non-obvious examples:

* `LEAST` and `GREATEST` work across different columns within the same row, as opposed to MAX and MIN that work along all rows (entries) of one column (???)
* `ROUND` takes a second param for n digits, and it can be negative, so `ROUND(..., -3)` rounds to the nearest 1000, for example.
* There are also lots of system-specific operations on dates and times. See the manual: https://www.w3schools.com/sql/sql_ref_sqlserver.asp

# Joins

* `INNER JOIN` - only those records that exist in both tables. A simple `JOIN` without anything else, is an INNER JOIN by default.
* `LEFT JOIN` - all rows from the first table, and if possible - matching rows from the 2nd table. If not possible, it pads the output with nulls. Some manuals claim that one should add the word OUTER before JOIN, but it doesn't seem to be necessary.
* `RIGHT JOIN`- same as left, but reverse
* `FULL JOIN` - combines both tables (and thus acts same as UNION describe above).
* To imitate subtraction, do: `SELECT * FROM t1 LEFT JOIN t2 ON t1.key=t2.key WHERE t2.key is NULL;`. This way it will first try to LEFT JOIN, and maybe finds some matches, but for those rows of t1 that didn't get  a match in t2, it sets t2.key to NULL. And then we immediately filter these rows.
* Something like a self-join is also possible, using syntax with aliasing: `SELECT a.name AS name1, b.name AS name2 FROM table1 AS a, table1 AS b WHERE a.manager=b.id;`. In this case we'll have a list of all relations between people in an organization, all from one self-referencing table.

Some logic of joining:
* `ON` statement can technically contain any time of logic, including logical functions. So one can do something like `ON (t1.id=t2.id OR t2.name='cat')`
* For inner and left joins, `ON` and `WHERE` are equivalent, and the order in which they are written is irrelevant. It's better to think of them as being performed after joining (even though optimizer may choose to actually perform them before joining if it doesn't change the output)

# Subqueries

There are three type of subqueries:

The most common one goes inside `WHERE`: `SELECT * FROM table1 WHERE id IN (SELECT ...);`.
  * Some  useful keywords here include `ALL` and `ANY`, that are used as conditions on subqueries. (Some systems use`SOME` instead of `ANY`). For example: `WHERE num > ANY (SELECT ...)`. _It seems that it can be replaced with `MAX` or `MIN` under inner `SELECT`, no? If yest, then what's the benefit of using them? Isn't `SELECT MAX(num)` inherently easier to understand?_
  * `EXISTS`- to check whether a subquery returned anything: `WHERE EXISTS (SELECT ...)`.

`FROM` subquery: for example `SELECT * FROM (SELECT ...) AS table2`. In some dialects, this `AS` is required, as without it we will have ambiguity, as inner and outer tables have the same column names.
* In systems without windows and `OFFSET` one can use this trick to find the 2nd largest element with it, for example.
* It is also a must if you created a custom field (something like `SELECT lower(NAME) as lowly_name`) and now want to reference this new field in your `WHERE` clause. If you do it in the same clause, it won't work (will return an error), so you have to put the custom field calculation inside a sub-query, and then `SELECT *` from it.

`SELECT` subquery: one can write `SELECT a, b, SELECT(...) as c FROM ...`. This one seems to be the most esoteric, and at least some systems don't allow more than one query of this kind per entire SQL expression.

# Views

`CREATE VIEW view_name AS SELECT * FROM table1 WHERE coll="whatever";`. Can also be updated by `CREATE OR REPLACE VIEW view_name`. After you are done with this virtual table, it can be dropped using `DROP VIEW view_name`.

# Writing and messing around

## Inserting new data

`INSERT INTO table1 (col1, col2) VALUES (1, "dog");`. If you know the order of columns, you can also skip the first bracket, and just do `INSERT INTO table1 VALUES (...);`. If not all columns are specified, all remaining will be set to null.

Interesting way to copy some selected stuff from one table to another: `INSERT INTO table1 SELECT * FROM table2 WHERE condition;`. Interestingly, the condition may reference both tables, allowing for interesting data movements. If the table doesn't exist yet, use a different syntax: `SELECT * INTO table1 FROM table2 ...;`.

## Updating rows

Like a simple select query (without grouping and other fancy things obviously), just instead of SELECT you do: `UPDATE table1 SET col1=1, col2="dog" WHERE id=0;`. Be extra careful to always have a `WHERE` statement there, as without it the statement will still be valid; just it will update all records 😱. Updates can directly reference table fields, so things like `col1 = col1+1` are normal. Updates can also reference other tables by running a subquery, or doing a JOINT, or (in some versions) even by having a FROM sequence directly following the UPDATE part.

Updating some rows in one column is possible ([ref](https://stackoverflow.com/questions/36872053/update-a-single-column-on-multiple-rows-with-one-sql-query)), but it seems cumbersome enough, so in practice it's probably easier to download these rows, update them locally, then delete them remotely, and re-insert. _The only risk, I guess, is that if you crash between deletion and insertion, data will be lost..._

## Creating tables

* Create database: `CREATE DATABASE db_name;`
* Create a table: `CREATE TABLE table1 (col1 type, col2 type ...);`. There are many types, but key ones are: INT (medium, 4 bytes), BIGINT, DOUBLE, BOOL, CHAR(size) - fixed length, TEXT - variable length, LONGTEXT, ENUM(val1,val2,...) - a limited list of options, DATE, TIME. These really depend on the system though, so need to be researched.
* Columns can have **constraints** that are specified during creation, like that: `col_name data_type constraint`, where constraints come from the following list: `NOT NULL` - cannot be nulll; `UNIQUE` - all records are unique; `PRIMARY KEY` - both not null and unique (often combined with `AUTO INCREMENT`, which is not technically a constraint, but a built-in resolution of it); `FOREIGN KEY REFERENCES table2(keycol) ` - a key for a diff table; `CHECK (col_name>0)` - requires a condition to be met; `DEFAULT value` - sets a default value; `INDEX` - makes retrieval faster. If a constraint like CHECK or UNIQUE isn't met, it triggers an error, and in a real system it shoudl be handled properly (see below for control structures). Also the syntax of constraints differs a bit between systems.
* Another way to build an index: `CREATE INDEX ind ON table1 (col1);`. More than one column may be indexed, which may make reading faster, but it will also make writing slower. Ideally, indices should match selects, but maybe almost anti-match insertions, deletions, and updates.

## Changing table structure

`ALTER TABLE tb ADD col_name type;`, or `ALTER TABLE tb DROP COLUMN col_name`, or `ALTER TABLE tb MODIFY COLUMN col_name type` (some systems use ALTER COLUMN instead of MODIFY). Constraints may be removed from columns using `ALTER TABLE tb DROP CONSTRAINT constraint;` (slightly diff syntax in MySQL).

## Deleting stuff

* Delete a row: `DELETE FROM table WHERE condition;`. As usual, without WHERE would delete everything.
* Delete (drop) a whole table: `DROP TABLE table1;`. Simple and brutal.
Apparently it's possible not to grant users permissions to delete rows and drop tables (something like `REVOKE DELETE ON table TO username;` for this particular user), and it is possible to create server-wide triggers and auto-rollback on dropping; and in some editions. There may also be a way to protect tables using a schema, but I'm not sure about that.
* `BACKUP DATABASE dbname` also exists.

# Misc

* Escape sequence for apostrophe `'` is double apostrophe `''`. So it would be `O''Hara` if you wanted to search for this name.
* Some versions of SQL (Oracle?) refuse to sort by a Boolean value (some versions do type coercion, but some don't). To treat Booleans as numbers, use `CASE WHEN condition THEN 1 ELSE 0 END`. You can obviously do any other values here.
* Some versions (Oracle again?) cannot do `UNION` on sorted  queries, as they treat `UNION` as a set operation, which invalidates sorting. You can only sort things after union.

# Control structures
Some dialects of SQL (like **SQLServer**, **Transact-SQL** from Microsoft) have their own control structures, with IF, ELSE, BEGIN TRY, BEGIN CATCH, WHILE, and what not.

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