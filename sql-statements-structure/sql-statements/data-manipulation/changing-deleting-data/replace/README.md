# REPLACE

## Syntax

```sql
REPLACE [LOW_PRIORITY | DELAYED]
 [INTO] tbl_name [PARTITION (partition_list)] [(col,...)]
 {VALUES | VALUE} ({expr | DEFAULT},...),(...),...
[RETURNING select_expr 
      [, select_expr ...]]
```

Or:

```sql
REPLACE [LOW_PRIORITY | DELAYED]
    [INTO] tbl_name [PARTITION (partition_list)]
    SET col={expr | DEFAULT}, ...
[RETURNING select_expr 
      [, select_expr ...]]
```

Or:

```sql
REPLACE [LOW_PRIORITY | DELAYED]
    [INTO] tbl_name [PARTITION (partition_list)] [(col,...)]
    SELECT ...
[RETURNING select_expr 
      [, select_expr ...]]
```

## Description

<code class="highlight fixed" style="white-space:pre-wrap">REPLACE</code> works exactly like
 <code class="highlight fixed" style="white-space:pre-wrap">[INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert)</code>, except that if an old row in the table
 has the same value as a new row for a <code class="highlight fixed" style="white-space:pre-wrap">PRIMARY KEY</code> or a
 <code class="highlight fixed" style="white-space:pre-wrap">UNIQUE</code> index, the old row is deleted before the new row is
 inserted. If the table has more than one `UNIQUE` keys, it is possible that the new row conflicts with more than one row. In this case, all conflicting rows will be deleted.

The table name can be specified in the form `db_name`.`tbl_name` or, if a default database is selected, in the form `tbl_name` (see [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers)). This allows to use [REPLACE ... SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select) to copy rows between different databases.
<br><br>

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The PARTITION clause was introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/). See [Partition Pruning and Selection](/mariadb-administration/partitioning-tables/partition-pruning-and-selection) for details.

##### MariaDB starting with [10.5.0](/kb/en/mariadb-1050-release-notes/)

The RETURNING clause was introduced in [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/)

Basically it works like this:

```sql
BEGIN;
SELECT 1 FROM t1 WHERE key=# FOR UPDATE;
IF found-row
  DELETE FROM t1 WHERE key=# ;
  INSERT INTO t1 VALUES (...);
ENDIF
END;
```

The above can be replaced with:

```sql
REPLACE INTO t1 VALUES (...)
```

<code class="highlight fixed" style="white-space:pre-wrap">REPLACE</code> is a MariaDB/MySQL extension to the SQL standard. It
 either inserts, or deletes and inserts. For other MariaDB/MySQL extensions to
 standard SQL --- that also handle duplicate values --- see [IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/ignore) and [INSERT ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update).

Note that unless the table has a <code class="highlight fixed" style="white-space:pre-wrap">PRIMARY KEY</code> or
 <code class="highlight fixed" style="white-space:pre-wrap">UNIQUE</code> index, using a <code class="highlight fixed" style="white-space:pre-wrap">REPLACE</code> statement
makes no sense. It becomes equivalent to <code class="highlight fixed" style="white-space:pre-wrap">INSERT</code>, because
there is no index to be used to determine whether a new row duplicates another.

Values for all columns are taken from the values sSee [Partition Pruning and Selection](/mariadb-administration/partitioning-tables/partition-pruning-and-selection) for details.pecified in the
 <code class="highlight fixed" style="white-space:pre-wrap">REPLACE</code> statement. Any missing columns are set to their
default values, just as happens for <code class="highlight fixed" style="white-space:pre-wrap">INSERT</code>. You cannot refer
to values from the current row and use them in the new row. If you use an
assignment such as <code class="highlight fixed" style="white-space:pre-wrap">'SET col = col + 1'</code>, the
reference to the column name on the right hand side is treated as
 <code class="highlight fixed" style="white-space:pre-wrap">DEFAULT(col)</code>, so the assignment is equivalent to
 <code class="highlight fixed" style="white-space:pre-wrap">'SET col = DEFAULT(col) + 1'</code>.

To use <code class="highlight fixed" style="white-space:pre-wrap">REPLACE</code>, you must have both the
 <code class="highlight fixed" style="white-space:pre-wrap">INSERT</code> and <code class="highlight fixed" style="white-space:pre-wrap">DELETE</code> [privileges](/sql-statements-structure/sql-statements/account-management-sql-commands/grant)
for the table.

There are some gotchas you should be aware of, before using `REPLACE`:

- If there is an [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment) field, a new value will be generated.
- If there are foreign keys, `ON DELETE` action will be activated by `REPLACE`.
- [Triggers](/programming-customizing-mariadb/triggers-events/triggers) on `DELETE` and `INSERT` will be activated by `REPLACE`.

To avoid some of these behaviors, you can use `INSERT ... ON DUPLICATE KEY UPDATE`.

