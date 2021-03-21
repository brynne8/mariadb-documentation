# ColumnStore Information Functions

## Functions

MariaDB ColumnStore Information Functions are selectable pseudo functions that return MariaDB ColumnStore specific “meta” information to ensure queries can be locally directed to a specific node. These functions can be specified in the projection (SELECT), WHERE, GROUP BY, HAVING and ORDER BY portions of the SQL statement and will be processed in a distributed manner.

<table><tbody><tr><th>Function</th><th>Description</th></tr>
<tr><td>idbBlockId(column)</td><td>The Logical Block Identifier (LBID) for the block containing the physical row</td></tr>
<tr><td>idbDBRoot(column)</td><td>The DBRoot where the physical row resides</td></tr>
<tr><td>idbExtentId(column)</td><td>The Logical Block Identifier (LBID) for the first block in the extent containing the physical row</td></tr>
<tr><td>idbExtentMax(column)</td><td>The max value from the extent map entry for the extent containing the physical row</td></tr>
<tr><td>idbExtentMin(column)</td><td>The min value from the extent map entry for the extent containing the physical row</td></tr>
<tr><td>idbExtentRelativeRid(column)</td><td>The row id (1 to 8,388,608) within the column's extent</td></tr>
<tr><td>idbLocalPm()</td><td>The PM from which the query was launched. This function will return NULL if the query is launched from a standalone UM</td></tr>
<tr><td>idbPartition(column)</td><td>The three part partition id (Directory.Segment.DBRoot)</td></tr>
<tr><td>idbPm(column)</td><td>The PM where the physical row resides</td></tr>
<tr><td>idbSegmentDir(column)</td><td>The lowest level directory id for the column file containing the physical row</td></tr>
<tr><td>idbSegment(column)</td><td>The number of the segment file containing the physical row</td></tr>
</tbody></table>

## Example

SELECT involving a join between a fact table on the PM node and dimension table across all the nodes to import back on local PM:

With the [infinidb_local_query](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/managing-columnstore/managing-columnstore-database-environment/configuring-columnstore-local-pm-query-mode/) variable set to 0 (default with [local PM Query](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/managing-columnstore/managing-columnstore-database-environment/configuring-columnstore-local-pm-query-mode/) ):

Create a script (i.e., extract_query_script.sql in our example) similar to the following:

```sql
set infinidb_local_query=0;
select fact.column1, dim.column2 
from fact join dim using (key) 
where idbPm(fact.key) = idbLocalPm();
```

The [infinidb_local_query](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/managing-columnstore/managing-columnstore-database-environment/configuring-columnstore-local-pm-query-mode/) is set to 0 to allow query across all PMs.

The query is structured so that the UM process on the PM node gets the fact table data locally from the PM node (as indicated by the use of the idbLocalPm() function), while the dimension table data is extracted from all the PM nodes.

Then you can execute the script to pipe it directly into cpimport:

```sql
mcsmysql source_schema -N < extract_query_script.sql | /usr/local/mariadb/columnstore/bin/cpimport target_schema target_table -s '\t' –n1
```