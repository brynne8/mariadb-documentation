# Information Schema INNODB_TABLESPACES_SCRUBBING Table

##### MariaDB [10.1.3](/kb/en/mariadb-1013-release-notes/) - [10.5.1](/kb/en/mariadb-1051-release-notes/)

InnoDB and XtraDB data scrubbing was introduced in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/). The table was removed in [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/) - see [MDEV-15528](https://jira.mariadb.org/browse/MDEV-15528).

The [Information Schema](/kb/en/information_schema/) `INNODB_TABLESPACES_SCRUBBING` table contains [data scrubbing](/kb/en/xtradb-innodb-data-scrubbing/) information.

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>SPACE</code></td><td>InnoDB table space id number.</td></tr>
<tr><td><code>NAME</code></td><td>Path to the table space file, without the extension.</td></tr>
<tr><td><code>COMPRESSED</code></td><td>The compressed page size, or zero if uncompressed.</td></tr>
<tr><td><code>LAST_SCRUB_COMPLETED</code></td><td>Date and time when the last scrub was completed, or <code>NULL</code> if never been performed.</td></tr>
<tr><td><code>CURRENT_SCRUB_STARTED</code></td><td>Date and time when the current scrub started, or <code>NULL</code> if never been performed.</td></tr>
<tr><td><code>CURRENT_SCRUB_ACTIVE_THREADS</code></td><td>Number of threads currently scrubbing the tablespace.</td></tr>
<tr><td><code>CURRENT_SCRUB_PAGE_NUMBER</code></td><td>Page that the scrubbing thread is currently scrubbing, or <code>NULL</code> if not enabled.</td></tr>
<tr><td><code>CURRENT_SCRUB_MAX_PAGE_NUMBER</code></td><td>When a scrubbing starts rotating a table space, the field contains its current size. <code>NULL</code> if not enabled.</td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM information_schema.INNODB_TABLESPACES_SCRUBBING LIMIT 1\G
*************************** 1. row ***************************
                        SPACE: 1
                         NAME: mysql/innodb_table_stats
                   COMPRESSED: 0
         LAST_SCRUB_COMPLETED: NULL
        CURRENT_SCRUB_STARTED: NULL
    CURRENT_SCRUB_PAGE_NUMBER: NULL
CURRENT_SCRUB_MAX_PAGE_NUMBER: 0
         ROTATING_OR_FLUSHING: 0
1 rows in set (0.00 sec)
```