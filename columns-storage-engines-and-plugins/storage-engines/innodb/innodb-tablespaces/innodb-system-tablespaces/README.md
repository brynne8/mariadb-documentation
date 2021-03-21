# InnoDB System Tablespaces

When InnoDB needs to store general information relating to the system as a whole, rather than a specific table, the specific file it writes to is the system tablespace.  By default, this is the `ibdata1` file located in the data directory, (as defined by either the <a undefined>datadir</a> or <a undefined>innodb_data_home_dir</a> system variables).  InnoDB uses the system tablespace to store the data dictionary, change buffer, and undo logs.

You can define the system tablespace filename or filenames, size and other options by setting the <a undefined>innodb_data_file_path</a> system variable. This system variable can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariadb]
...
innodb_data_file_path=ibdata1:50M:autoextend
```

This system variable defaults to the file `ibdata1`, and it defaults to a minimum size of `12M`, and it defaults with the `autoextend` attribute enabled.

## Changing Sizes

InnoDB defaults to allocating 12M to the `ibdata1` file for the system tablespace.  While this is sufficient for most use cases, it may not be for all.  You may find after using MariaDB for a while that the allocation is too small for the system tablespace or it grows too large for your disk.  Fortunately, you can adjust this size as need later.

### Increasing the Size

When setting the <a undefined>innodb_data_file_path</a> system variable, you can define a size for each file given.  In cases where you need a larger system tablespace, add the `autoextend` option to the last value.

```sql
[mariadb]
...
innodb_data_file_path=ibdata1:12M;ibdata2:50M:autoextend
```

Under this configuration, when the last system tablespace grows beyond the size allocation, InnoDB increases the size of the file by increments.  To control the allocation increment, set the <a undefined>innodb_autoextend_increment</a> system variable.

### Decreasing the Size

In cases where the InnoDB system tablespace has grown too large, the process to reduce it in size is a little more complicated than increasing the size.  MariaDB does not allow you to remove data from the tablespace file itself.  Instead you need to delete the tablespace files themselves, then restore the database from backups.

The backup utility mysqldump produces backup files containing the SQL statements needed to recreate the database.  As a result, it restores a database with the bare minimum data rather than any additional information that might have built up in the tablespace file.

Use mysqldump to backup all of your InnoDB database tables, including the system tables in the `mysql` database that use InnoDB.  You can find out what they are using the Information Schema.

```sql
SELECT TABLE_NAME FROM information_schema.TABLES
WHERE TABLE_SCHEMA = 'mysql' AND ENGINE = 'InnoDB';
```

If you only use InnoDB, you may find it easier to back up all databases and tables.

```sql
$ mysqldump -u root -p --all-databases > full-backup.sql
```

Then stop the MariaDB Server and remove the InnoDB tablespace files.  In the data directory or the InnoDB data home directory, delete all the `ibdata` and `ib_log` files as well as any file with an `.ibd` or `.frm` extension.

Once this is done, restart the server and import the dump file:

```sql
$ mysql -u root -p < full-backup.sql
```

## Using Raw Disk Partitions

Instead of having InnoDB write to the file system, you can set it to use raw disk partitions.  On Windows and some Linux distributions, this allows you to perform non-buffered I/O without the file system overhead.  Note that in many use cases this may not actually improve performance.  Run tests to verify if there are any real gains for your application usage.

To enable a raw disk partition, first start MariaDB with the `newraw` option set on the tablespace.  For example:

```sql
[mariadb]
...
innodb_data_file_path=/dev/sdc:10Gnewraw
```

When the MariaDB Server starts, it initializes the partition.  Don't create or change any data, (any data written to InnoDB at this stage will be lost on restart).  Once the server has successful started, stop it then edit the configuration file again, changing the `newraw` keyword to `raw`.

```sql
[mariadb]
...
innodb_data_file_path=/dev/sdc:10Graw
```

When you start MariaDB again, it'll read and write InnoDB data to the given disk partition instead of the file system.

### Raw Disk Partitions on Windows

When defining a raw disk partition for InnoDB on the Windows operating system, use the same procedure as defined above, but when defining the path for the <a undefined>innodb_data_file_path</a> system variable, use `<em>./</em>` at the start. For example:

```sql
[mariadb]
...
innodb_data_file_path=//./E::10Graw
```

The given path is synonymous with the Windows syntax for accessing the physical drive.

## System Tables within the InnoDB System Tablespace

InnoDB creates some system tables within the InnoDB System Tablespace:

- `SYS_DATAFILES`
- `SYS_FOREIGN`
- `SYS_FOREIGN_COLS`
- `SYS_TABLESPACES`
- `SYS_VIRTUAL`
- `SYS_ZIP_DICT`
- `SYS_ZIP_DICT_COLS`

These tables cannot be queried. However, you might see references to them in some places, such as in the <a undefined>INNODB_SYS_TABLES</a> table in the [information_schema](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables) database.