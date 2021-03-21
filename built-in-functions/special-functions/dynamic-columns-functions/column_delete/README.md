# COLUMN_DELETE

##### MariaDB starting with [5.3](/kb/en/what-is-mariadb-53/)

The [Dynamic columns](/sql-statements-structure/nosql/dynamic-columns) feature was introduced in [MariaDB 5.3](/kb/en/what-is-mariadb-53/).

## Syntax

```sql
COLUMN_DELETE(dyncol_blob, column_nr, column_nr...);
COLUMN_DELETE(dyncol_blob, column_name, column_name...);
```

## Description

Deletes a [dynamic column](/sql-statements-structure/nosql/dynamic-columns) with the specified name. Multiple names can be given. The return value is a dynamic column blob after the modification.