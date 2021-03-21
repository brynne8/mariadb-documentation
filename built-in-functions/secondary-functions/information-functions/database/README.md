# DATABASE

## Syntax

```sql
DATABASE()
```

## Description

Returns the default (current) database name as a string in the utf8 [character set](/kb/en/data-types-character-sets-and-collations/). If there is no default database, DATABASE() returns NULL. Within a [stored routine](/programming-customizing-mariadb/stored-routines/), the default database is the database that the routine is associated with, which is not necessarily the same as the database that is the default in the calling context.

SCHEMA() is a synonym for DATABASE().

To select a default database, the [USE](/sql-statements-structure/sql-statements/administrative-sql-statements/use/) statement can be run. Another way to set the default database is specifying its name at [mysql](/clients-utilities/mysql-client/mysql-command-line-client/) command line client startup.

## Examples

```sql
SELECT DATABASE();
+------------+
| DATABASE() |
+------------+
| NULL       |
+------------+

USE test;
Database changed

SELECT DATABASE();
+------------+
| DATABASE() |
+------------+
| test       |
+------------+
```