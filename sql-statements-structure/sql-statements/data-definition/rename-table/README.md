# RENAME TABLE

## Syntax

```sql
RENAME TABLE [IF EXISTS] tbl_name 
  [WAIT n | NOWAIT]
  TO new_tbl_name
    [, tbl_name2 TO new_tbl_name2] ...
```

## Description

This statement renames one or more tables or [views](/programming-customizing-mariadb/views), but not the privileges associated with them.

### IF EXISTS

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

If this directive is used, one will not get an error if the table to be renamed doesn't exist.

The rename operation is done atomically, which means that no other session can
access any of the tables while the rename is running. For example, if you have
an existing table <code class="fixed" style="white-space:pre-wrap">old_table</code>, you can create another table
<code class="fixed" style="white-space:pre-wrap">new_table</code> that has the same structure but is empty, and then
replace the existing table with the empty one as follows (assuming that
<code class="fixed" style="white-space:pre-wrap">backup_table</code> does not already exist):

```sql
CREATE TABLE new_table (...);
RENAME TABLE old_table TO backup_table, new_table TO old_table;
```

`tbl_name` can optionally be specified as `db_name`.`tbl_name`. See [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers). This allows to use `RENAME` to move a table from a database to another (as long as they are on the same filesystem):

```sql
RENAME TABLE db1.t TO db2.t;
```

Note that moving a table to another database is not possible if it has some [triggers](/programming-customizing-mariadb/triggers-events/triggers). Trying to do so produces the following error:

```sql
ERROR 1435 (HY000): Trigger in wrong schema
```

Also, views cannot be moved to another database:

```sql
ERROR 1450 (HY000): Changing schema from 'old_db' to 'new_db' is not allowed.
```

If a `RENAME TABLE` renames more than one table and one renaming fails, all renames executed by the same statement are rolled back.

Renames are always executed in the specified order. Knowing this, it is also possible to swap two tables' names:

```sql
RENAME TABLE t1 TO tmp_table,
    t2 TO t1,
    tmp_table TO t2;
```

### WAIT/NOWAIT

##### MariaDB starting with [10.3.0](/kb/en/mariadb-1030-release-notes/)

Set the lock wait timeout. See [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait).

## Privileges

Executing the `RENAME TABLE` statement requires the [DROP](/kb/en/grant/#table-privileges), [CREATE](/kb/en/grant/#table-privileges) and [INSERT](/kb/en/grant/#table-privileges) privileges for the table or the database.