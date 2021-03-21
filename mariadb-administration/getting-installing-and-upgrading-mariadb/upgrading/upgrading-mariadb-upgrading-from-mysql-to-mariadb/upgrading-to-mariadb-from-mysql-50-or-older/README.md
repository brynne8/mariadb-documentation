# Upgrading to MariaDB From MySQL 5.0 or Older

If you upgrade to [MariaDB 5.1](/kb/en/what-is-mariadb-51/) from MySQL 5.1 you [don't have to do anything](/kb/en/how-can-i-upgrade-from-mysql-to-mariadb/) with your data or MySQL clients. Things should "just work".

When upgrading between different major versions of MariaDB or MySQL you need to
run the [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade) program to convert data that are incompatible between versions. This will also update your privilege tables in the mysql database to the latest format.

In almost all cases [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade) should be able to convert your tables, without you having to dump and restore your data.

After installing MariaDB, just do:

```sql
mysql_upgrade --verbose
```

If you want to run with a specific TCP/IP port do:

```sql
mysql_upgrade --host=127.0.0.1 --port=3308 --protocol=tcp
```

If you want to connect with a socket do:

```sql
mysql_upgrade --socket=127.0.0.1 --protocol=socket
```

To see other options, use [--help](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade).

"mysql_upgrade" reads the [my.cnf](/mariadb-administration/getting-installing-and-upgrading-mariadb/mysqld-configuration-files-and-groups) sections [mysql_upgrade] and [client] for default values.

There are a variety of reasons tables need to be converted; they could be any of the following:

- The collation (sorting order) for an index column has changed
- A field type has changed storage format
<ul start="1"><li>[DECIMAL](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/decimal) and [VARCHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar) changed format between MySQL 4.1 and MySQL 5.0
</li></ul>
- An engine has a new storage format
<ul start="1"><li>[ARCHIVE](/columns-storage-engines-and-plugins/storage-engines/archive) changed storage format between 5.0 and 5.1
</li></ul>
- The format for storing [table names](/sql-statements-structure/sql-language-structure/identifier-names) has changed
<ul start="1"><li>In MySQL 5.1 table names are encoded so that the file names are identical on all computers.  Old table names that contains forbidden file name characters will show up prefixed with #mysql50# in `SHOW TABLES` until you convert them.
</li></ul>

If you don't convert the tables, one of the following things may happen:

- You will get warnings in the error log every time you access a table with an invalid (old) file name.
- When searching on key values you may not find all rows
- You will get an error "ERROR 1459 (HY000): Table upgrade required" when accessing the table.
- You may get crashes

"mysql_upgrade" works by calling mysqlcheck with different options and running the "mysql_fix_privileges" script. If you have trouble with "mysql_upgrade", you can run these commands separately to get more information of what is going on.

Most of the things in the [MySQL 5.1 manual](http://dev.mysql.com/doc/refman/5.1/en/upgrading.html) section also applies to MariaDB.

The following differences exists between "mysql_upgrade" in MariaDB and MySQL (as of [MariaDB 5.1.50](/kb/en/mariadb-5150-release-notes/)):

- MariaDB will convert long table names properly.
- MariaDB will convert [InnoDB](/kb/en/xtradb-and-innodb/) tables (no need to do a dump/restore or [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table)).
- MariaDB will convert old archive tables to the new 5.1 format (note: new feature in testing).
- "mysql_upgrade --verbose" will run "mysqlcheck --verbose" so that you get more information of what is happening.