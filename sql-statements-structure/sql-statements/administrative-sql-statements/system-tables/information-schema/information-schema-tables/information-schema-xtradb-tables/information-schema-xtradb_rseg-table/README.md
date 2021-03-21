# Information Schema XTRADB_RSEG Table

##### MariaDB starting with [10.0.9](/kb/en/mariadb-1009-release-notes/)

The `XTRADB_RSEG` table was added in [MariaDB 10.0.9](/kb/en/mariadb-1009-release-notes/).

The [Information Schema](/kb/en/information_schema/) `XTRADB_RSEG` table contains information about the XtraDB rollback segments.

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>rseg_id</code></td><td>Rollback segment id.</td></tr>
<tr><td><code>space_id</code></td><td>Space where the segment is placed.</td></tr>
<tr><td><code>zip_size</code></td><td>Size in bytes of the compressed page size, or zero if uncompressed.</td></tr>
<tr><td><code>page_no</code></td><td>Page number of the segment header.</td></tr>
<tr><td><code>max_size</code></td><td>Maximum size in pages.</td></tr>
<tr><td><code>curr_size</code></td><td>Current size in pages.</td></tr>
</tbody></table>

The number of records will match the value set in the <a undefined>innodb_undo_logs</a> variable (by default 128).