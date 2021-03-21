# Performance Schema Overview

The Performance Schema is a feature for monitoring server performance.
<br>
<br>
<br>

## Versions in MariaDB

<table><tbody><tr><th>Performance Schema Version</th><th>Introduced</th></tr>
<tr><td>5.6.43</td><td><a href="/kb/en/mariadb-10038-release-notes/">MariaDB 10.0.38</a></td></tr>
<tr><td>5.6.42</td><td><a href="/kb/en/mariadb-10037-release-notes/">MariaDB 10.0.37</a></td></tr>
<tr><td>5.6.39</td><td><a href="/kb/en/mariadb-10034-release-notes/">MariaDB 10.0.34</a></td></tr>
<tr><td>5.6.38</td><td><a href="/kb/en/mariadb-10033-release-notes/">MariaDB 10.0.33</a></td></tr>
<tr><td>5.6.37</td><td><a href="/kb/en/mariadb-10126-release-notes/">MariaDB 10.1.26</a>, <a href="/kb/en/mariadb-10032-release-notes/">MariaDB 10.0.32</a></td></tr>
<tr><td>5.6.36</td><td><a href="/kb/en/mariadb-1027-release-notes/">MariaDB 10.2.7</a>, <a href="/kb/en/mariadb-10124-release-notes/">MariaDB 10.1.24</a>, <a href="/kb/en/mariadb-10031-release-notes/">MariaDB 10.0.31</a></td></tr>
<tr><td>5.6.35</td><td><a href="/kb/en/mariadb-10121-release-notes/">MariaDB 10.1.21</a>, <a href="/kb/en/mariadb-10029-release-notes/">MariaDB 10.0.29</a></td></tr>
<tr><td>5.6.33</td><td><a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a>, <a href="/kb/en/mariadb-10118-release-notes/">MariaDB 10.1.18</a>, <a href="/kb/en/mariadb-10028-release-notes/">MariaDB 10.0.28</a></td></tr>
<tr><td>5.6.32</td><td><a href="/kb/en/mariadb-10117-release-notes/">MariaDB 10.1.17</a>, <a href="/kb/en/mariadb-10027-release-notes/">MariaDB 10.0.27</a></td></tr>
<tr><td>5.6.31</td><td><a href="/kb/en/mariadb-10115-release-notes/">MariaDB 10.1.15</a>, <a href="/kb/en/mariadb-10026-release-notes/">MariaDB 10.0.26</a></td></tr>
<tr><td>5.6.30</td><td><a href="/kb/en/mariadb-10114-release-notes/">MariaDB 10.1.14</a>, <a href="/kb/en/mariadb-10025-release-notes/">MariaDB 10.0.25</a></td></tr>
<tr><td>5.6.29</td><td><a href="/kb/en/mariadb-10112-release-notes/">MariaDB 10.1.12</a>, <a href="/kb/en/mariadb-10024-release-notes/">MariaDB 10.0.24</a></td></tr>
<tr><td>5.6.28</td><td><a href="/kb/en/mariadb-10023-release-notes/">MariaDB 10.0.23</a></td></tr>
<tr><td>5.6.27</td><td><a href="/kb/en/mariadb-1018-release-notes/">MariaDB 10.1.8</a> , <a href="/kb/en/mariadb-10022-release-notes/">MariaDB 10.0.22</a></td></tr>
<tr><td>5.6.26</td><td><a href="/kb/en/mariadb-1017-release-notes/">MariaDB 10.1.7</a>, <a href="/kb/en/mariadb-10021-release-notes/">MariaDB 10.0.21</a></td></tr>
<tr><td>5.6.25</td><td><a href="/kb/en/mariadb-1016-release-notes/">MariaDB 10.1.6</a>, <a href="/kb/en/mariadb-10020-release-notes/">MariaDB 10.0.20</a></td></tr>
<tr><td>5.6.24</td><td><a href="/kb/en/mariadb-10018-release-notes/">MariaDB 10.0.18</a></td></tr>
<tr><td>5.6.20</td><td><a href="/kb/en/mariadb-10013-release-notes/">MariaDB 10.0.13</a></td></tr>
<tr><td>5.6.17</td><td><a href="/kb/en/mariadb-10011-release-notes/">MariaDB 10.0.11</a></td></tr>
</tbody></table>