This statement activates INSERT and DELETE triggers. See [Trigger Overview](/programming-customizing-mariadb/triggers-events/triggers/trigger-overview) for details.

## REPLACE RETURNING

`REPLACE ... RETURNING` returns a resultset of the replaced rows.

This returns the listed columns for all the rows that are replaced, or alternatively, the specified SELECT expression. Any SQL expressions which can be calculated can be used in the select expression for the RETURNING clause, including virtual columns and aliases, expressions which use various operators such as bitwise, logical and arithmetic operators, string functions, date-time functions, numeric functions, control flow functions, secondary functions and stored functions. Along with this, statements which have subqueries and prepared statements can also be used.

### Examples

Simple REPLACE statement

```sql
REPLACE INTO t2 VALUES (1,'Leopard'),(2,'Dog') RETURNING id2, id2+id2 
as Total ,id2|id2, id2&&id2;
+-----+-------+---------+----------+
| id2 | Total | id2|id2 | id2&&id2 |
+-----+-------+---------+----------+
|   1 |     2 |       1 |        1 |
|   2 |     4 |       2 |        1 |
+-----+-------+---------+----------+
```

Using stored functions in RETURNING

```sql
DELIMITER |
CREATE FUNCTION f(arg INT) RETURNS INT
    BEGIN
      RETURN (SELECT arg+arg);
    END|

DELIMITER ;
PREPARE stmt FROM "REPLACE INTO t2 SET id2=3, animal2='Fox' RETURNING f2(id2),
UPPER(animal2)";

EXECUTE stmt;
+---------+----------------+
| f2(id2) | UPPER(animal2) |
+---------+----------------+
|       6 | FOX            |
+---------+----------------+
```

Subqueries in the statement

```sql
REPLACE INTO t1 SELECT * FROM t2 RETURNING (SELECT id2 FROM t2 WHERE 
id2 IN (SELECT id2 FROM t2 WHERE id2=1)) AS new_id;
+--------+
| new_id |
+--------+
|      1 |
|      1 |
|      1 |
|      1 |
+--------+
```

Subqueries in the RETURNING clause that return more than one row or column cannot be used..

Aggregate functions cannot be used in the RETURNING clause. Since aggregate functions work on a set of values and if the purpose is to get the row count, ROW_COUNT() with SELECT can be used, or it can be used in REPLACE...SEL== Description

`REPLACE ... RETURNING` returns a resultset of the replaced rows.

This returns the listed columns for all the rows that are replaced, or alternatively, the specified SELECT expression. Any SQL expressions which can be calculated can be used in the select expression for the RETURNING clause, including virtual columns and aliases, expressions which use various operators such as bitwise, logical and arithmetic operators, string functions, date-time functions, numeric functions, control flow functions, secondary functions and stored functions. Along with this, statements which have subqueries and prepared statements can also be used.

## Examples

Simple REPLACE statement

```sql
REPLACE INTO t2 VALUES (1,'Leopard'),(2,'Dog') RETURNING id2, id2+id2 
as Total ,id2|id2, id2&&id2;
+-----+-------+---------+----------+
| id2 | Total | id2|id2 | id2&&id2 |
+-----+-------+---------+----------+
|   1 |     2 |       1 |        1 |
|   2 |     4 |       2 |        1 |
+-----+-------+---------+----------+
```

Using stored functions in RETURNING

```sql
DELIMITER |
CREATE FUNCTION f(arg INT) RETURNS INT
    BEGIN
      RETURN (SELECT arg+arg);
    END|

DELIMITER ;
PREPARE stmt FROM "REPLACE INTO t2 SET id2=3, animal2='Fox' RETURNING f2(id2),
UPPER(animal2)";

EXECUTE stmt;
+---------+----------------+
| f2(id2) | UPPER(animal2) |
+---------+----------------+
|       6 | FOX            |
+---------+----------------+
```

Subqueries in the statement

```sql
REPLACE INTO t1 SELECT * FROM t2 RETURNING (SELECT id2 FROM t2 WHERE 
id2 IN (SELECT id2 FROM t2 WHERE id2=1)) AS new_id;
+--------+
| new_id |
+--------+
|      1 |
|      1 |
|      1 |
|      1 |
+--------+
```

Subqueries in the RETURNING clause that return more than one row or column cannot be used..

Aggregate functions cannot be used in the RETURNING clause. Since aggregate functions work on a set of values and if the purpose is to get the row count, ROW_COUNT() with SELECT can be used, or it can be used in REPLACE...SELECT...RETURNING if the table in the RETURNING clause is not the same as the REPLACE table.
ECT...RETURNING if the table in the RETURNING clause is not the same as the REPLACE table.

## See Also

- [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert)
- [HIGH_PRIORITY and LOW_PRIORITY clauses](/kb/en/high_priority-and-low_priority-clauses/)
- [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed) for details on the `DELAYED` clause