# Partitions Files

A partitioned table is stored in multiple files. By default, these files are stored in the MariaDB (or InnoDB) data directory. It is possible to keep them in different paths by specifying [DATA_DIRECTORY and INDEX_DIRECTORY](/kb/en/create-table/#data-directoryindex-directory) table options. This is useful to store different partitions on different devices.

Note that, if the [innodb_file_per_table](/kb/en/xtradbinnodb-server-system-variables/#innodb_file_per_table) server system variable is set to 0 at the time of the table creation, all partitions will be stored in the system tablespace.

The following files exist for each partitioned tables:

<table><tbody><tr><th>File name</th><th>Notes</th></tr>
<tr><td>table_name.frm</td><td>Contains the table definition. Non-partitioned tables have this file, too.</td></tr>
<tr><td>table_name.par</td><td>Contains the partitions definitions.</td></tr>
<tr><td>table_name#P#partition_name.ext</td><td>Normal files created by the storage engine use this pattern for names. The extension depends on the storage engine.</td></tr>
</tbody></table>

For example, an InnoDB table with 4 partitions will have the following files:

```sql
orders.frm
orders.par
orders#P#p0.ibd
orders#P#p1.ibd
orders#P#p2.ibd
orders#P#p3.ibd
```

If we convert the table to MyISAM, we will have these files:

```sql
orders.frm
orders.par
orders#P#p0.MYD
orders#P#p0.MYI
orders#P#p1.MYD
orders#P#p1.MYI
orders#P#p2.MYD
orders#P#p2.MYI
orders#P#p3.MYD
orders#P#p3.MYI
```