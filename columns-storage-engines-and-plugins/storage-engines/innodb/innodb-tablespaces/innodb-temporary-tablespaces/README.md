# InnoDB Temporary Tablespaces

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

The use of the temporary tablespaces in InnoDB was introduced in [MariaDB 10.2](/kb/en/what-is-mariadb-102/).  In earlier versions, temporary tablespaces exist as part of the InnoDB [system](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-tablespaces/innodb-system-tablespaces) tablespace or were file-per-table depending on the configuration of the <a undefined>innodb_file_per_table</a> system variable.

When the user creates a temporary table using the [CREATE TEMPORARY TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) statement and the engine is set as InnoDB, MariaDB creates a temporary tablespace file.  When the table is not compressed, MariaDB writes to a shared temporary tablespace as defined by the <a undefined>innodb_temp_data_file_path</a> system variable.  When compressed, the temporary table is written to a dedicated temporary tablespace for that table.  MariaDB deletes temporary tablespaces when the server shuts down gracefully and is recreated when it starts again.  It cannot be placed on a raw device.

Internal temporary tablespaces, (that is, temporary tables that cannot be kept in memory) use either Aria or MyISAM, depending on the <a undefined>aria_used_for_temp_tables</a> system variable.  You can set the default storage engine for user-created temporary tables using the <a undefined>default_tmp_storage_engine</a> system variable.

## Sizing Temporary Tablespaces

In order to size temporary tablespaces, use the <a undefined>innodb_temp_data_file_path</a> system variable. This system variable can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariadb]
...
innodb_temp_data_file_path=ibtmp1:32M:autoextend
```

This system variable's syntax is the same as the <a undefined>innodb_data_file_path</a> system variable.  That is, a file name, size and option.  By default, it writes a 12MB autoextending file to `ibtmp1` in the data directory.

To see the current size of the temporary tablespace from MariaDB, query the Information Schema:

```sql
SELECT FILE_NAME AS "File Name"
   INITIAL_SIZE AS "Initial Size",
   DATA_FREE AS "Free Space",
   TOTAL_EXTENTS * EXTENT_SIZE AS "Total Size",
   MAXIMUM_SIZE AS "Max"
FROM information_Schema.FILES
WHERE TABLESPACE_NAME = "innodb_temporary";

+-----------+--------------+------------+------------+------+
| File Name | Initial Size | Free Space | Total Size | Max  |
+-----------+--------------+------------+------------+------+
| ./ibtmp1  |     12582912 |     621456 |   12592912 | NULL |
+-----------+--------------+------------+------------+------+
```

To increase the size of the temporary tablespace, you can add a path to an additional tablespace file to the value of the the <a undefined>innodb_temp_data_file_path</a> system variable. Providing additional paths allows you to spread the temporary tablespace between multiple tablespace files. The last file can have the `autoextend` attribute, which ensures that you won't run out of space. For example:

```sql
[mariadb]
...
innodb_temp_data_file_path=ibtmp1:32M;ibtmp2:32M:autoextend
```

Unlike normal tablespaces, temporary tablespaces are deleted when you stop MariaDB.  To shrink temporary tablespaces to their minimum sizes, restart the server.