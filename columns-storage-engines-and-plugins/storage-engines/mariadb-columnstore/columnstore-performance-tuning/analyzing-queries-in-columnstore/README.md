# Analyzing Queries in ColumnStore

# Determining active queries

## show processlist

The MariaDB <strong>show processlist</strong> command may be used to see a list of active queries on that UM:

```sql
MariaDB [test]> show processlist;
+----+------+-----------+-------+---------+------+-------+--------------+
| Id | User | Host | db | Command | Time | State | Info |
+----+------+-----------+-------+---------+------+-------+--------------+
| 73 | root | localhost | ssb10 | Query | 0 | NULL | show processlist
+----+------+-----------+-------+---------+------+-------+--------------+
1 row in set (0.01 sec)
```

## getActiveSQLStatements

<em>getActiveSQLStatements</em> is a mcsadmin command that shows which SQL statements are currently being executed on the database:

```sql
mcsadmin> getActiveSQLStatements
getactivesqlstatements Wed Oct 7 08:38:32 2015
Get List of Active SQL Statements
=================================
Start Time    Time (hh:mm:ss) Session ID SQL Statement
---------------- ---------------- -------------------- ------------------------------------------------------------
Oct 7 08:38:30    00:00:03       73 select c_name,sum(lo_revenue) from customer, lineorder where lo_custkey = c_custkey and c_custkey = 6 group by c_name
```

# Analysis of individual queries

## Query statistics

The <em>calGetStats</em>  function provides statistics about resources used on the [User Module](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-user-module/) (UM) node, PM node, and network by the last run query.
Example:

```sql
MariaDB [test]> select count(*) from wide2;
+----------+                                       
| count(*) |
+----------+
|  5000000 |
+----------+
1 row in set (0.22 sec)

MariaDB [test]> select calGetStats();
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| calGetStats()                                                                                                                                                                                     |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| Query Stats: MaxMemPct-0; NumTempFiles-0; TempFileSpace-0B; ApproxPhyI/O-1931; CacheI/O-2446; BlocksTouched-2443; PartitionBlocksEliminated-0; MsgBytesIn-73KB; MsgBytesOut-1KB; Mode-Distributed |
+---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
1 row in set (0.01 sec)
```

The output contains information on:

- <strong>MaxMemPct</strong> - Peak memory utilization on the [User Module](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-user-module/), likely in support of a large (User Module) based hash join operation.
- <strong>NumTempFiles</strong> - Report on any temporary files created in support of query operations larger than available memory, typically for unusual join operations where the smaller table join cardinality exceeds some configurable threshold.
- <strong>TempFileSpace</strong> - Report on space used by temporary files created in support of query operations larger than available memory, typically for unusual join operations where the smaller table join cardinality exceeds some configurable threshold.
- <strong>PhyI/O</strong> - Number of 8k blocks read from disk, SSD, or other persistent storage.
- <strong>CacheI/O</strong> - Approximate number of 8k blocks processed in memory, adjusted down by the number of discrete PhyI/O calls required.
- <strong>BlocksTouched</strong> - Approximate number of 8k blocks processed in memory.
- <strong>PartitionBlocksEliminated</strong> - The number of block touches eliminated via the Extent Map elimination behavior.
- <strong>MsgBytesIn, MsgByteOut</strong> - Message size in MB sent between nodes in support of the query.

The output is useful to determine how much physical I/O was required, how much data was cached, and how many partition blocks were eliminated through use of extent map elimination. The system maintains min / max values for each extent and uses these to help implement where clause filters to completely bypass extents where the value is outside of the min/max range. When a column is ordered (or semi-ordered) during load such as a time column this offer very large performance gains as the system can avoid scanning many extents for the column.

# Query plan / trace

While the MariaDB Server's [EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain/) utility can be used to look at the query plan, it is somewhat less helpful for ColumnStore tables as ColumnStore does not use indexes or make use of MariaDB I/O functionality.
The execution plan for a query on a ColumnStore table is made up of multiple steps. Each step in the query plan performs a set of operations that are issued from the [User Module](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-user-module/) to the set of [Performance Modules](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/) in support of a given step in a query.

- Full Column Scan - an operation that scans each entry in a column using all available threads on the Performance Modules. Speed of operation is generally related to the size of the data type and the total number of rows in the column. The closest analogy for a traditional system is an index scan operation.
- Partitioned Column Scan - an operation that uses the Extent Map to identify that certain portions of the column do not contain any matching values for a given set of filters. The closest analogy for a traditional row-based DBMS is a partitioned index scan, or partitioned table scan operation.
- Column lookup by row offset - once the set of matching filters have been applied and the minimal set of rows have been identified, additional blocks are requested using a calculation that determines exactly which block is required. The closest analogy for a traditional system is a lookup by rowid.

