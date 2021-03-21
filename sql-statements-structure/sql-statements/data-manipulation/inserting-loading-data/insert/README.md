# INSERT

## Syntax

```sql
INSERT [LOW_PRIORITY | DELAYED | HIGH_PRIORITY] [IGNORE]
 [INTO] tbl_name [PARTITION (partition_list)] [(col,...)]
 {VALUES | VALUE} ({expr | DEFAULT},...),(...),...
 [ ON DUPLICATE KEY UPDATE
   col=expr
     [, col=expr] ... ] [RETURNING select_expr 
      [, select_expr ...]]
```

Or:

```sql
INSERT [LOW_PRIORITY | DELAYED | HIGH_PRIORITY] [IGNORE]
    [INTO] tbl_name [PARTITION (partition_list)]
    SET col={expr | DEFAULT}, ...
    [ ON DUPLICATE KEY UPDATE
      col=expr
        [, col=expr] ... ] [RETURNING select_expr 
      [, select_expr ...]]
```

Or:

```sql
INSERT [LOW_PRIORITY | HIGH_PRIORITY] [IGNORE]
    [INTO] tbl_name [PARTITION (partition_list)] [(col,...)]
    SELECT ...
    [ ON DUPLICATE KEY UPDATE
      col=expr
        [, col=expr] ... ] [RETURNING select_expr 
      [, select_expr ...]]
```

The `INSERT` statement is used to insert new rows into an existing table. The `INSERT ... VALUES`
and `INSERT ... SET` forms of the statement insert rows based on explicitly specified values. The `INSERT ... SELECT` form inserts rows selected from another table or tables. `INSERT ... SELECT` is discussed further in the [INSERT ... SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select/) article.

The table name can be specified in the form `db_name`.`tbl_name` or, if a default database is selected, in the form `tbl_name` (see [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers/)). This allows to use [INSERT ... SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select/) to copy rows between different databases.

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The PARTITION clause was introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/). It can be used in both the INSERT and the SELECT part. See [Partition Pruning and Selection](/mariadb-administration/partitioning-tables/partition-pruning-and-selection/) for details.

##### MariaDB starting with [10.5](/kb/en/what-is-mariadb-105/)

The RETURNING clause was introduced in [MariaDB 10.5](/kb/en/what-is-mariadb-105/).

The columns list is optional. It specifies which values are explicitly inserted, and in which order. If this clause is not specified, all values must be explicitly specified, in the same order they are listed in the table definition.

The list of value follow the `VALUES` or `VALUE` keyword (which are interchangeable, regardless how much values you want to insert), and is wrapped by parenthesis. The values must be listed in the same order as the columns list. It is possible to specify more than one list to insert more than one rows with a single statement. If many rows are inserted, this is a speed optimization.

For one-row statements, the `SET` clause may be more simple, because you don't need to remember the columns order. All values are specified in the form `col` = `expr`.

Values can also be specified in the form of a SQL expression or subquery. However, the subquery cannot access the same table that is named in the `INTO` clause.

If you use the `LOW_PRIORITY` keyword, execution of the `INSERT` is delayed until no other clients are reading from the table. If you use the `HIGH_PRIORITY` keyword, the statement has the same priority as `SELECT`s. This affects only storage engines that use only table-level locking (MyISAM, MEMORY, MERGE). However, if one of these keywords is specified, [concurrent inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts/) cannot be used. See [HIGH_PRIORITY and LOW_PRIORITY clauses](/kb/en/high_priority-and-low_priority-clauses/) for details.

## INSERT DELAYED

For more details on the `DELAYED` option, see [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/).

## HIGH PRIORITY and LOW PRIORITY

See [HIGH_PRIORITY and LOW_PRIORITY](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/high_priority-and-low_priority/).

## Defaults and Duplicate Values

See [INSERT - Default &amp; Duplicate Values](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-default-duplicate-values/) for details..

## INSERT IGNORE

See [INSERT IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-ignore/).

## INSERT ON DUPLICATE KEY UPDATE

See [INSERT ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/).

## Examples

Specifying the column names:

```sql
INSERT INTO person (first_name, last_name) VALUES ('John', 'Doe');
```

Inserting more than 1 row at a time:

```sql
INSERT INTO tbl_name VALUES (1, "row 1"), (2, "row 2");
```

Using the `SET` clause:

```sql
INSERT INTO person SET first_name = 'John', last_name = 'Doe';
```

SELECTing from another table:

```sql
INSERT INTO contractor SELECT * FROM person WHERE status = 'c';
```

See [INSERT ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/) and [INSERT IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-ignore/) for further examples.

## INSERT ... RETURNING

`INSERT ... RETURNING` returns a resultset of the inserted rows.

This returns the listed columns for all the rows that are inserted, or alternatively, the specified SELECT expression. Any SQL expressions which can be calculated can be used in the select expression for the RETURNING clause, including virtual columns and aliases, expressions which use various operators such as bitwise, logical and arithmetic operators, string functions, date-time functions, numeric functions, control flow functions, secondary functions and stored functions. Along with this, statements which have subqueries and prepared statements can also be used.

### Examples

Simple INSERT statement

```sql
INSERT INTO t2 VALUES (1,'Dog'),(2,'Lion'),(3,'Tiger'),(4,'Leopard') 
RETURNING id2,id2+id2,id2&id2,id2||id2;
+-----+---------+---------+----------+
| id2 | id2+id2 | id2&id2 | id2||id2 |
+-----+---------+---------+----------+
|   1 |       2 |       1 |        1 |
|   2 |       4 |       2 |        1 |
|   3 |       6 |       3 |        1 |
|   4 |       8 |       4 |        1 |
+-----+---------+---------+----------+
```

Using stored functions in RETURNING

```sql
DELIMITER |
CREATE FUNCTION f(arg INT) RETURNS INT
    BEGIN
       RETURN (SELECT arg+arg);
    END|

DELIMITER ;

PREPARE stmt FROM "INSERT INTO t1 SET id1=1, animal1='Bear' RETURNING f(id1), UPPER(animal1)";

EXECUTE stmt;
+---------+----------------+
| f(id1)  | UPPER(animal1) |
+---------+----------------+
|       2 | BEAR           |
+---------+----------------+
```

Subqueries in the RETURNING clause that return more than one row or column cannot be used.

Aggregate functions cannot be used in the RETURNING clause. Since aggregate functions work on a set of values, and if the purpose is to get the row count, ROW_COUNT() with SELECT can be used or it can be used in INSERT...SELECT...RETURNING if the table in the RETURNING clause is not the same as the INSERT table.

## See Also

- [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/)
- [INSERT SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select/)
- [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace/) Equivalent to DELETE + INSERT of conflicting row.
- [HIGH_PRIORITY and LOW_PRIORITY](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/high_priority-and-low_priority/)
- [Concurrent Inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts/)
- [INSERT - Default &amp; Duplicate Values](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-default-duplicate-values/)
- [INSERT IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-ignore/)
- [INSERT ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/)
- [How to quickly insert data into MariaDB](/replication/optimization-and-tuning/query-optimizations/how-to-quickly-insert-data-into-mariadb/)