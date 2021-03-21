# Partitioning Types Overview

A partitioning type determines how a partitioned table's rows are distributed across partitions. Some partition types require the user to specify a partitioning expression that determines in which partition a row will be stored.

The size of individual partitions depends on the partitioning type. Read and write performance are affected by the partitioning expression. Therefore, these choices should be made carefully.

MariaDB supports the following partitioning types:

- [RANGE](/mariadb-administration/partitioning-tables/partitioning-types/range-partitioning-type/)
- [LIST](/kb/en/list-partitioning/)
- [RANGE COLUMNS and LIST COLUMNS](/mariadb-administration/partitioning-tables/partitioning-types/range-columns-and-list-columns-partitioning-types/), HASH COLUMNS
- HASH
- KEY
- LINEAR HASH, LINEAR KEY
- [SYSTEM_TIME](/sql-statements-structure/temporal-tables/system-versioned-tables/)