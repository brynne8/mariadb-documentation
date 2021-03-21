# Query Response Time Plugin

##### MariaDB starting with [10.0.4](/kb/en/mariadb-1004-release-notes/)

The `query_response_time` plugin was first released in [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/).

The `query_response_time` plugin creates the  <a undefined>QUERY_RESPONSE_TIME</a> table in the [INFORMATION_SCHEMA](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database. The plugin also adds the [SHOW QUERY_RESPONSE_TIME](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-query_response_time/) and [FLUSH  QUERY_RESPONSE_TIME](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) statements.

The [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/) provides exact information about queries that take a long time to execute. However, sometimes there are a large number of queries that each take a very short amount of time to execute. This feature provides a tool for analyzing that information by counting and displaying the number of queries according to the the length of time they took to execute.

This feature is based on Percona's [Response Time Distribution](http://www.percona.com/doc/percona-server/5.5/diagnostics/response_time_distribution.html).

## Installing the Plugin

This shared library actually consists of two different plugins:

- `QUERY_RESPONSE_TIME` - An INFORMATION_SCHEMA plugin that exposes statistics.
- `QUERY_RESPONSE_TIME_AUDIT` - audit plugin, collects statistics.

Both plugins need to be installed to get meaningful statistics.

Although the plugin's shared library is distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/). For example:

```sql
INSTALL SONAME 'query_response_time';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). For example:

```sql
[mariadb]
...
plugin_load_add = query_response_time
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/). For example:

```sql
UNINSTALL SONAME 'query_response_time';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Response Time Distribution

The user can define time intervals that divide the range 0 to positive infinity into smaller intervals and then collect the number of commands whose execution times fall into each of those intervals.

Each interval is described as:

```sql
(range_base ^ n; range_base ^ (n+1)]
```

The range_base is some positive number (see Limitations). The interval is defined as the difference between two nearby powers of the range base.

For example, if the range base=10, we have the following intervals:

```sql
(0; 10 ^ -6], (10 ^ -6; 10 ^ -5], (10 ^ -5; 10 ^ -4], ..., 
  (10 ^ -1; 10 ^1], (10^1; 10^2]...(10^7; positive infinity]
```

or

```sql
(0; 0.000001], (0.000001; 0.000010], (0.000010; 0.000100], ..., 
  (0.100000; 1.0]; (1.0; 10.0]...(1000000; positive infinity]
```

For each interval, a count is made of the queries with execution times that fell into that interval.

You can select the range of the intervals by changing the range base. For example, for base range=2 we have the following intervals:

```sql
(0; 2 ^ -19], (2 ^ -19; 2 ^ -18], (2 ^ -18; 2 ^ -17], ..., 
  (2 ^ -1; 2 ^1], (2 ^ 1; 2 ^ 2]...(2 ^ 25; positive infinity]
```

or

```sql
(0; 0.000001], (0.000001, 0.000003], ..., 
  (0.25; 0.5], (0.5; 2], (2; 4]...(8388608; positive infinity]
```

Small numbers look strange (i.e., don’t look like powers of 2), because we lose precision on division when the ranges are calculated at runtime. In the resulting table, you look at the high boundary of the range.

For example, you may see:

```sql
SELECT * FROM INFORMATION_SCHEMA.QUERY_RESPONSE_TIME;
+----------------+-------+----------------+
| TIME           | COUNT | TOTAL          |
+----------------+-------+----------------+
|       0.000001 |     0 |       0.000000 |
|       0.000010 |    17 |       0.000094 |
|       0.000100 |  4301         0.236555 |
|       0.001000 |  1499 |       0.824450 |
|       0.010000 | 14851 |      81.680502 |
|       0.100000 |  8066 |     443.635693 |
|       1.000000 |     0 |       0.000000 |
|      10.000000 |     0 |       0.000000 |
|     100.000000 |     1 |      55.937094 |
|    1000.000000 |     0 |       0.000000 |
|   10000.000000 |     0 |       0.000000 |
|  100000.000000 |     0 |       0.000000 |
| 1000000.000000 |     0 |       0.000000 |
| TOO LONG       |     0 | TOO LONG       |
+----------------+-------+----------------+
```

This means there were:

```sql
* 17 queries with 0.000001 < query execution time < = 0.000010 seconds; total execution time of the 17 queries = 0.000094 seconds

* 4301 queries with 0.000010 < query execution time < = 0.000100 seconds; total execution time of the 4301 queries = 0.236555 seconds

* 1499 queries with 0.000100 < query execution time < = 0.001000 seconds; total execution time of the 1499 queries = 0.824450 seconds

* 14851 queries with 0.001000 < query execution time < = 0.010000 seconds; total execution time of the 14851 queries = 81.680502 seconds

