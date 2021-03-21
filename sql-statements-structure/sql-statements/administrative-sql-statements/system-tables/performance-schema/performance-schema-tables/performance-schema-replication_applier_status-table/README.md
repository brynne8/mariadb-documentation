# Performance Schema replication_applier_status Table

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

The `replication_applier_status` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables), was added in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).

The [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema) replication_applier_status table contains information about the general transaction execution status on the slave.

It contains the following fields.

```sql
+----------------------------+---------------------+------+-----+---------+-------+
| Field                      | Type                | Null | Key | Default | Extra |
+----------------------------+---------------------+------+-----+---------+-------+
| CHANNEL_NAME               | char(64)            | NO   |     | NULL    |       |
| SERVICE_STATE              | enum('ON','OFF')    | NO   |     | NULL    |       |
| REMAINING_DELAY            | int(10) unsigned    | YES  |     | NULL    |       |
| COUNT_TRANSACTIONS_RETRIES | bigint(20) unsigned | NO   |     | NULL    |       |
+----------------------------+---------------------+------+-----+---------+-------+
```