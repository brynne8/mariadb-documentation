# INSERT SELECT

## Syntax

```sql
INSERT [LOW_PRIORITY | HIGH_PRIORITY] [IGNORE]
    [INTO] tbl_name [(col_name,...)]
    SELECT ...
    [ ON DUPLICATE KEY UPDATE col_name=expr, ... ]
```

## Description

With <code class="highlight fixed" style="white-space:pre-wrap">INSERT ... SELECT</code>, you can quickly insert many rows
into a table from one or more other tables. For example:

```sql
INSERT INTO tbl_temp2 (fld_id)
  SELECT tbl_temp1.fld_order_id
  FROM tbl_temp1 WHERE tbl_temp1.fld_order_id > 100;
```

`tbl_name` can also be specified in the form `db_name`.`tbl_name` (see [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers/)). This allows to copy rows between different databases.

If the new table has a primary key or UNIQUE indexes, you can use [IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/ignore/) to handle duplicate key errors during the query. The newer values will not be inserted if an identical value already exists.

[REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace/) can be used instead of `INSERT` to prevent duplicates on `UNIQUE` indexes by deleting old values. In that case, `ON DUPLICATE KEY UPDATE` cannot be used.

`INSERT ... SELECT` works for tables which already exist. To create a table for a given resultset, you can use [CREATE TABLE ... SELECT](/sql-statements-structure/sql-statements/data-definition/create/create-table/).

## See Also

- [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/)
- [INSERT DELAYED](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-delayed/)
- [HIGH_PRIORITY and LOW_PRIORITY](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/high_priority-and-low_priority/)
- [Concurrent Inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts/)
- [INSERT - Default &amp; Duplicate Values](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-default-duplicate-values/)
- [INSERT IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-ignore/)
- [INSERT ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/)