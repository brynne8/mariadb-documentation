# FOUND_ROWS

## Syntax

```sql
FOUND_ROWS()
```

## Description

A [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statement may include a [LIMIT](/kb/en/select/#limit) clause to restrict the number
of rows the server returns to the client. In some cases, it is
desirable to know how many rows the statement would have returned
without the LIMIT, but without running the statement again. To obtain
this row count, include a [SQL_CALC_FOUND_ROWS](/kb/en/select/#sql_calc_found_rows) option in the SELECT
statement, and then invoke FOUND_ROWS() afterwards.

You can also use FOUND_ROWS() to obtain the number of rows returned by a [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) which does not contain a [LIMIT](/kb/en/select/#limit) clause. In this case you don't need to use the [SQL_CALC_FOUND_ROWS](/kb/en/select/#sql_calc_found_rows) option. This can be useful for example in a [stored procedure](/programming-customizing-mariadb/stored-routines/stored-procedures/).

Also, this function works with some other statements which return a resultset, including [SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/), [DESC](/sql-statements-structure/sql-statements/administrative-sql-statements/describe/) and [HELP](/sql-statements-structure/sql-statements/administrative-sql-statements/help-command/). For [DELETE ... RETURNING](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) you should use [ROW_COUNT()](/kb/en/information-functions-row_count/). It also works as a [prepared statement](/sql-statements-structure/sql-statements/prepared-statements/), or after executing a prepared statement.

Statements which don't return any results don't affect FOUND_ROWS() - the previous value will still be returned.

<strong>Warning:</strong> When used after a [CALL](/sql-statements-structure/sql-statements/stored-routine-statements/call/) statement, this function returns the number of rows selected by the last query in the procedure, not by the whole procedure.

Statements using the FOUND_ROWS() function are not [safe for replication](/kb/en/unsafe-statements-for-replication/).

## Examples

```sql
SHOW ENGINES;
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| Engine             | Support | Comment                                                        | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
| InnoDB             | DEFAULT | Supports transactions, row-level locking, and foreign keys     | YES          | YES  | YES        |
...
| SPHINX             | YES     | Sphinx storage engine                                          | NO           | NO   | NO         |
+--------------------+---------+----------------------------------------------------------------+--------------+------+------------+
11 rows in set (0.01 sec)

SELECT FOUND_ROWS();
+--------------+
| FOUND_ROWS() |
+--------------+
|           11 |
+--------------+

SELECT SQL_CALC_FOUND_ROWS * FROM tbl_name WHERE id > 100 LIMIT 10;

SELECT FOUND_ROWS();
+--------------+
| FOUND_ROWS() |
+--------------+
|           23 |
+--------------+
```

## See Also

- [ROW_COUNT()](/kb/en/information-functions-row_count/)