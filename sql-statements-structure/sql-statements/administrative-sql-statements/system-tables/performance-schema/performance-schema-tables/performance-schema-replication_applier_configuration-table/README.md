# Performance Schema replication_applier_configuration Table

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

The `replication_applier_configuration` table, along with many other new [Performance Schema tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables), was added in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/).

The [Performance Schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema) replication_applier_configuration table contains configuration settings affecting slave transactions.

It contains the following fields.

```sql
+---------------+----------+------+-----+---------+-------+
| Field         | Type     | Null | Key | Default | Extra |
+---------------+----------+------+-----+---------+-------+
| CHANNEL_NAME  | char(64) | NO   |     | NULL    |       |
| DESIRED_DELAY | int(11)  | NO   |     | NULL    |       |
+---------------+----------+------+-----+---------+-------+
```