# Partitioning Tables

A huge table can be split into smaller subsets. Both data and indexes are partitioned.

- [Partitioning Overview](/mariadb-administration/partitioning-tables/partitioning-overview/) — A table partitioning overview
- [Partitioning Types](/mariadb-administration/partitioning-tables/partitioning-types/) — A partitioning type determines how a table rows are distributed across partitions.
- [Partition Pruning and Selection](/mariadb-administration/partitioning-tables/partition-pruning-and-selection/) — Partition pruning is when the optimizer knows which partitions are relevant for the query
- [Partition Maintenance](/mariadb-administration/partitioning-tables/partition-maintenance/) — For time series (includes list of PARTITION uses)
- [Partitioning Limitations](/mariadb-administration/partitioning-tables/partitioning-limitations/) — Limitations applying to partitioning in MariaDB.
- [Partitions Files](/mariadb-administration/partitioning-tables/partitions-files/) — A partitioned table is stored in multiple files
- [Partitions Metadata](/mariadb-administration/partitioning-tables/partitions-metadata/) — How to obtain information about partitions definition