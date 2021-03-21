# REPAIR VIEW

##### MariaDB starting with [10.0.18](/kb/en/mariadb-10018-release-notes/)

REPAIR VIEW was introduced in [MariaDB 10.0.18](/kb/en/mariadb-10018-release-notes/) and [MariaDB 5.5.43](/kb/en/mariadb-5543-release-notes/).

## Syntax

```sql
REPAIR [NO_WRITE_TO_BINLOG | LOCAL] VIEW  view_name[, view_name] ... [FROM MYSQL]
```

## Description

The `REPAIR VIEW` statement was introduced to assist with fixing [MDEV-6916](https://jira.mariadb.org/browse/MDEV-6916), an issue introduced in [MariaDB 5.2](/kb/en/what-is-mariadb-52/) where the view algorithms were swapped compared to their MySQL on disk representation. It checks whether the view algorithm is correct. It is run as part of [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade), and should not normally be required in regular use.

By default it corrects the checksum and if necessary adds the mariadb-version field. If the optional `FROM MYSQL` clause is used, and no mariadb-version field is present, the MERGE and TEMPTABLE algorithms are toggled.

By default, `REPAIR VIEW` statements are written to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) and will be [replicated](/replication). The `NO_WRITE_TO_BINLOG` keyword (`LOCAL` is an alias) will ensure the statement is not written to the binary log.

Note that REPAIR VIEW in [MariaDB 10.0.18](/kb/en/mariadb-10018-release-notes/) and [MariaDB 5.5.43](/kb/en/mariadb-5543-release-notes/) could crash the server (see [MDEV-8115](https://jira.mariadb.org/browse/MDEV-8115)). Upgrade to a later version.

## See Also

- [CHECK VIEW](/sql-statements-structure/sql-statements/table-statements/check-view)