* 8066 queries with 0.010000 < query execution time < = 0.100000 seconds; total execution time of the 8066 queries = 443.635693 seconds

* 1 query with 10.000000 < query execution time < = 100.0000 seconds; total execution time of the 1 query = 55.937094 seconds
```

## Using the Plugin

### Using the Information Schema Table

You can get the distribution by querying the the <a undefined>QUERY_RESPONSE_TIME</a> table in the [INFORMATION_SCHEMA](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database. For example:

```sql
SELECT * FROM INFORMATION_SCHEMA.QUERY_RESPONSE_TIME;
```

You can also write more complex queries. For example:

```sql
SELECT c.count, c.time,
(SELECT SUM(a.count) FROM INFORMATION_SCHEMA.QUERY_RESPONSE_TIME as a 
   WHERE a.count != 0) as query_count,
(SELECT COUNT(*)     FROM INFORMATION_SCHEMA.QUERY_RESPONSE_TIME as b 
  WHERE b.count != 0) as not_zero_region_count,
(SELECT COUNT(*)     FROM INFORMATION_SCHEMA.QUERY_RESPONSE_TIME) as region_count
FROM INFORMATION_SCHEMA.QUERY_RESPONSE_TIME as c 
  WHERE c.count > 0;
```

Note: If <a undefined>query_response_time_stats</a> is set to `ON`, then the execution times for these two SELECT queries will also be collected.

### Using the SHOW Statement

##### MariaDB starting with [10.1.1](/kb/en/mariadb-1011-release-notes/)

Starting with [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), as an alternative to the <a undefined>QUERY_RESPONSE_TIME</a> table in the [INFORMATION_SCHEMA](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database, you can also use the [SHOW QUERY_RESPONSE_TIME](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-query_response_time/) statement. For example:

```sql
SHOW QUERY_RESPONSE_TIME;
```

##### MariaDB until [10.1.1](/kb/en/mariadb-1011-release-notes/)

Prior to [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), the [SHOW QUERY_RESPONSE_TIME](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-query_response_time/) statement is not supported.

### Flushing Plugin Data

Flushing the plugin data does two things:

- Clears the collected times from the <a undefined>QUERY_RESPONSE_TIME</a> table in the [INFORMATION_SCHEMA](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/) database.
- Reads the value of <a undefined>query_response_time_range_base</a> and uses it to set the range base for the table.

##### MariaDB starting with [10.1.1](/kb/en/mariadb-1011-release-notes/)

Starting with [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), plugin data can be flushed with the [FLUSH  QUERY_RESPONSE_TIME](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) statement. For example:

```sql
FLUSH QUERY_RESPONSE_TIME;
```

Setting the <a undefined>query_response_time_flush</a> system variable has the same effect. For example:

```sql
SET GLOBAL query_response_time_flush=1;
```

##### MariaDB until [10.1.1](/kb/en/mariadb-1011-release-notes/)

Prior to [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/), the [FLUSH  QUERY_RESPONSE_TIME](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) statement is not supported. To flush plugin data, set the <a undefined>query_response_time_flush</a> system variable. For example:

```sql
SET GLOBAL query_response_time_flush=1;
```

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>1.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10113-release-notes/">MariaDB 10.1.13</a></td></tr>
<tr><td>1.0</td><td>Gamma</td><td><a href="/kb/en/mariadb-10010-release-notes/">MariaDB 10.0.10</a></td></tr>
<tr><td>1.0</td><td>Alpha</td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
</tbody></table>

## System Variables

### `query_response_time_flush`

- <strong>Description:</strong> Updating this variable flushes the statistics and re-reads [query_response_time_range_base](#query_response_time_range_base).
- <strong>Commandline:</strong> None
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

### `query_response_time_range_base`

- <strong>Description:</strong> Select base of log for `QUERY_RESPONSE_TIME` ranges. WARNING: variable change takes affect only after flush.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--query-response-time-range-base=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `10`
- <strong>Range:</strong> `2` to `1000`

---

### `query_response_time_exec_time_debug`

- <strong>Description:</strong> Pretend queries take this many microseconds. When 0 (the default) use the actual execution time. 
<ul start="1"><li>This system variable is only available when the plugin is a [debug build](/kb/en/compiling-mariadb-for-debugging/).
</li></ul>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `31536000`

---

### `query_response_time_stats`

- <strong>Description:</strong> Enable or disable query response time statistics collecting
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">query-response-time-stats[={0|1}]</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `boolean`
- <strong>Default Value:</strong> `OFF`

---

## Options

### `query_response_time`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the <a undefined>mysql.plugins</a> table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--query-response-time=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---

### `query_response_time_audit`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the <a undefined>mysql.plugins</a> table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname/) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin/) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--query-response-time-audit=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`

---