## Introduction

It is implemented as a storage engine, and so will appear in the list of storage engines available.

```sql
SHOW ENGINES;
+--------------------+---------+----------------------------------+--------------+------+------------+
| Engine             | Support | Comment                          | Transactions | XA   | Savepoints |
+--------------------+---------+----------------------------------+--------------+------+------------+
| ...                |         |                                  |              |      |            |
| PERFORMANCE_SCHEMA | YES     | Performance Schema               | NO           | NO   | NO         |
| ...                |         |                                  |              |      |            |
+--------------------+---------+----------------------------------+--------------+------+------------+
```

However, `performance_schema` is not a regular storage engine for storing data, it's a mechanism for implementing the Performance Schema feature.

The storage engine contains a database called `performance_schema`, which in turn consists of a number of tables that can be queried with regular SQL statements, returning specific performance information.

```sql
USE performance_schema
```

```sql
SHOW TABLES;
+----------------------------------------------------+
| Tables_in_performance_schema                       |
+----------------------------------------------------+
| accounts                                           |
...
| users                                              |
+----------------------------------------------------+
80 rows in set (0.00 sec)
```

See [List of Performance Schema Tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables) for a full list and links to detailed descriptions of each table. From [MariaDB 10.5](/kb/en/what-is-mariadb-105/), there are 80 Performance Schema tables, while until [MariaDB 10.4](/kb/en/what-is-mariadb-104/), there are 52.

## Activating the Performance Schema

The performance schema is disabled by default for performance reasons. You can check its current status by looking at the value of the [performance_schema](/kb/en/performance-schema-system-variables/#performance_schema) system variable.

```sql
SHOW VARIABLES LIKE 'performance_schema';
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| performance_schema | ON    |
+--------------------+-------+
```

The performance schema cannot be activated at runtime - it must be set when the server starts by adding the following line in your `my.cnf` configuration file.

```sql
performance_schema=ON
```

Until [MariaDB 10.4](/kb/en/what-is-mariadb-104/), all memory used by the Performance Schema is allocated at startup. From [MariaDB 10.5](/kb/en/what-is-mariadb-105/), some memory is allocated dynamically, depending on load, number of connections, number of tables open etc.

## Enabling the Performance Schema

You need to set up all consumers (starting collection of data) and instrumentations (what to collect):

```sql
UPDATE performance_schema.setup_consumers SET ENABLED = 'YES';
UPDATE performance_schema.setup_instruments SET ENABLED = 'YES', TIMED = 'YES';
```

You can decide what to enable/disable with `WHERE NAME like "%what_to_enable"`;
You can disable instrumentations by setting `ENABLED` to `"NO"`.

You can also do this in your my.cnf file.
The following enables all instrumentation of all stages (computation units) in MariaDB:

```sql
[mysqld]
performance_schema=ON
performance-schema-instrument='stage/%=ON'
performance-schema-consumer-events-stages-current=ON
performance-schema-consumer-events-stages-history=ON
performance-schema-consumer-events-stages-history-long=ON
```

## Listing Performance Schema Variables

```sql
SHOW VARIABLES LIKE "perf%";
+--------------------------------------------------------+-------+
| Variable_name                                          | Value |
+--------------------------------------------------------+-------+
| performance_schema                                     | ON    |
...
| performance_schema_users_size                          | 100   |
+--------------------------------------------------------+-------+
```

See [Performance Schema System Variables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-system-variables) for a full list of available system variables.

Note that the "consumer" events are not shown on this list, as they are only available as options, not as system variables, and they can only be enabled at [startup](/kb/en/mysqld-options/#performance-schema-options).

## See Also

- [Performance schema options](/kb/en/mysqld-options/#performance-schema-options)
- [SHOW ENGINE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine)
- [SHOW PROFILE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-profile)<code>
</code>
- [ANALYZE STATEMENT](/analyze-statement)
- [Performance schema in MySQL 5.6](https://dev.mysql.com/doc/refman/5.6/en/performance-schema.html). All things here should also work for MariaDB.