These operations are automatically executed together in order to execute appropriate filters and column lookup by row offset.

## Viewing the ColumnStore query plan

In MariaDB ColumnStore there is a set of SQL tracing stored functions provided to see the distributed query execution plan between the [UM](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-user-module/) and the [PM](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-performance-module/).

The basic steps to using these SQL tracing stored functions are:

1 Start the trace for the particular session.
2 Execute the SQL statement in question.
3 Review the trace collected for the statement.
As an example, the following session starts a trace, issues a query against a 6 million row fact table and
300,000 row dimension table, and then reviews the output from the trace:

```sql
MariaDB [test]> select calSetTrace(1);
+----------------+
| calSetTrace(1) |
+----------------+
|              0 |
+----------------+
1 row in set (0.00 sec)

MariaDB [test]> select c_name, sum(o_totalprice)
    -> from customer, orders
    -> where o_custkey = c_custkey
    -> and c_custkey = 5
    -> group by c_name;
+--------------------+-------------------+
| c_name             | sum(o_totalprice) |
+--------------------+-------------------+
| Customer#000000005 |         684965.28 |
+--------------------+-------------------+
1 row in set, 1 warning (0.34 sec)

MariaDB [test]> select calGetTrace();
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------                                                                                                                    --------------------------------------------------------------------------------------------------------------------------------------------------------------------                                                                                                                    ----------------------------------------------------------------------------------------------------------+
| calGetTrace()                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------                                                                                                                    --------------------------------------------------------------------------------------------------------------------------------------------------------------------                                                                                                                    ----------------------------------------------------------------------------------------------------------+
|
Desc Mode Table           TableOID ReferencedColumns        PIO LIO PBE Elapsed Rows
BPS  PM   customer        3024     (c_custkey,c_name)       0   43  36  0.006   1
BPS  PM   orders          3038     (o_custkey,o_totalprice) 0   766 0   0.032   3
HJS  PM   orders-customer 3038     -                        -   -   -   -----   -
TAS  UM   -               -        -                        -   -   -   0.021   1
 |
+-------------------------------------------------------------------------------------------------------------------------------------------------------------------                                                                                                                    --------------------------------------------------------------------------------------------------------------------------------------------------------------------                                                                                                                    ----------------------------------------------------------------------------------------------------------+
1 row in set (0.00 sec)

```

The columns headings in the output are as follows:

- <strong>Desc</strong> – Operation being executed. Possible values:
<ul start="1"><li><strong>BPS</strong> - Batch Primitive Step : scanning or projecting the column blocks.
</li><li><strong>CES</strong> - Cross Engine Step: Performing Cross engine join
</li><li><strong>DSS</strong> - Dictionary Structure Step : a dictionary scan for a particular variable length string value.
</li><li><strong>HJS</strong> - Hash Join Step : Performing a hash join between 2 tables
</li><li><strong>HVS</strong> - Having Step: Performing the having clause on the result set 
</li><li><strong>SQS</strong> - Sub Query Step: Performaning a sub query
</li><li><strong>TAS</strong> - Tuple Aggregation step : the process of receiving intermediate aggregation results at the UM from the PM nodes.
</li><li><strong>TNS</strong> - Tuple Annexation Step : Query result finishing, e.g. filling in constant columns, limit, order by and final distinct cases.
</li><li><strong>TUS</strong> = Tuple Union step : Performing a SQL union of 2 sub queries.
</li><li><strong>TCS</strong> = Tuple Constant Step: Process Constant Value Columns
</li><li><strong>WFS</strong> = Window Function Step: Performing a window function. 
</li></ul>
- <strong>Mode</strong> – Where the operation was performed: UM or PM
- <strong>Table</strong> – Table for which columns may be scanned/projected.
- <strong>TableOID</strong> – ObjectID for the table being scanned.
- <strong>ReferencedOIDs</strong> – ObjectIDs for the columns required by the query.
- <strong>PIO</strong> – Physical I/O (reads from storage) executed for the query.
- <strong>LIO</strong> – Logical I/O executed for the query, also known as Blocks Touched.
- <strong>PBE</strong> – Partition Blocks Eliminated identifies blocks eliminated by Extent Map min/max.
- <strong>Elapsed</strong> – Elapsed time for a give step.
- <strong>Rows</strong> – Intermediate rows returned

