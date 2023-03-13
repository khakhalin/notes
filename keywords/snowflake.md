# Snowflake

Parents: [[01_Tools]], [[database]], [[sql]]
See also: [[postgres]]

#db


Some notes on the Snowflake dialect of [[sql]]

# Virtual tables

No need for `HAVING` and subqueries: just defihe a bunch of virtual tables, and cross-reference them as you please. One code word `WITH` to start the sequence, then a bunch of definitions separated by commas, and then finall - the main query:

```sql
with
table1 as (select * ...),
table2 as (select * ...)

select * from table1
left join table2 on COL
```

# Ternary operator

For a ternary (or any sort of conditional) operator at `select` stage, instead of a basic `select COL` write this thing below. In many cases you can fit it in one row of course. Both values can of coure be constants. It's possible to add more `when .. then ..` clauses. And `as` can be omitted (but I find it more readable with it).

```sql
case 
    when RULE1 = 0 and RULE2 = 1 then COL1
    else COL2
end
as NEWCOL
```

# Listing elements

Three important functionalities here:
* `listagg()` acts as a grouping function (same status as `min()` or `count()`), stacking lots of strings into one long string-row
* `||` concatenates strings
* `::VARCHAR` casts a non-string variable into a string

```sql
select
    listagg('Interval ' || START_DATE::VARCHAR || ' - ' || END_DATE::VARCHAR, ',') as NEWCOL
from TABLE
group by GROUPCOL
```

# Over → Qualify

To leave only one row for every combination of target columns, you can of course do nested subquery, then in the outer part of your query group by these columns and do `FIRST`, but you can also do this:
```sql
select * from ___
row_number() over (
    partition by COLUMN1, COLUMN2 
    order by COLUMN3, COLUMN4) RN
where ___
qualify RN = 1
```

Footnotes:
* https://docs.snowflake.com/en/sql-reference/constructs/qualify.html