# DROP SEQUENCE

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

DROP SEQUENCE was introduced in [MariaDB 10.3](/kb/en/what-is-mariadb-103/).

## Syntax

```sql
DROP [TEMPORARY] SEQUENCE [IF EXISTS] [/*COMMENT TO SAVE*/]
    sequence_name [, sequence_name] ...
```

## Description

`DROP SEQUENCE` removes one or more sequences created with [CREATE SEQUENCE](/sql-statements-structure/sequences/create-sequence/). You must have the `DROP` privilege for each sequence. MariaDB returns an error indicating by name which non-existing tables it was unable to drop, but it also drops all of the tables in the list that do exist.

Important: When a table is dropped, user privileges on the table are not automatically dropped. See [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/).

If another connection is using the sequence, a metadata lock is active, and this statement will wait until the lock is released. This is also true for non-transactional tables.

For each referenced sequence, DROP SEQUENCE drops a temporary sequence with that name, if it exists. If it does not exist, and the `TEMPORARY` keyword is not used, it drops a non-temporary sequence with the same name, if it exists. The `TEMPORARY` keyword ensures that a non-temporary sequence will not accidentally be dropped.

Use `IF EXISTS` to prevent an error from occurring for sequences that do not exist. A NOTE is generated for each non-existent sequence when using `IF EXISTS`. See [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/).

DROP SEQUENCE requires the [DROP privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/).

## Notes

DROP SEQUENCE only removes sequences, not tables. However, [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table/) can remove both sequences and tables.

## See Also

- [Sequence Overview](/sql-statements-structure/sequences/sequence-overview/)
- [CREATE SEQUENCE](/sql-statements-structure/sequences/create-sequence/)
- [ALTER SEQUENCE](/sql-statements-structure/sequences/alter-sequence/)
- [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table/)