<strong> Note: The time recorded is the time from PrimProc and ExeMgr. Execution time from withing mysqld is not tracked here. There could be extra processing time in mysqld due to a number of factors such as ORDER BY.</strong>

# Cache clearing to enable cold testing

Sometimes it can be useful to clear caches to allow understanding of uncached and cached query access. The calFlushCache() function will clear caches on all servers. This is only really useful for testing query performance:

```sql
MariaDB [test]> select calFlushCache();
```

# Viewing extent map information

It can be useful to view details about the extent map for a given column. This can be achieved using the editem process on a PM server.  Available arguments can be provided by using the -h flag. The most common use is to provide the column object id with the -o argument which will output details for the column and in this case the -t argument is provided to show min / max values as dates:

```sql
/usr/local/mariadb/columnstore/bin/editem -o 3032 -t
Col OID = 3032, NumExtents = 10, width = 4
428032 - 432127 (4096) min: 1992-01-01, max: 1993-06-21, seqNum: 1, state: valid, fbo: 0, DBRoot: 1, part#: 0, seg#: 0, HWM: 0; status: avail
502784 - 506879 (4096) min: 1992-01-01, max: 1993-06-22, seqNum: 1, state: valid, fbo: 0, DBRoot: 2, part#: 0, seg#: 1, HWM: 0; status: unavail
708608 - 712703 (4096) min: 1993-06-21, max: 1994-12-11, seqNum: 1, state: valid, fbo: 0, DBRoot: 1, part#: 0, seg#: 2, HWM: 0; status: unavail
766976 - 771071 (4096) min: 1993-06-22, max: 1994-12-12, seqNum: 1, state: valid, fbo: 0, DBRoot: 2, part#: 0, seg#: 3, HWM: 0; status: unavail
989184 - 993279 (4096) min: 1994-12-11, max: 1996-06-01, seqNum: 1, state: valid, fbo: 4096, DBRoot: 1, part#: 0, seg#: 0, HWM: 8191; status: avail
1039360 - 1043455 (4096) min: 1994-12-12, max: 1996-06-02, seqNum: 1, state: valid, fbo: 4096, DBRoot: 2, part#: 0, seg#: 1, HWM: 8191; status: avail
1220608 - 1224703 (4096) min: 1996-06-01, max: 1997-11-22, seqNum: 1, state: valid, fbo: 4096, DBRoot: 1, part#: 0, seg#: 2, HWM: 8191; status: avail
1270784 - 1274879 (4096) min: 1996-06-02, max: 1997-11-22, seqNum: 1, state: valid, fbo: 4096, DBRoot: 2, part#: 0, seg#: 3, HWM: 8191; status: avail
1452032 - 1456127 (4096) min: 1997-11-22, max: 1998-08-02, seqNum: 1, state: valid, fbo: 0, DBRoot: 1, part#: 1, seg#: 0, HWM: 1930; status: avail
1510400 - 1514495 (4096) min: 1997-11-22, max: 1998-08-02, seqNum: 1, state: valid, fbo: 0, DBRoot: 2, part#: 1, seg#: 1, HWM: 1930; status: avail
```

Here it can be seen that the extent maps for the o_orderdate (object id 3032) column are well partitioned since the order table source data was sorted by the order_date. This example shows 2 seperate DBRoot values as the environment was a 2 node combined deployment.

Column object ids may be found by querying the calpontsys.syscolumn metadata table (deprecated) or information_schema.columnstore_columns table (version 1.0.6+).

# Query statistics history

MariaDB ColumnStore query statistics history can be retrieved for analysis. By default the query stats collection is disabled. To enable the collection of query stats, the &lt;QueryStats&gt;&lt;Enabled&gt; element in the ColumnStore.XML configuration file should be set to Y (default is N).

```sql
<QueryStats>
<Enabled>Y</Enabled>
</QueryStats>
```

Cross Engine Support must also be enabled before enabling Query Statistics. See the [Cross Engine Configuration](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/managing-columnstore/managing-columnstore-database-environment/configuring-columnstore-cross-engine-joins/) section.

When enabled the history of query statistics across all sessions along with execution time, and those stats provided by calgetstats() is stored in a table in the infinidb_querystats schema. Only queries in the following ColumnStore syntax are available for statistics monitoring:

- SELECT
- INSERT
- UPDATE
- DELETE
- INSERT SELECT
- LOAD DATA INFILE

## Query statistics table

When QueryStats is enabled,  the query statistics history is collected in the querystats table in the infinidb_querystats schema.

The columns of this table are:

