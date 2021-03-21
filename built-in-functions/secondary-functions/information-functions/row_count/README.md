# ROW_COUNT

## Syntax

```sql
ROW_COUNT()
```

## Description

ROW_COUNT() returns the number of rows updated, inserted or deleted
by the preceding statement. This is the same as the row count that the
mysql client displays and the value from the [mysql_affected_rows()](/kb/en/mysql_affected_rows/) C
API function.

Generally:

- For statements which return a result set (such as [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/), [SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/), [DESC](/sql-statements-structure/sql-statements/administrative-sql-statements/describe/) or [HELP](/sql-statements-structure/sql-statements/administrative-sql-statements/help-command/)), returns -1, even when the result set is empty. This is also true for administrative statements, such as [OPTIMIZE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/).
- For DML statements other than [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) and for [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/), returns the number of affected rows.
- For DDL statements (including [TRUNCATE](/built-in-functions/numeric-functions/truncate/)) and for other statements which don't return any result set (such as [USE](/sql-statements-structure/sql-statements/administrative-sql-statements/use/), [DO](/sql-statements-structure/sql-statements/stored-routine-statements/do/), [SIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/signal/) or [DEALLOCATE PREPARE](/kb/en/deallocate-drop-prepared-statement/)), returns 0.

For [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/), affected rows is by default the number of rows that were actually changed. If the CLIENT_FOUND_ROWS flag to [mysql_real_connect()](/kb/en/mysql_real_connect/) is specified when connecting to mysqld, affected rows is instead the number of rows matched by the WHERE clause.

For [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace/), deleted rows are also counted. So, if REPLACE deletes a row and adds a new row, ROW_COUNT() returns 2.

For [INSERT ... ON DUPLICATE KEY](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/), updated rows are counted twice. So, if INSERT adds a new rows and modifies another row, ROW_COUNT() returns 3.

ROW_COUNT() does not take into account rows that are not directly deleted/updated by the last statement. This means that rows deleted by foreign keys or triggers are not counted.

<strong>Warning:</strong> You can use ROW_COUNT() with prepared statements, but you need to call it after EXECUTE, not after [DEALLOCATE PREPARE](/kb/en/deallocate-drop-prepared-statement/), because the row count for allocate prepare is always 0.

<strong>Warning:</strong> When used after a [CALL](/sql-statements-structure/sql-statements/stored-routine-statements/call/) statement, this function returns the number of rows affected by the last statement in the procedure, not by the whole procedure.

<strong>Warning:</strong> After [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/), ROW_COUNT() returns the number of the rows you tried to insert, not the number of the successful writes.

This information can also be found in the [diagnostics area](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/diagnostics-area/).

Statements using the ROW_COUNT() function are not [safe for replication](/kb/en/unsafe-statements-for-replication/).

## Examples

```sql
CREATE TABLE t (A INT);

INSERT INTO t VALUES(1),(2),(3);

SELECT ROW_COUNT();
+-------------+
| ROW_COUNT() |
+-------------+
|           3 |
+-------------+

DELETE FROM t WHERE A IN(1,2);

SELECT ROW_COUNT(); 
+-------------+
| ROW_COUNT() |
+-------------+
|           2 |
+-------------+
```

Example with prepared statements:

```sql
SET @q = 'INSERT INTO t VALUES(1),(2),(3);';

PREPARE stmt FROM @q;

EXECUTE stmt;
Query OK, 3 rows affected (0.39 sec)
Records: 3  Duplicates: 0  Warnings: 0

SELECT ROW_COUNT();
+-------------+
| ROW_COUNT() |
+-------------+
|           3 |
+-------------+
```

## See Also

- [FOUND_ROWS()](/built-in-functions/secondary-functions/information-functions/found_rows/)