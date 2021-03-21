# Moving Data Between SQL Server and MariaDB

There are several ways to move data between SQL Server and MariaDB. Here we will discuss them and we will highlight some caveats.

## Moving Data Definition from SQL Server to MariaDB

To copy SQL Server data structures to MariaDB, one has to:

1 Generate a CSV file from SQL Server data.
2 Modify the syntax so that it works in MariaDB.
3 Run the file in MariaDB.

### Variables That Affect DDL Statements

DDL statements are affected by some server system variables.

[sql_mode](/mariadb-administration/variables-and-modes/sql-mode/) determines the behavior of some SQL statements and expressions, including how strict error checking is, and some details regarding the syntax. Objects like [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures/), [stored functions](/programming-customizing-mariadb/stored-routines/stored-functions/) [triggers](/programming-customizing-mariadb/triggers-events/triggers/) and [views](/programming-customizing-mariadb/views/), are always executed with the sql_mode that was in effect during their creation. [sql_mode='MSSQL'](/kb/en/sql_modemssql/) can be used to have MariaDB behaving as close to SQL Server as possible.

[innodb_strict_mode](/kb/en/innodb-system-variables/#innodb_strict_mode) enables the so-called InnoDB strict mode. Normally some errors in the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) options are ignored. When InnoDB strict mode is enabled, the creation of InnoDB tables will fail with an error when certain mistakes are made.

[updatable_views_with_limit](/kb/en/server-system-variables/#updatable_views_with_limit) determines whether view updates can be made with an [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) or [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) statement with a `LIMIT` clause if the view does not contain all primary or not null unique key columns from the underlying table.

### Dumps and sys.sql_modules

SQL Server Management Studio allows one to create a working SQL script to recreate a database - something that MariaDB users refer to as a <em>dump</em>. Several options allow fine-tuning the generated syntax. It could be necessary to adjust some of these options to make the output compatible with MariaDB. It is possible to export schemas, data or both. One can create a single global file, or one file for each exported object. Normally, producing a single file is more practical.

Alternatively, the [sp_helptext()](https://docs.microsoft.com/en-us/sql/relational-databases/system-stored-procedures/sp-helptext-transact-sql) procedure returns information about how to recreate a certain object. Similar information is also present in the [sql_modules](https://docs.microsoft.com/en-us/sql/relational-databases/system-catalog-views/sys-sql-modules-transact-sql) table (`definition` column), in the `sys` schema. Such information, however, is not a ready-to-use set of SQL statements.

Remember however that [MariaDB does not support schemas](/kb/en/understanding-mariadb-architecture/#databases). An SQL Server schema is approximately a MariaDB database.

To execute a dump, we can pass the file to [mysql](/clients-utilities/mysql-client/mysql-command-line-client/), the MariaDB command-line client.

Provided that a dump file contains syntax that is valid in MariaDB, it can be executed in this way:

```sql
mysql --show-warnings < dump.sql
```

`--show-warnings` tells MariaDB to output any warnings produced by the statements contained in the dump. Without this option, warnings will not appear on screen. Warnings don't stop the dump execution.

Errors will appear on screen. Errors will stop the dump execution, unless the `--force` option (or just `-f`) is specified.

For other `mysql` options, see [mysql Command-line Client Options](/kb/en/mysql-command-line-client/#options).

Another way to achieve the same purpose is to start the `mysql` client in interactive mode first, and then run the `source` command. For example:

```sql
root@d5a54a082d1b:/# mysql -uroot -psecret
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 22
Server version: 10.4.7-MariaDB-1:10.4.7+maria~bionic mariadb.org binary distribution

Copyright (c) 2000, 2018, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> \W
Show warnings enabled.
MariaDB [(none)]> source dump.sql
```

In this case, to show warnings we used the `\W` command, where "w" is uppercase. To hide warnings (which is the default), we can use `\w` (lowercase).

For other `mysql` commands, see [mysql Commands](/kb/en/mysql-command-line-client/#mysql-commands).

### CSV Data

If the table structures are already in MariaDB, we need only to import table data. While this can still be done as explained above, it may be more practical to export CSV files from SQL Server and import them into MariaDB.

SQL Server Management Studio and several other Microsoft tools allow one to export CSV files.

MariaDB allows importing CSV files with the [LOAD DATA INFILE](/kb/en/load-data-infile/) statement, which is essentially the MariaDB equivalent of `BULK INSERT`.

It can happen that we don't want to import the whole data, but some filtered or transformed version of it. In that case, we may prefer to use the [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) storage engine to access CSV files and query them. The results of a query can be inserted into a table using [INSERT SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select/).

## Moving Data from MariaDB to SQL Server

There are several ways to move data from MariaDB to SQL Server:

- If the tables don't exist at all in SQL Server, we need to generate a dump first. The dump can include data or not.
- If the tables are already in SQL Server, we can use CSV files instead of dumps to move the rows. CSV files are the most concise format to move data between different technologies.
- With the tables already in SQL Server, another way to move data is to insert the rows into [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) tables that "point" to remote SQL Server tables.

### Using a Dump (Structure)

[mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) can be used to generate dumps of all databases, a specified database, or a set of tables. It is even possible to only dump a set of rows by specifying the `WHERE` clause.

By specifying the `--no-data` option we can dump the table structures without data.

`--compatible=mssql` will produce an output that should be usable in SQL Server.

### Using a Dump (Data)

mysqldump by default produces an output with both data and structure.

`--no-create-info` can be used to skip the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) statements.

`--compatible=mssql` will produce an output that should be usable in SQL Server.

`--single-transaction` should be specified to select the source data in a single transaction, so that a consistent dump is produced.

`--quick` speeds up the dump process when dumping big tables.

### Using a CSV File

CSV files can also be used to export data to SQL Server. There are several ways to produce CSV files from MariaDB:

- The [SELECT INTO OUTFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-outfile/) statement.
- The [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) storage engine, with the [CSV table type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-csv-and-fmt-table-types/).
- The [CSV](/columns-storage-engines-and-plugins/storage-engines/csv/) storage engine (note that it doesn't support `NULL` and indexes).

### Using CONNECT Tables

The [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) storage engine allows one to access external data, in many forms:

- [Data files](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-table-types-data-files/) ([CSV](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-csv-and-fmt-table-types/), [JSON](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-json-table-type/), [XML](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-xml-table-type/), HTML and more).
- Remote databases, using the [ODBC](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-odbc-table-type-accessing-tables-from-another-dbms/) or [JDBC](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-jdbc-table-type-accessing-tables-from-another-dbms/) standards, or [MariaDB/MySQL native protocol](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-mysql-table-type-accessing-mysqlmariadb-tables/).
- Some [special data sources](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-table-types-special-virtual-tables/).

`CONNECT` was mentioned previously because it could allow one to read a CSV file and query it in SQL, filtering and transforming the data that we want to move into regular MariaDB tables.

However, `CONNECT` can also access remote SQL Server tables. We can read data from it, or even write data.

To enable `CONNECT` to work with SQL Server, we need to fulfill these requirements:

- Install the ODBC driver, downloadable form [Microsoft](https://microsoft.com/) website. The driver is also available for Linux and MacOS.
- Install [unixODBC](http://www.unixodbc.org/).
- [Install `CONNECT`](/columns-storage-engines-and-plugins/storage-engines/connect/installing-the-connect-storage-engine/) (unless it is already installed).

Here is an example of a `CONNECT` table that points to a SQL Server table:

```sql
CREATE TABLE city (
    id INT PRIMARY KEY,
    city_name VARCHAR(100),
    province_id INT NOT NULL
)
    ENGINE=CONNECT,
    TABLE_TYPE=ODBC,
    TABNAME='city'
    CONNECTION='Driver=SQL Server Native Client 13.0;Server=sql-server-hostname;Database=world;UID=mariadb_connect;PWD=secret';
```

The key points here are:

- `ENGINE=CONNECT` tells MariaDB that we want to create a `CONNECT` table.
- `TABLE_TYPE` must be 'ODBC', so `CONNECT` knows what type of data source it has to use.
- `CONNECTION` is the connection string to use, including server address, username and password.
- `TABNAME` tells `CONNECT` what the remote table is called. The local name could be different.

`CONNECT` is able to query SQL Server to find out the remote table structure. We can use this feature to avoid specifying the column names and types:

```sql
CREATE TABLE city
    ENGINE=CONNECT,
    TABLE_TYPE=ODBC,
    TABNAME='city'
    CONNECTION='Driver=SQL Server Native Client 13.0;Server=sql-server-hostname;Database=world;UID=mariadb_connect;PWD=secret';
```

However, we may prefer to manually specify the MariaDB types, sizes and character sets to use.

### Linked Server

Instead of using MariaDB `CONNECT`, it is possible to use SQL Server Linked Server functionality. This will allow one to read data from a remote MariaDB database and copy it into local SQL Server tables. However, note that `CONNECT` allows more control on [types and character sets](/mariadb-administration/getting-installing-and-upgrading-mariadb/migrating-from-sql-server-to-mariadb/sql-server-and-mariadb-types-comparison/) mapping.

Refer to [Linked Servers](https://docs.microsoft.com/en-us/sql/relational-databases/linked-servers/linked-servers-database-engine?view=sql-server-ver15) section in Microsoft documentation.