- <strong>queryID</strong> - A unique identifier assigned to the query
- <strong>Session ID (sessionID)</strong> - The session number that executed the statement.
- <strong>queryType</strong> - The type of the query whether insert, update, delete, select, delete, insert select or load data infile
- <strong>query</strong> - The text of the query
- <strong>Host (host)</strong> - The host that executed the statement.
- <strong>User ID (user)</strong> - The user that executed the statement.
- <strong>Priority (priority)</strong> The priority the user has for this statement.
- <strong>Query Execution Times (startTime, endTime)</strong> Calculated as end time – start time.
<ul start="1"><li><strong>start time</strong> -  the time that the query gets to ExeMgr, DDLProc, or DMLProc
</li><li><strong>end time</strong> - the time that the last result packet exits ExeMgr, DDLProc or DMLProc
</li></ul>
- <strong>Rows returned or affected (rows)</strong> -The number of rows returned for SELECT queries, or the number of rows affected by DML queries. Not valid for DDL and other query types.
- <strong>Error Number (errNo)</strong> - The IDB error number if this query failed, 0 if it succeeded.
- <strong>Physical I/O (phyIO)</strong> - The number of blocks that the query accessed from the disk, including the pre-fetch blocks. This statistic is only valid for the queries that are processed by ExeMgr, i.e. SELECT, DML with WHERE clause, and INSERT SELECT.
- <strong>Cache I/O (cacheIO)</strong> - The number of blocks that the query accessed from the cache. This statistic is only valid for queries that are processed by ExeMgr, i.e. SELECT, DML with WHERE clause, and INSERT SELECT.
- <strong>Blocks Touched (blocksTouched)</strong> - The total number of blocks that the query accessed physically and from the cache. This should be equal or less than the sum of physical I/O and cache I/O. This statistic is only valid for queries that are processed by ExeMgr, i.e. SELECT, DML with WHERE clause, and INSERT SELECT.
- <strong>Partition Blocks Eliminated (CPBlocksSkipped)</strong> - The number of blocks being eliminated by the extent map casual partition. This statistic is only valid for queries that are processed by ExeMgr, i.e. SELECT, DML with WHERE clause, and INSERT SELECT.
- <strong>Messages from [UM](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-user-module/) to [PM](columnstore-performance-modulec) (msgOutUM)</strong> - The number of messages in bytes that ExeMgr sends to the PrimProc. If a message needs to be distributed to all the PMs, the sum of all the distributed messages will be counted. Only valid for queries that are processed by ExeMgr, i.e. SELECT, DML with WHERE clause, and INSERT SELECT.
- <strong>Messages from PM to UM (msgInUM)</strong> - The number of messages in bytes that PrimProc sends to the ExeMgr. Only valid for queries that are processed by ExeMgr, i.e. SELECT, DML with where clause, and INSERT SELECT.
- <strong>Memory Utilization (maxMemPct)</strong> - This field shows memory utilization for the [User Module](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-architecture/columnstore-user-module/) (UM) in support of any UM join, group by, aggregation, distinct, or other operation.
- <strong>Blocks Changed (blocksChanged)</strong> - Total number of blocks that queries physically changed on disk. This is only for delete/update statements.
- <strong>Temp Files (numTempFiles)</strong> - This field shows any temporary file utilization for the User Module (UM) in support of any UM join, group by, aggregation, distinct, or other operation.
- <strong>Temp File Space (tempFileSpace)</strong> - This shows the size of any temporary file utilization for the User Module (UM) in support of any UM join, group by, aggregation, distinct, or other operation.

## Query statistics viewing

Users can view the query statistics by selecting the rows from the query stats table in the infinidb_querystats schema. Examples listed below:

- Example 1: List execution time, rows returned for all the select queries within the past 12 hours:<br>

```sql
MariaDB [infinidb_querystats]> select queryid, query, endtime-starttime, rows from querystats 
where starttime >= now() - interval 12 hour and querytype = 'SELECT'; 
```

- Example 2: List the three slowest running select queries of session 2 within the past 12 hours:<br>

```sql
MariaDB [infinidb_querystats]> select a.* from (select endtime-starttime execTime, query from queryStats 
where sessionid = 2 and querytype = 'SELECT' and starttime >= now()-interval 12 hour
order by 1 limit 3) a;
```

- Example 3: List the average, min and max running time of all the INSERT SELECT queries within the past 12 hours:<br>

```sql
MariaDB [infinidb_querystats]> select min(endtime-starttime), max(endtime-starttime), avg(endtime-starttime) from querystats 
where querytype='INSERT SELECT' and starttime >= now() - interval 12 hour;
```