# LOAD DATA INFILE

## Syntax

```sql
LOAD DATA [LOW_PRIORITY | CONCURRENT] [LOCAL] INFILE 'file_name'
    [REPLACE | IGNORE]
    INTO TABLE tbl_name
    [CHARACTER SET charset_name]
    [{FIELDS | COLUMNS}
        [TERMINATED BY 'string']
        [[OPTIONALLY] ENCLOSED BY 'char']
        [ESCAPED BY 'char']
    ]
    [LINES
        [STARTING BY 'string']
        [TERMINATED BY 'string']
    ]
    [IGNORE number LINES]
    [(col_name_or_user_var,...)]
    [SET col_name = expr,...]
```

## Description

`LOAD DATA INFILE` is [unsafe](/kb/en/unsafe-statements-for-replication/) for statement-based replication.

Reads rows from a text file into the designated table on the database at a very high speed. The file name must be given as a literal string.

Files are written to disk using the [SELECT INTO OUTFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-outfile/) statement.  You can then read the files back into a table using the `LOAD DATA INFILE` statement.  The `FIELDS` and `LINES` clauses are the same in both statements.  These clauses are optional, but if both are specified then the `FIELDS` clause must precede `LINES`.

Executing this statement activates `INSERT` [triggers](/programming-customizing-mariadb/triggers-events/triggers/).

