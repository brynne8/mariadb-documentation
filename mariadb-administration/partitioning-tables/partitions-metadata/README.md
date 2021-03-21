# Partitions Metadata

The [PARTITIONS](/kb/en/information-schema-partitions-table/) table in the [INFORMATION_SCHEMA](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database contains information about partitions.

The [SHOW TABLE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status/) statement contains a `Create_options` column, that contains the string 'partitioned' for partitioned tables.

The [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) statement returns the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) statement that can be used to re-create a table, including the partitions definition.