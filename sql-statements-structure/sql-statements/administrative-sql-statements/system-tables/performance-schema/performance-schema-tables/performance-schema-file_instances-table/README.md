# Performance Schema file_instances Table

## Description

The `file_instances` table lists instances of instruments seen by the Performance Schema when executing file I/O instrumentation, and the associated files. Only files that have been opened, and that have not been deleted, will be listed in the table.

The [performance_schema_max_file_instances](/kb/en/performance-schema-system-variables/#performance_schema_max_file_instances) system variable specifies the maximum number of instrumented file objects.

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>FILE_NAME</code></td><td>File name.</td></tr>
<tr><td><code>EVENT_NAME</code></td><td>Instrument name associated with the file.</td></tr>
<tr><td><code>OPEN_COUNT</code></td><td>Open handles on the file. A value of greater than zero means that the file is currently open.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM performance_schema.file_instances WHERE OPEN_COUNT>0;
+----------------------------------------------------+--------------------------------------+------------+
| FILE_NAME                                          | EVENT_NAME                           | OPEN_COUNT |
+----------------------------------------------------+--------------------------------------+------------+
| /var/log/mysql/mariadb-bin.index                   | wait/io/file/sql/binlog_index        |          1 |
| /var/lib/mysql/ibdata1                             | wait/io/file/innodb/innodb_data_file |          2 |
| /var/lib/mysql/ib_logfile0                         | wait/io/file/innodb/innodb_log_file  |          2 |
| /var/lib/mysql/ib_logfile1                         | wait/io/file/innodb/innodb_log_file  |          2 |
| /var/lib/mysql/mysql/gtid_slave_pos.ibd            | wait/io/file/innodb/innodb_data_file |          3 |
| /var/lib/mysql/mysql/innodb_index_stats.ibd        | wait/io/file/innodb/innodb_data_file |          3 |
| /var/lib/mysql/mysql/innodb_table_stats.ibd        | wait/io/file/innodb/innodb_data_file |          3 |
...
```