One must have the [FILE](/kb/en/grant/#file) privilege to be able to execute LOAD DATA. This is the ensure the normal users will not attempt to read system files.

### `LOAD DATA LOCAL INFILE`

When you execute the `LOAD DATA INFILE` statement, MariaDB Server attempts to read the input file from its own file system. In contrast, when you execute the `LOAD DATA LOCAL INFILE` statement, the client attempts to read the input file from its file system, and it sends the contents of the input file to the MariaDB Server. This allows you to load files from the client's local file system into the database.

In the event that you don't want to permit this operation (such as for security reasons), you can disable the `LOAD DATA LOCAL INFILE` statement on either the server or the client.

- The `LOAD DATA LOCAL INFILE` statement can be disabled on the server by setting the [local_infile](/kb/en/server-system-variables/#local_infile) system variable to `0`.
- The `LOAD DATA LOCAL INFILE` statement can be disabled on the client. If you are using [MariaDB Connector/C](/kb/en/about-mariadb-connector-c/), this can be done by unsetting the `CLIENT_LOCAL_FILES` capability flag with the [mysql_real_connect](/kb/en/mysql_real_connect/) function or by unsetting the `MYSQL_OPT_LOCAL_INFILE` option with [mysql_optionsv](/kb/en/mysql_optionsv/) function. If you are using a different client or client library, then see the documentation for your specific client or client library to determine how it handles the `LOAD DATA LOCAL INFILE` statement.

If the `LOAD DATA LOCAL INFILE` statement is disabled by either the server or the client and if the user attempts to execute it, then the server will cause the statement to fail with the following error message:

```sql
The used command is not allowed with this MariaDB version
```

Note that it is not entirely accurate to say that the MariaDB version does not support the command. It would be more accurate to say that the MariaDB configuration does not support the command. See [MDEV-20500](https://jira.mariadb.org/browse/MDEV-20500) for more information.

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the error message is more accurate:

```sql
The used command is not allowed because the MariaDB server or client 
  has disabled the local infile capability
```

### `REPLACE` and `IGNORE`

In cases where you load data from a file into a table that already contains data and has a [primary key](/kb/en/getting-started-with-indexes/#primary-key), you may encounter issues where the statement attempts to insert a row with a primary key that already exists. When this happens, the statement fails with Error 1064, protecting the data already on the table. In cases where you want MariaDB to overwrite duplicates, use the `REPLACE` keyword.

The `REPLACE` keyword works like the [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace/) statement. Here, the statement attempts to load the data from the file. If the row does not exist, it adds it to the table.  If the row contains an existing Primary Key, it replaces the table data. That is, in the event of a conflict, it assumes the file contains the desired row.

This operation can cause a degradation in load speed by a factor of 20 or more if the part that has already been loaded is larger than the capacity of the [InnoDB Buffer Pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool/).  This happens because it causes a lot of turnaround in the buffer pool.

Use the `IGNORE` keyword when you want to skip any rows that contain a conflicting primary key. Here, the statement attempts to load the data from the file. If the row does not exist, it adds it to the table. If the row contains an existing primary key, it ignores the addition request and moves on to the next. That is, in the event of a conflict, it assumes the table contains the desired row.

### Character-sets

When the statement opens the file, it attempts to read the contents using the default character-set, as defined by the [character_set_database](/kb/en/server-system-variables/#character_set_database) system variable.

In the cases where the file was written using a character-set other than the default, you can specify the character-set to use with the `CHARACTER SET` clause in the statement.  It ignores character-sets specified by the [SET NAMES](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/set-names/) statement and by the [character_set_client](/kb/en/server-system-variables/#character_set_client) system variable.  Setting the `CHARACTER SET` clause to a value of `binary` indicates "no conversion."

The statement interprets all fields in the file as having the same character-set, regardless of the column data type.  To properly interpret file contents, you must ensure that it was written with the correct character-set.  If you write a data file with [mysqldump -T](/clients-utilities/backup-restore-and-import-clients/mysqldump/) or with the [SELECT INTO OUTFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-outfile/) statement with the [mysql](/clients-utilities/mysql-client/mysql-command-line-client/) client, be sure to use the `--default-character-set` option, so that the output is written with the desired character-set.

When using mixed character sets, use the `CHARACTER SET` clause in both [SELECT INTO OUTFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-outfile/) and `LOAD DATA INFILE` to ensure that MariaDB correctly interprets the escape sequences.

The [character_set_filesystem](/kb/en/server-system-variables/#character_set_filesystem) system variable controls the interpretation of the filename.

It is currently not possible to load data files that use the `ucs2` character set.

### Priority and Concurrency

In storage engines that perform table-level locking ([MyISAM](/kb/en/myisam/), [MEMORY](/kb/en/memory/) and [MERGE](/columns-storage-engines-and-plugins/storage-engines/merge/)), using the [LOW_PRIORITY](/kb/en/high_priority-and-low_priority-clauses/) keyword, MariaDB delays insertions until no other clients are reading from the table. Alternatively, when using the [MyISAM](/kb/en/myisam/) storage engine, you can use the [CONCURRENT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts/) keyword to perform concurrent insertion.

The `LOW_PRIORITY` and `CONCURRENT` keywords are mutually exclusive.  They cannot be used in the same statement.

### Progress Reporting

The `LOAD DATA INFILE` statement supports [progress reporting](/kb/en/progress-reporting/). You may find this useful when dealing with long-running operations. Using another client you can issue a [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/) query to check the progress of the data load.

### Using mariadb-import/mysqlimport

MariaDB ships with a separate utility for loading data from files: [mariadb-import](/clients-utilities/backup-restore-and-import-clients/mysqlimport/) (or `mysqlimport` before [MariaDB 10.5](/kb/en/what-is-mariadb-105/)). It operates by sending `LOAD DATA INFILE` statements to the server.

Using [mariadb-import/mysqlimport](/clients-utilities/backup-restore-and-import-clients/mysqlimport/) you can compress the file using the `--compress` option, to get better performance over slow networks, providing both the client and server support the compressed protocol.  Use the `--local` option to load from the local file system.

### Indexing

In cases where the storage engine supports [ALTER TABLE... DISABLE KEYS](/kb/en/alter-table/#enable-disable-keys) statements ([MyISAM](/kb/en/myisam/) and [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/)), the `LOAD DATA INFILE` statement automatically disables indexes during the execution.

## Examples

You have a file with this content (note the the separator is ',', not tab, which is the default):

```sql
2,2
3,3
4,4
5,5
6,8
```

```sql
CREATE TABLE t1 (a int, b int, c int, d int);
LOAD DATA LOCAL INFILE 
 '/tmp/loaddata7.dat' into table t1 fields terminated by ',' (a,b) set c=a+b;
SELECT * FROM t1;
```

```sql
+------+------+------+
| a    | b    | c    |
+------+------+------+
|    2 |    2 |    4 |
|    3 |    3 |    6 |
|    4 |    4 |    8 |
|    5 |    5 |   10 |
|    6 |    8 |   14 |
+------+------+------+
```

## See Also

- [How to quickly insert data into MariaDB](/replication/optimization-and-tuning/query-optimizations/how-to-quickly-insert-data-into-mariadb/)
- [Character Sets and Collations](/kb/en/character-sets-and-collations/)
- [SELECT ... INTO OUTFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-outfile/)
- [mariadb-import/mysqlimport](/clients-utilities/backup-restore-and-import-clients/mysqlimport/)