# Information Schema CHANGED_PAGE_BITMAPS Table

##### MariaDB starting with [10.1.6](/kb/en/mariadb-1016-release-notes/)

The`CHANGED_PAGE_BITMAPS` table was added in [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/).

The [Information Schema](/kb/en/information_schema/) `CHANGED_PAGE_BITMAPS` table is a dummy table added to [XtraDB](/kb/en/xtradb/) with reset_table callback to allow `FLUSH NO_WRITE_TO_BINLOG CHANGED_PAGE_BITMAPS` to be called from `innobackupex`. It contains only one column, <em>dummy</em>.

For more information, see [MDEV-7472](https://jira.mariadb.org/browse/MDEV-7472).

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.