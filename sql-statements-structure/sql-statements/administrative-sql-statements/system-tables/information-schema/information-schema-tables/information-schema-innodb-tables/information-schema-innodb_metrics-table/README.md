# Information Schema INNODB_METRICS Table

##### MariaDB starting with [10.0.0](/kb/en/mariadb-1000-release-notes/)

The `INNODB_METRICS` table was added in [MariaDB 10.0.0](/kb/en/mariadb-1000-release-notes/)

The [Information Schema](/kb/en/information_schema/) `INNODB_METRICS` table contains a list of useful InnoDB performance metrics. Each row in the table represents an instrumented counter that can be stopped, started and reset, and which can be grouped together by module.

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>NAME</code></td><td>Unique counter name.</td></tr>
<tr><td><code>SUBSYSTEM</code></td><td>InnoDB subsystem. See below for the matching module to use to enable/disable monitoring this subsytem with the <a href="/kb/en/innodb-system-variables/#innodb_monitor_enable">innodb_monitor_enable</a> and <a href="/kb/en/innodb-system-variables/#innodb_monitor_disable">innodb_monitor_disable</a> system variables.</td></tr>
<tr><td><code>COUNT</code></td><td>Count since being enabled.</td></tr>
<tr><td><code>MAX_COUNT</code></td><td>Maximum value since being enabled.</td></tr>
<tr><td><code>MIN_COUNT</code></td><td>Minimum value since being enabled.</td></tr>
<tr><td><code>AVG_COUNT</code></td><td>Average value since being enabled.</td></tr>
<tr><td><code>COUNT_RESET</code></td><td>Count since last being reset.</td></tr>
<tr><td><code>MAX_COUNT_RESET</code></td><td>Maximum value since last being reset.</td></tr>
<tr><td><code>MIN_COUNT_RESET</code></td><td>Minimum value since last being reset.</td></tr>
<tr><td><code>AVG_COUNT_RESET</code></td><td>Average value since last being reset.</td></tr>
<tr><td><code>TIME_ENABLED</code></td><td>Time last enabled.</td></tr>
<tr><td><code>TIME_DISABLED</code></td><td>Time last disabled</td></tr>
<tr><td><code>TIME_ELAPSED</code></td><td>Time since enabled</td></tr>
<tr><td><code>TIME_RESET</code></td><td>Time last reset.</td></tr>
<tr><td><code>STATUS</code></td><td>Whether the counter is currently enabled to disabled.</td></tr>
<tr><td><code>TYPE</code></td><td>Item type; one of <code>counter</code>, <code>value</code>, <code>status_counter</code>, <code>set_owner</code>, <code>set_member</code>.</td></tr>
<tr><td><code>COMMENT</code></td><td>Counter description.</td></tr>
</tbody></table>

## Enabling and disabling counters

Most of the counters are disabled by default. To enable them, use the [innodb_monitor_enable](/kb/en/innodb-system-variables/#innodb_monitor_enable) system variable. You can either enable a variable by its name, for example:

```sql
SET GLOBAL innodb_monitor_enable = icp_match;
```

or enable a number of counters grouped by module. The `SUBSYSTEM` field indicates which counters are grouped together, but the following module names need to be used:

<table><tbody><tr><th>Module Name</th><th>Subsytem Field</th></tr>
<tr><td><code>module_metadata</code></td><td><code>metadata</code></td></tr>
<tr><td><code>module_lock</code></td><td><code>lock</code></td></tr>
<tr><td><code>module_buffer</code></td><td><code>buffer</code></td></tr>
<tr><td><code>module_buf_page</code></td><td><code>buffer_page_io</code></td></tr>
<tr><td><code>module_os</code></td><td><code>os</code></td></tr>
<tr><td><code>module_trx</code></td><td><code>transaction</code></td></tr>
<tr><td><code>module_purge</code></td><td><code>purge</code></td></tr>
<tr><td><code>module_compress</code></td><td><code>compression</code></td></tr>
<tr><td><code>module_file</code></td><td><code>file_system</code></td></tr>
<tr><td><code>module_index</code></td><td><code>index</code></td></tr>
<tr><td><code>module_adaptive_hash</code></td><td><code>adaptive_hash_index</code></td></tr>
<tr><td><code>module_ibuf_system</code></td><td><code>change_buffer</code></td></tr>
<tr><td><code>module_srv</code></td><td><code>server</code></td></tr>
<tr><td><code>module_ddl</code></td><td><code>ddl</code></td></tr>
<tr><td><code>module_dml</code></td><td><code>dml</code></td></tr>
<tr><td><code>module_log</code></td><td><code>recovery</code></td></tr>
<tr><td><code>module_icp</code></td><td><code>icp</code></td></tr>
</tbody></table>

There are four counters in the `icp` subsystem:

```sql
SELECT NAME, SUBSYSTEM FROM INNODB_METRICS WHERE SUBSYSTEM='icp';
+------------------+-----------+
| NAME             | SUBSYSTEM |
+------------------+-----------+
| icp_attempts     | icp       |
| icp_no_match     | icp       |
| icp_out_of_range | icp       |
| icp_match        | icp       |
+------------------+-----------+
```

To enable them all, use the associated module name from the table above, `module_icp`.

```sql
SET GLOBAL innodb_monitor_enable = module_icp;
```

The `%` wildcard, used to represent any number of characters, can also be used when naming counters, for example:

```sql
SET GLOBAL innodb_monitor_enable = 'buffer%'
```

To disable counters, use the [innodb_monitor_disable](/kb/en/innodb-system-variables/#innodb_monitor_disable) system variable, using the same naming rules as described above for enabling.

Counter status is not persistent, and will be reset when the server restarts. It is possible to use the options on the command line, or the `innodb_monitor_enable` option only in a configuration file.

## Resetting counters

Counters can also be reset. Resetting sets all the `*_COUNT_RESET` values to zero, while leaving the `*_COUNT` values, which perform counts since the counter was enabled, untouched. Resetting is performed with the [innodb_monitor_reset](/kb/en/innodb-system-variables/#innodb_monitor_reset) (for individual counters) and [innodb_monitor_reset_all](/kb/en/innodb-system-variables/#innodb_monitor_reset_all) (for all counters) system variables.