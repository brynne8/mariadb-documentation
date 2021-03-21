# ColumnStore System Databases

When using ColumnStore, MariaDB Server creates a series of system databases used for operational purposes.

<table><tbody><tr><th>Database</th><th>Description</th></tr>
<tr><td><code>calpontsys</code></td><td>Database maintains table metadata about ColumnStore tables.</td></tr>
<tr><td><code>infinidb_querystats</code></td><td>Database maintains information about query performance.  For more information, see <a href="/kb/en/analyzing-queries-in-columnstore/">Query Analysis</a>.</td></tr>
<tr><td><code>infinidb_vtable</code></td><td>Database used in creation of temporary tables in query execution.  This database only exists in ColumnStore 1.2 and below. In those versions, users executing ColumnStore queries must have <code>CREATE TEMPORARY TABLE</code> privilege on this database.  For more information, see <a href="/kb/en/columnstore-database-user-management/">User Management</a>.</td></tr>
<tr><td><code>columnstore_info</code></td><td>Database for stored procedures used to retrieve information about ColumnStore usage.  For more information, see the <a href="/kb/en/columnstore-information-schema-tables/">ColumnStore Information Schema</a> tables.</td></tr>
</tbody></table>