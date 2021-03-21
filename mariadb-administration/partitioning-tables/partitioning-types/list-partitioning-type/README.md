# LIST Partitioning Type

LIST partitioning is conceptually similar to [RANGE partitioning](/mariadb-administration/partitioning-tables/partitioning-types/range-partitioning-type/). In both cases you decide a partitioning expression (a column, or a slightly more complex calculation) and use it to determine which partitions will contain each row. However, with the RANGE type, partitioning is done by assigning a range of values to each partition. With the LIST type, we assign a set of values to each partition. This is usually preferred if the partitioning expression can return a limited set of values.

A variant of this partitioning method, [LIST COLUMNS](/mariadb-administration/partitioning-tables/partitioning-types/range-columns-and-list-columns-partitioning-types/), allows us to use multiple columns and more datatypes.

## Syntax

The last part of a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) statement can be the definition of the new table's partitions. In the case of LIST partitioning, the syntax is the following:

```sql
PARTITION BY LIST (partitioning_expression)
(
	PARTITION partition_name VALUES IN (value_list),
	[ PARTITION partition_name VALUES IN (value_list), ... ]
        [ PARTITION partition_name DEFAULT ]
)
```

PARTITION BY LIST indicates that the partitioning type is LIST.

The `partitioning_expression` is an SQL expression that returns a value from each row. In the simplest cases, it is a column name. This value is used to determine which partition should contain a row.

`partition_name` is the name of a partition.

`value_list` is a list of values. If `partitioning_expression` returns one of these values, the row will be stored in this partition. If we try to insert something that does not belong to any of these value lists, the row will be rejected with an error.

The `DEFAULT` partition catches all records which do not fit into other partitions. Only one `DEFAULT` partition is permitted. This option was added in [MariaDB 10.2](/kb/en/what-is-mariadb-102/).

## Use cases

LIST partitioning can be useful when we have a column that can only contain a limited set of values. Even in that case, RANGE partitioning could be used instead; but LIST partitioning allows us to equally distribute the rows by assigning a proper set of values to each partition.