# How to Quickly Insert Data Into MariaDB

This article describes different techniques for inserting data quickly into MariaDB.

## Background

When inserting new data into MariaDB, the things that take time are:
(in order of importance):

- Syncing data to disk (as part of the end of transactions)
- Adding new keys. The larger the index, the more time it takes to keep keys
  updated.
- Checking against foreign keys (if they exist).
- Adding rows to the storage engine.
- Sending data to the server.

The following describes the different techniques (again, in order of
importance) you can use to quickly insert data into a table.

## Disabling Keys

You can temporarily disable updating of non unique indexes. This is mostly
useful when there are zero (or very few) rows in the table into which you are
inserting data.

```sql
ALTER TABLE table_name DISABLE KEYS;
BEGIN;
... inserting data with INSERT or LOAD DATA ....
COMMIT;
ALTER TABLE table_name ENABLE KEYS;
```

In many storage engines (at least MyISAM and Aria),
`ENABLE KEYS` works by scanning through the row data and collecting keys,
sorting them, and then creating the index blocks. This is an order of magnitude
faster than creating the index one row at a time and it also uses less key
buffer memory.

<strong>Note:</strong> When you insert into an <strong>empty table</strong> with [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) or
[LOAD DATA](/kb/en/load-data-infile/), MariaDB <strong>automatically</strong> does an
[DISABLE KEYS](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) before and an [ENABLE KEYS](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/)
afterwards.

When inserting big amounts of data, integrity checks are sensibly time-consuming. It is possible to disable the `UNIQUE` indexes and the [foreign keys](/replication/optimization-and-tuning/optimization-and-indexes/foreign-keys/) checks using the [unique_checks](/kb/en/server-system-variables/#unique_checks) and the [foreign_key_checks](/kb/en/server-system-variables/#foreign_key_checks) system variables:

```sql
SET @@session.unique_checks = 0;
SET @@session.foreign_key_checks = 0;
```

For InnoDB tables, the [AUTO_INCREMENT lock mode](/columns-storage-engines-and-plugins/storage-engines/innodb/auto_increment-handling-in-innodb/) can be temporarily set to 2, which is the fastest setting:

```sql
SET @@global.innodb_autoinc_lock_mode = 2;
```

Also, if the table has [INSERT triggers](/programming-customizing-mariadb/triggers-events/triggers/) or [PERSISTENT](/kb/en/virtual-columns/) columns, you may want to drop them, insert all data, and recreate them.

## Loading Text Files

The <strong>fastest way</strong> to insert data into MariaDB is through the
[LOAD DATA INFILE](/kb/en/load-data-infile/) command.

The simplest form of the command is:

```sql
LOAD DATA INFILE 'file_name' INTO TABLE table_name;
```

You can also read a file locally on the machine where the client is running by
using:

```sql
LOAD DATA LOCAL INFILE 'file_name' INTO TABLE table_name;
```

This is not as fast as reading the file on the server side, but the difference
is not that big.

`LOAD DATA INFILE` is very fast because:

1 there is no parsing of SQL.
2 data is read in big blocks.
3 if the table is empty at the beginning of the operation, all non unique
  indexes are disabled during the operation.
4 the engine is told to cache rows first and then insert them in big blocks (At
  least MyISAM and Aria support this).
5 for empty tables, some transactional engines (like Aria) do not log the
  inserted data in the transaction log because one can rollback the operation
  by just doing a [TRUNCATE](/sql-statements-structure/sql-statements/table-statements/truncate-table/) on the table.

Because of the above speed advantages there are many cases, when you need to
insert <strong>many</strong> rows at a time, where it may be faster to create a file
locally, add the rows there, and then use `LOAD DATA INFILE` to load them;
compared to using `INSERT` to insert the rows.

You will also get [progress reporting](/kb/en/progress-reporting/) for
`LOAD DATA INFILE`.

### mariadb-import/mysqlimport

You can import many files in parallel with [mariadb-import](/clients-utilities/backup-restore-and-import-clients/mysqlimport/) (`mysqlimport` before [MariaDB 10.5](/kb/en/what-is-mariadb-105/)). For example:

```sql
mysqlimport --use-threads=10 database text-file-name [text-file-name...]
```

Internally [mariadb-import/mysqlimport](/clients-utilities/backup-restore-and-import-clients/mysqlimport/) uses [LOAD DATA INFILE](/kb/en/load-data-infile/) to read
in the data.

## Inserting Data with INSERT Statements

### Using Big Transactions

When doing many inserts in a row, you should wrap them with `BEGIN / END` to
avoid doing a full transaction (which includes a disk sync) for every row. For
example, doing a begin/end every 1000 inserts will speed up your inserts by
almost 1000 times.

```sql
BEGIN;
INSERT ...
INSERT ...
END;
BEGIN;
INSERT ...
INSERT ...
END;
...
```

The reason why you may want to have many `BEGIN/END` statements instead of
just one is that the former will use up less transaction log space.

### Multi-Value Inserts

You can insert many rows at once with multi-value row inserts:

```sql
INSERT INTO table_name values(1,"row 1"),(2, "row 2"),...;
```

The limit for how much data you can have in one statement is controlled by the
[max_allowed_packet](/kb/en/server-system-variables/#max_allowed_packet) server variable.

## Inserting Data Into Several Tables at Once

If you need to insert data into several tables at once, the best way to do so
is to enable multi-row statements and send many inserts to the server at once:

```sql
INSERT INTO table_name_1 (auto_increment_key, data) VALUES (NULL,"row 1");
INSERT INTO table_name_2 (auto_increment, reference, data) values (NULL, LAST_INSERT_ID(), "row 2");
```

[LAST_INSERT_ID()](/built-in-functions/secondary-functions/information-functions/last_insert_id/) is a function that returns the last
`auto_increment` value inserted.

By default, the command line `mysql` client will send the above as
multiple statements.

To test this in the `mysql` client you have to do:

```sql
delimiter ;;
select 1; select 2;;
delimiter ;
```

<strong>Note:</strong> for multi-query statements to work, your client must specify the
`CLIENT_MULTI_STATEMENTS` flag to `mysql_real_connect()`.

## Server Variables That Can be Used to Tune Insert Speed

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><a href="/kb/en/innodb-system-variables/#innodb_buffer_pool_size">innodb_buffer_pool_size</a></td><td>Increase this if you have many indexes in InnoDB/XtraDB tables</td></tr>
<tr><td><a href="/kb/en/myisam-system-variables/#key_buffer_size">key_buffer_size</a></td><td>Increase this if you have many indexes in MyISAM tables</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#max_allowed_packet">max_allowed_packet</a></td><td>Increase this to allow bigger multi-insert statements</td></tr>
<tr><td><a href="/kb/en/server-system-variables/#read_buffer_size">read_buffer_size</a></td><td>Read block size when reading a file with <code>LOAD DATA</code></td></tr>
</tbody></table>

See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for the full list of server
variables.