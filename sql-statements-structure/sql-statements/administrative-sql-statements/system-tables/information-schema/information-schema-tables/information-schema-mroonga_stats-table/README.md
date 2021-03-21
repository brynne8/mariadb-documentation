# Information Schema MROONGA_STATS Table

The [Information Schema](/kb/en/information_schema/) `MROONGA_STATS` table only exists if the [Mroonga](/columns-storage-engines-and-plugins/storage-engines/mroonga) storage engine is installed, and contains information about its activities.

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>VERSION</code></td><td>Mroonga version.</td></tr>
<tr><td><code>rows_written</code></td><td>Number of rows written into Mroonga tables.</td></tr>
<tr><td><code>rows_read</code></td><td>Number of rows read from all Mroonga tables.</td></tr>
</tbody></table>

This table always contains 1 row.