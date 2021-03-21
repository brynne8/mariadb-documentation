# CHECK VIEW

##### MariaDB starting with [10.0.18](/kb/en/mariadb-10018-release-notes/)

CHECK VIEW was introduced in [MariaDB 10.0.18](/kb/en/mariadb-10018-release-notes/).

## Syntax

```sql
CHECK VIEW view_name
```

## Description

The `CHECK VIEW` statement was introduced in [MariaDB 10.0.18](/kb/en/mariadb-10018-release-notes/) to assist with fixing [MDEV-6916](https://jira.mariadb.org/browse/MDEV-6916), an issue introduced in [MariaDB 5.2](/kb/en/what-is-mariadb-52/) where the view algorithms were swapped. It checks whether the view algorithm is correct. It is run as part of [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade), and should not normally be required in regular use.

## See Also

- [REPAIR VIEW](/sql-statements-structure/sql-statements/table-statements/repair-view)