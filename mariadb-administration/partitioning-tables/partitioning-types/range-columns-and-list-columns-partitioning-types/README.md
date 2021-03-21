# RANGE COLUMNS and LIST COLUMNS Partitioning Types

RANGE COLUMNS and LIST COLUMNS are variants of, respectively, [RANGE](/mariadb-administration/partitioning-tables/partitioning-types/range-partitioning-type/) and [LIST](/kb/en/list-partitioning/). With these partitioning types there is not a single partitioning expression; instead, a list of one or more columns is accepted. The following rules apply:

- The list can contain one or more columns.
- Columns can be of any [integer](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int/), [string](/columns-storage-engines-and-plugins/data-types/string-data-types/), [DATE](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/date/), and [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime/) types.
- Only bare columns are permitted; no expressions.

All the specified columns are compared to the specified values to determine which partition should contain a specific row. See below for details.

## Syntax

The last part of a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) statement can be definition of the new table's partitions. In the case of RANGE COLUMNS partitioning, the syntax is the following:

```sql
PARTITION BY RANGE COLUMNS (col1, col2, ...)
(
	PARTITION partition_name VALUES LESS THAN (value1, value2, ...),
	[ PARTITION partition_name VALUES LESS THAN (value1, value2, ...), ... ]
)
```

The syntax for LIST COLUMNS is the following:

```sql
PARTITION BY LIST COLUMNS (partitioning_expression)
(
	PARTITION partition_name VALUES IN (value1, value2, ...),
	[ PARTITION partition_name VALUES IN (value1, value2, ...), ... ]
        [ PARTITION partititon_name DEFAULT ]
)
```

`partition_name` is the name of a partition.

## Comparisons

To determine which partition should contain a row, all specified columns will be compared to each partition definition.

With LIST COLUMNS, a row matches a partition if all row values are identical to the specified values. At most one partition can match the row.

With RANGE COLUMNS, a row matches a partition if all row values are less than specified values. The first partition that matches row values will be used.

`DEFAULT` partition catch all records which do not fit in other partitions. Only one `DEFAULT` partititon allowed. This option added in [MariaDB 10.2](/kb/en/what-is-mariadb-102/).