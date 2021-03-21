# VALUES / VALUE

## Syntax

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

```sql
VALUE(col_name) 
```

##### MariaDB until [10.3.2](/kb/en/mariadb-1032-release-notes/)

```sql
VALUES(col_name) 
```

## Description

In an [INSERT ... ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update) statement, you can use the `VALUES(col_name)` function in the  [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) clause to refer to column values from the  [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert) portion of the statement. In other words,  `VALUES(col_name)` in the `UPDATE` clause refers to the value of col_name that would be inserted, had no duplicate-key conflict occurred. This function is especially useful in multiple-row inserts.

The `VALUES()` function is meaningful only in `INSERT ... ON DUPLICATE KEY UPDATE` statements and returns `NULL` otherwise.

In [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/) this function was renamed to `VALUE()`, because it's incompatible with the standard Table Value Constructors syntax, implemented in [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/).

The `VALUES()` function can still be used even from [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/), but only in `INSERT ... ON DUPLICATE KEY UPDATE` statements; it's a syntax error otherwise.

## Examples

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

```sql
INSERT INTO t (a,b,c) VALUES (1,2,3),(4,5,6)
    ON DUPLICATE KEY UPDATE c=VALUE(a)+VALUE(b);
```

##### MariaDB until [10.3.2](/kb/en/mariadb-1032-release-notes/)

```sql
INSERT INTO t (a,b,c) VALUES (1,2,3),(4,5,6)
    ON DUPLICATE KEY UPDATE c=VALUES(a)+VALUES(b);
```