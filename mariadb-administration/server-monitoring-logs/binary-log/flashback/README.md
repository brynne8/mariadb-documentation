# Flashback

##### MariaDB starting with [10.2.4](/kb/en/mariadb-1024-release-notes/)

DML-only flashback was introduced in [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)

Flashback is a feature that will allow instances, databases or tables to be rolled back to an old snapshot.

Flashback is currently supported only over DML statements ([INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert), [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete), [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update)). An upcoming version of MariaDB will add support for flashback over DDL statements ([DROP](/sql-statements-structure/sql-statements/data-definition/drop/drop-table), [TRUNCATE](/sql-statements-structure/sql-statements/table-statements/truncate-table), [ALTER](/sql-statements-structure/sql-statements/data-definition/alter/alter-table), etc.) by copying or moving the current table to a reserved and hidden database, and then copying or moving back when using flashback. See [MDEV-10571](https://jira.mariadb.org/browse/MDEV-10571).

Flashback is achieved in MariaDB Server using existing support for full image format binary logs (`binlog_row_image=FULL`), so it supports all engines.

The real work of Flashback is done by [mysqlbinlog](/clients-utilities/mysqlbinlog) with `--flashback`. This causes events to be translated: INSERT to DELETE, DELETE to INSERT, and for UPDATEs the before and after images are swapped.

When executing `mysqlbinlog` with `--flashback`, the Flashback events will be stored in memory. You should make sure your server has enough memory for this feature.

## New Arguments

- [mysqlbinlog](/clients-utilities/mysqlbinlog) has a new option: `--flashback` or `-B` that will let it work in flashback mode.
- [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) has a new option: [--flashback](/kb/en/mysqld-options/#-flashback) that enables the binary log and sets `binlog_format=ROW`. It is not mandatory to use this option if you have already enabled those options directly.

Do not use `-v` `-vv` options, as this adds verbose information to the binary log which can cause problems when importing. See [MDEV-12066](https://jira.mariadb.org/browse/MDEV-12066) and [MDEV-12067](https://jira.mariadb.org/browse/MDEV-12067).

## Example

With a table "mytable" in database "test", you can compare the output with `--flashback` and without.

```sql
 mysqlbinlog /var/lib/mysql/mysql-bin.000001 -vv -d test -T mytable \
    --start-datetime="2013-03-27 14:54:00" > review.sql
```

```sql
 mysqlbinlog /var/lib/mysql/mysql-bin.000001 -vv -d test -T mytable \
    --start-datetime="2013-03-27 14:54:00" --flashback > flashback.sql
```

If you know the exact position, `--start-position` can be used instead of `--start-datetime`.

Then, by importing the output file (`mysql &lt; flashback.sql`), you can flash your database/table back to the specified time or position.

## Common Use Case

A common use case for Flashback is the following scenario:

- You have one master and two slaves, one started with `--flashback` (i.e. with binary logging enabled, using `binlog_format=ROW`, and `binlog_row_image=FULL`).
- Something goes wrong on the master (like a wrong update or delete) and you would like to revert to a state of the database (or just a table) at a certain point in time.
- Remove the flashback-enabled slave from replication.
- Invoke `mysqlbinlog` to find the exact log position of the first offending operation after the state you want to revert to.
- Run `mysqlbinlog --flashback --start-position=xyz | mysql` to pipe the output of mysqlbinlog directly to the `mysql` client, or save the output to a file and then direct the file to the command-line client.