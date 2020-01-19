# SQL
#tools

**Some refs:**
* [Good reference](https://www.w3schools.com/sql/default.asp)

## Generic query
```sql
SELECT col1, MAX(col2) AS colname2
FROM table1 AS t1
WHERE (col1=1 AND col2>2) OR NOT (col5 IN ("cat","dog")) OR (col8 BETWEEN 3 AND 7)
GROUP BY col3
HAVING COUNT(*)>1 or SUM(col7)>0
ORDER BY col4 DESC, col5 ASC
LIMIT 10
FROM table2 AS t2 JOIN *
ON t1.id=t2.id
WHERE t2.col2 IS NULL;
```

##  More bells and whistles
* `DISTINCT` - return unique results only. Can go right after select: `SELECT DISTINCT`, or can be put inside count, like in `COUNT (DISTINCT col2)`.
* `LIKE` - search for a pattern in text; goes inside the condition: `WHERE col LIKE 'a%'`. Supports wildcards: `%` for any number of characters (including none), `_` for a single character; `[ab]` for either a or b (as in regular expressions), `[^ab]` for any character except a and b (not on all systems). In general, wildcards seem to differ a bit across systems.
* Depending on the version, SQL may use either `<>` or `!=` for 'not equal'. It seems that `!=` is actually preferred, but `<>` is older, and so is used as a legacy operator. At least in some versions, `!<` (not less than) and `!>` also exist. They seem to compare strings, in a case-sensitive maner, and if string would meet a number, it seems there will be an attempt to convert them for comparison. It is also possible to cast a string to a different type, or to transform it, including changing its encoding (see below).
* Precedence: arithmetics (including bitwise `~&|`) > comparisons > `NOT` > `AND` > `OR` and its friends (`LIKE`, `IN`, `BETWEEN`, `ALL`, `ANY`, `SOME`) > assignment.
* `IN` can use either a fixed list, or a subquery.
* `AVG`, `MIN`, `MAX` - other functions for grouped queries, similar to SUM. They can also go in the SELECT block (to be returned, instead of conditioned), and combined with aliasing.
* `COALESCE (col1, col2, "default value")` - returns the first non-null element from this list
* `CASE` - a whole case switch system for binning values on-the fly, and returning the bin name (uses WHEN, THEN, and ELSE as other keywords inside the CASE structure. Read the manual if needed.)
* `HAVING` is used for clauses on aggregate functions, as it is performed after WHERE (and after grouping).
* `ASC` is used with `ORDER BY` by default, and so can be omitted
* Some systems use `TOP 10` (SQL Server, MS Access) or `ROWNUM <= 10` (Oracle) instead of `LIMIT 10` (mySQL). Those that use TOP also support a codeword `TOP 10 PERCENT`.
* `OFFSET 1` - a rather rare thing that goes in the same part as LIMIT, and makes the query return not the rows that were found, but rows that are offset from this rows by this number.
* `UNION`, `INTERSECT`, `EXCEPT` - Logical operations on selects. Usage: `SELECT * FROM table1 UNION SELECT * FROM table 2;`. The tables should match in terms of their columns, otherwise it'll break (use JOINs if the tables don't match perfectly). The first table that was called defines column names for the entire output. By default UNION only returns dinstinct rows, but use `UNION ALL` if duplicates are needed.

### Joins
* `INNER JOIN` - only those records that exist in both tables. A simple `JOIN` without anything else, is an INNER JOIN by default.
* `LEFT JOIN` - all rows from the first table, and if possible - matching rows from the 2nd table. If not possible, it pads the output with nulls. Some manuals claim that one should add the word OUTER before JOIN, but it doesn't seem to be necessary.
* `RIGHT JOIN`- same as left, but reverse
* `FULL JOIN` - combines both tables (and thus acts same as UNION describe above).
* To imitate subtraction, do: `SELECT * FROM t1 LEFT JOIN t2 ON t1.key=t2.key WHERE t2.key is NULL;`. This way it will first try to JOIN, but set t2.key to null for all missing elements, and then they'll immediately be filtered.
* Something like a self-join is also possible, using syntax with aliasing: `SELECT a.name AS name1, b.name AS name2 FROM table1 AS a, table1 AS b WHERE a.manager=b.id;`. In this case we'll have a list of all relations between people in an organization, all from one self-referencing table.

### Subquery
`SELECT * FROM table1 WHERE id IN (SELECT ...);`.
* `ALL`, `ANY` - used within where or having, as conditions on subqueries. So you can do `WHERE col1 = ANY (SELECT ...)`.
* `SOME` - exact synonim to `ANY` used in some versions.
* `EXISTS`- to check whether a subquery returned anything: `WHERE EXISTS (SELECT ...)`.

### Virtual table
`CREATE VIEW view_name AS SELECT * FROM table1 WHERE coll="whatever";`. Can also be updated by `CREATE OR REPLACE VIEW view_name`. After you are done with this virtual table, it should be dropped using `DROP VIEW view_name`.

## Insert data
`INSERT INTO table1 (col1, col2) VALUES (1, "dog");`. If you know the order of columns, you can also skip the first brackeet, and just do `INSERT INTO table1 VALUES (...);`. If not all columns are specified, all remaining will be set to null.

Interesting way to copy some selected stuff from one table to another: `INSERT INTO table1 SELECT * FROM table2 WHERE condition;`. Interestingly, the condition may reference both tables, allowing for interesting data movements. If the table doesn't exist yet, use a different syntax: `SELECT * INTO table1 FROM table2 ...;`.

## Update rows
Like a simple select query (without grouping and other fancy things obviously), just instead of SELECT you do: `UPDATE table1 SET col1=1, col2="dog" WHERE id=0;`. Be careful to always have a `WHERE` statement there, as without it the statement will still be valid; just it will update all records. Updates can directly reference table fields, so things like `col1 = col1+1` are normal. Updates can also reference other tables by running a subquery, or doing a JOINT, or (in some versions) even by having a FROM sequence directly following the UPDATE part.

## Create stuff
* Create database: `CREATE DATABASE dbname;`
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

## Functions on data
There are lots of built-in functions; too many to mention, including math, trigonometry, string manipulation (like `LEFT`, `LEN`, `LOWER`, and alike), and what not. Some interesting ones that are not obvious are `LEAST` and `GRATEST` that work across differenct columns within the same row, as opposed to MAX and MIN that work along all rows (entries) for a returned column. There are also lots of **system-specific operaions on dates and times**. Read the manual.

## Control structures
Apparently SQL (especially larger systems, like **SQLServer**, or **Transact-SQL** from Microsoft) also has its own system of control structures, with IF, ELSE, BEGIN TRY, BEGIN CATCH, WHILE, and what not.