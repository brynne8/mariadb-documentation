# mysql.time_zone Table

The `mysql.time_zone` table is one of the mysql system tables that can contain [time zone](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones/) information. It is usually preferable for the system to handle the time zone, in which case the table will be empty (the default), but you can populate the mysql time zone tables using the [mysql_tzinfo_to_sql](/clients-utilities/mysql_tzinfo_to_sql/) utility. See [Time Zones](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones/) for details.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, this table uses the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, this table uses the [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/) storage engine.

The `mysql.time_zone` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>Time_zone_id</code></td><td><code>int(10) unsigned</code></td><td>NO</td><td>PRI</td><td><code>NULL</code></td><td>ID field, auto_increments.</td></tr>
<tr><td><code>Use_leap_seconds</code></td><td><code>enum('Y','N')</code></td><td>NO</td><td></td><td>N</td><td>Whether or not leap seconds are used.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM mysql.time_zone;
+--------------+------------------+
| Time_zone_id | Use_leap_seconds |
+--------------+------------------+
|            1 | N                |
|            2 | N                |
|            3 | N                |
|            4 | N                |
|            5 | N                |
|            6 | N                |
|            7 | N                |
|            8 | N                |
|            9 | N                |
|           10 | N                |
...
+--------------+------------------+
```

## See Also

- [mysql.time_zone_leap_second table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqltime_zone_leap_second-table/)
- [mysql.time_zone_name table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqltime_zone_name-table/)
- [mysql.time_zone_transition table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqltime_zone_transition-table/)
- [mysql.time_zone_transition_type table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqltime_zone_transition_type-table/)