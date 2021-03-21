# UPDATE

## Syntax

Single-table syntax:

```sql
UPDATE [LOW_PRIORITY] [IGNORE] table_reference 
  [PARTITION (partition_list)]
  SET col1={expr1|DEFAULT} [,col2={expr2|DEFAULT}] ...
  [WHERE where_condition]
  [ORDER BY ...]
  [LIMIT row_count]
```

Multiple-table syntax:

```sql
UPDATE [LOW_PRIORITY] [IGNORE] table_references
    SET col1={expr1|DEFAULT} [, col2={expr2|DEFAULT}] ...
    [WHERE where_condition]
```

## Description

For the single-table syntax, the `UPDATE` statement updates
columns of existing rows in the named table with new values. The
`SET` clause indicates which columns to modify and the values
they should be given.  Each value can be given as an expression, or the keyword
`DEFAULT` to set a column explicitly to its default value. The
`WHERE` clause, if given, specifies the conditions that identify
which rows to update. With no `WHERE` clause, all rows are
updated. If the [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/) clause is specified, the rows are
updated in the order that is specified. The [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/) clause
places a limit on the number of rows that can be updated.

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The PARTITION clause was introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/). See [Partition Pruning and Selection](/mariadb-administration/partitioning-tables/partition-pruning-and-selection/) for details.

Until [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/), for the multiple-table syntax, `UPDATE` updates rows in each
table named in table_references that satisfy the conditions. In this case,
[ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/) and [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/) cannot be used. This restriction was lifted in [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/) and both clauses can be used with multiple-table updates. An `UPDATE` can also reference tables which are located in different databases; see [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers/) for the syntax.

`where_condition` is an expression that evaluates to true for
each row to be updated.

`table_references` and `where_condition` are as
specified as described in [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/).

For single-table updates, assignments are evaluated in left-to-right order, while for multi-table updates, there is no guarantee of a particular order. If the `SIMULTANEOUS_ASSIGNMENT` [sql_mode](/mariadb-administration/variables-and-modes/sql-mode/) (available from [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/)) is set, UPDATE statements evaluate all assignments simultaneously.

You need the `UPDATE` privilege only for columns referenced in
an `UPDATE` that are actually updated. You need only the
[SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) privilege for any columns that are read but
not modified. See [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/).

The `UPDATE` statement supports the following modifiers:

- If you use the `LOW_PRIORITY` keyword, execution of
  the `UPDATE` is delayed until no other clients are reading from
  the table. This affects only storage engines that use only table-level
  locking (MyISAM, MEMORY, MERGE). See [HIGH_PRIORITY and LOW_PRIORITY clauses](/kb/en/high_priority-and-low_priority-clauses/) for details.
- If you use the `IGNORE` keyword, the update statement does 
  not abort even if errors occur during the update. Rows for which
  duplicate-key conflicts occur are not updated. Rows for which columns are
  updated to values that would cause data conversion errors are updated to the
  closest valid values instead.

### UPDATE Statements With the Same Source and Target

##### MariaDB starting with [10.3.2](/kb/en/mariadb-1032-release-notes/)

From [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/), UPDATE statements may have the same source and target.

For example, given the following table:

```sql
DROP TABLE t1;
CREATE TABLE t1 (c1 INT, c2 INT);
INSERT INTO t1 VALUES (10,10), (20,20);
```

Until [MariaDB 10.3.1](/kb/en/mariadb-1031-release-notes/), the following UPDATE statement would not work:

```sql
UPDATE t1 SET c1=c1+1 WHERE c2=(SELECT MAX(c2) FROM t1);
ERROR 1093 (HY000): Table 't1' is specified twice, 
  both as a target for 'UPDATE' and as a separate source for data
```

From [MariaDB 10.3.2](/kb/en/mariadb-1032-release-notes/), the statement executes successfully:

```sql
UPDATE t1 SET c1=c1+1 WHERE c2=(SELECT MAX(c2) FROM t1);

SELECT * FROM t1;
+------+------+
| c1   | c2   |
+------+------+
|   10 |   10 |
|   21 |   20 |
+------+------+
```

## Example

Single-table syntax:

```sql
UPDATE table_name SET column1 = value1, column2 = value2 WHERE id=100;
```

Multiple-table syntax:

```sql
UPDATE tab1, tab2 SET tab1.column1 = value1, tab1.column2 = value2 WHERE tab1.id = tab2.id;
```

## See Also

- [How IGNORE works](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/ignore/)
- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)
- [ORDER BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/order-by/)
- [LIMIT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/limit/)
- [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers/)