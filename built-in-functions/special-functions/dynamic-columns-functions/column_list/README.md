# COLUMN_LIST

##### MariaDB starting with [5.3](/kb/en/what-is-mariadb-53/)

The [Dynamic columns](/sql-statements-structure/nosql/dynamic-columns/) feature was introduced in [MariaDB 5.3](/kb/en/what-is-mariadb-53/).

## Syntax

```sql
COLUMN_LIST(dyncol_blob);
```

## Description

Since [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/), this function returns a comma-separated list of column names. The names are quoted with backticks.

Before [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/), it returned a comma-separated list of column numbers, not names.

See [dynamic columns](/sql-statements-structure/nosql/dynamic-columns/) for more information.