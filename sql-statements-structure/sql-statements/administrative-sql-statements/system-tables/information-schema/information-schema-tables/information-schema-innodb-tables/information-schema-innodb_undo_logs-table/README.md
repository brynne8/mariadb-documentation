# Information Schema INNODB_UNDO_LOGS Table

##### MariaDB [5.5.27](/kb/en/mariadb-5527-release-notes/) - [10.0](/kb/en/what-is-mariadb-100/)

The `INNODB_UNDO_LOGS` are a Percona enhancement, introduced in version [MariaDB 5.5.27](/kb/en/mariadb-5527-release-notes/) and removed in 10.0.

The [Information Schema](/kb/en/information_schema/) `INNODB_UNDO_LOGS` table is a Percona enchancement, and is only available for XtraDB, not InnoDB (see [XtraDB and InnoDB](/kb/en/xtradb-and-innodb/)). It contains information about the the InnoDB [undo log](/kb/en/undo-log/), with each record being an undo log segment.  It was removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>TRX_ID</code></td><td>Unique transaction ID number, matching the value from the <a href="/kb/en/information-schema-innodb_trx-table/">information_schema.INNODB_TRX</a> table.</td></tr>
<tr><td><code>RSEG_ID</code></td><td>Rollback segment ID, matching the value from the <code>information_schema.INNODB_RSEG</code> table.</td></tr>
<tr><td><code>USEG_ID</code></td><td>Undo segment ID.</td></tr>
<tr><td><code>SEGMENT_TYPE</code></td><td>Indicates the operation type, for example <code>INSERT</code> or <code>UPDATE</code>.</td></tr>
<tr><td><code>STATE</code></td><td>Segment state; one of <code>ACTIVE</code> (contains active transaction undo log), <code>CACHED</code>, <code>TO_FREE</code> (insert undo segment can be freed), <code>TO_PURGE</code> (update undo segment won't be re-used and can be purged when all undo data is removed) or <code>PREPARED</code> (segment of a prepared transaction).</td></tr>
<tr><td><code>SIZE</code></td><td>Size in pages of the segment.</td></tr>
</tbody></table>