# COLUMN_EXISTS

##### MariaDB starting with [5.3](/kb/en/what-is-mariadb-53/)

The [Dynamic columns](/sql-statements-structure/nosql/dynamic-columns/) feature was introduced in [MariaDB 5.3](/kb/en/what-is-mariadb-53/).

## Syntax

```sql
COLUMN_EXISTS(dyncol_blob, column_nr);
COLUMN_EXISTS(dyncol_blob, column_name);
```

## Description

Checks if a column with name `column_name` exists in `dyncol_blob`. If yes, return `1`, otherwise return `0`. See [dynamic columns](/sql-statements-structure/nosql/dynamic-columns/) for more information.