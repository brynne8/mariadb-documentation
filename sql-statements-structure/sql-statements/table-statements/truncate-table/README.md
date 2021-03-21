# TRUNCATE TABLE

## Syntax

```sql
TRUNCATE [TABLE] tbl_name
  [WAIT n | NOWAIT]
```

## Description

`TRUNCATE TABLE` empties a table completely. It requires the `DROP` privilege. See [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant).

`tbl_name` can also be specified in the form `db_name`.`tbl_name` (see [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers)).

Logically, `TRUNCATE TABLE` is equivalent to a [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) statement that deletes all rows, but there are practical differences under some circumstances.

`TRUNCATE TABLE` will fail for an [InnoDB table](/columns-storage-engines-and-plugins/storage-engines/innodb) if any FOREIGN KEY constraints from other tables reference the table, returning the error:

```sql
ERROR 1701 (42000): Cannot truncate a table referenced in a foreign key constraint
```

Foreign Key constraints between columns in the same table are permitted.

For an InnoDB table, if there are no `FOREIGN KEY` constraints, InnoDB performs fast truncation by dropping the original table and creating an empty one with the same definition, which is much faster than deleting rows one by one. The [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment) counter is reset by `TRUNCATE TABLE`, regardless of whether there is a `FOREIGN KEY` constraint.

The count of rows affected by `TRUNCATE TABLE` is accurate only
when it is mapped to a `DELETE` statement.

For other storage engines, `TRUNCATE TABLE` differs from
`DELETE` in the following ways:

- Truncate operations drop and re-create the table, which is much
  faster than deleting rows one by one, particularly for large tables.
- Truncate operations cause an implicit commit.
- Truncation operations cannot be performed if the session holds an
  active table lock.
- Truncation operations do not return a meaningful value for the number
  of deleted rows. The usual result is "0 rows affected," which should
  be interpreted as "no information."
- As long as the table format file `tbl_name.frm` is valid, the
  table can be re-created as an empty table
  with `TRUNCATE TABLE`, even if the data or index files have become
  corrupted.
- The table handler does not remember the last
  used [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment) value, but starts counting
  from the beginning. This is true even for MyISAM and InnoDB, which normally
  do not reuse sequence values.
- When used with partitioned tables, `TRUNCATE TABLE` preserves
  the partitioning; that is, the data and index files are dropped and
  re-created, while the partition definitions (.par) file is
  unaffected.
- Since truncation of a table does not make any use of `DELETE`,
  the `TRUNCATE` statement does not invoke `ON DELETE` triggers.
- `TRUNCATE TABLE` will only reset the values in the [Performance Schema summary tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/list-of-performance-schema-tables) to zero or null, and will not remove the rows.

For the purposes of binary logging and [replication](/replication), `TRUNCATE TABLE` is treated as [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table) followed by [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) (DDL rather than DML).

`TRUNCATE TABLE` does not work on [views](/programming-customizing-mariadb/views). Currently, `TRUNCATE TABLE` drops all historical records from a [system-versioned table](/sql-statements-structure/temporal-tables/system-versioned-tables).

##### MariaDB starting with [10.3.0](/kb/en/mariadb-1030-release-notes/)

#### WAIT/NOWAIT

Set the lock wait timeout. See [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait).

### Oracle-mode

[Oracle-mode](/kb/en/sql_modeoracle/) from [MariaDB 10.3](/kb/en/what-is-mariadb-103/) permits the optional keywords REUSE STORAGE or DROP STORAGE to be used.

```sql
TRUNCATE [TABLE] tbl_name [{DROP | REUSE} STORAGE]
```

These have no effect on the operation.

### Performance

`TRUNCATE TABLE` is faster than [DELETE](delete-table), because it drops and re-creates a table.

With [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb), `TRUNCATE TABLE` is slower if [innodb_file_per_table=ON](/kb/en/innodb-system-variables/#innodb_file_per_table) is set (the default). This is because `TRUNCATE TABLE` unlinks the underlying tablespace file, which can be an expensive operation. See [MDEV-8069](https://jira.mariadb.org/browse/MDEV-8069) for more details.

The performance issues with [innodb_file_per_table=ON](/kb/en/innodb-system-variables/#innodb_file_per_table) can be exacerbated in cases where the [InnoDB buffer pool](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-buffer-pool) is very large and [innodb_adaptive_hash_index=ON](/kb/en/innodb-system-variables/#innodb_adaptive_hash_index) is set. In that case, using [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table) followed by [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) instead of `TRUNCATE TABLE` may perform better. Setting [innodb_adaptive_hash_index=OFF](/kb/en/innodb-system-variables/#innodb_adaptive_hash_index) (it defaults to ON before [MariaDB 10.5](/kb/en/what-is-mariadb-105/)) can also help. In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) only, from [MariaDB 10.2.19](/kb/en/mariadb-10219-release-notes/), this performance can also be improved by setting [innodb_safe_truncate=OFF](/kb/en/innodb-system-variables/#innodb_safe_truncate). See [MDEV-9459](https://jira.mariadb.org/browse/MDEV-9459) for more details.

Setting [innodb_adaptive_hash_index=OFF](/kb/en/innodb-system-variables/#innodb_adaptive_hash_index) can also improve `TRUNCATE TABLE` performance in general. See [MDEV-16796](https://jira.mariadb.org/browse/MDEV-16796) for more details.

## See Also

- [TRUNCATE function](/built-in-functions/numeric-functions/truncate)
- [innodb_safe_truncate](/kb/en/innodb-system-variables/#innodb_safe_truncate) system variable
- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#simple-syntax-compatibility)