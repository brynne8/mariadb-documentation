# Information Schema XTRADB_READ_VIEW Table

##### MariaDB starting with [10.0.9](/kb/en/mariadb-1009-release-notes/)

The `XTRADB_READ_VIEW` table was added in [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/).

The [Information Schema](/kb/en/information_schema/) `XTRADB_READ_VIEW` table contains information about the oldest active transaction in the system.

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>READ_VIEW_UNDO_NUMBER</code></td><td></td></tr>
<tr><td><code>READ_VIEW_LOW_LIMIT_TRX_NUMBER</code></td><td>Highest transaction number at the time the view was created.</td></tr>
<tr><td><code>READ_VIEW_UPPER_LIMIT_TRX_ID</code></td><td>Highest transaction ID at the time the view was created. Should not see newer transactions with IDs equal to or greater than the value.</td></tr>
<tr><td><code>READ_VIEW_LOW_LIMIT_TRX_ID</code></td><td>Latest committed transaction ID at the time the oldest view was created. Should see all transactions with IDs equal to or less than the value.</td></tr>
</tbody></table>