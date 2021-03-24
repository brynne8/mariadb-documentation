# Information Schema INNODB_CHANGED_PAGES Table

##### MariaDB starting with [10.0.9](/kb/en/mariadb-1009-release-notes/)

The `INNODB_CHANGED_PAGES` table was added in [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/).

The [Information Schema](/kb/en/information_schema/) `INNODB_CHANGED_PAGES` Table contains data about modified pages from the bitmap file. It is updated at checkpoints by the log tracking thread parsing the log, so does not contain real-time data.

The number of records is limited by the value of the [innodb_max_changed_pages](/kb/en/xtradbinnodb-server-system-variables/#innodb_max_changed_pages) system variable.

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>SPACE_ID</code></td><td>Modified page space id</td></tr>
<tr><td><code>PAGE_ID</code></td><td>Modified page id</td></tr>
<tr><td><code>START_LSN</code></td><td>Interval start after which page was changed (equal to checkpoint LSN)</td></tr>
<tr><td><code>END_LSN</code></td><td>Interval end before which page was changed (equal to checkpoint LSN)</td></tr>
</tbody></table>