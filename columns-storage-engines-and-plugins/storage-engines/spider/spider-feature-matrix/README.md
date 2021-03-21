# Spider Feature Matrix

##### MariaDB starting with [10.0.4](/kb/en/mariadb-1004-release-notes/)

The Spider storage engine was introduced in [MariaDB 10.0.4](/kb/en/mariadb-1004-release-notes/)

Not complete yet - still being updated

F(*) Federation only , P(*)partioning only .
Spider column is for SpiderForMySQL found on the Spider web sIte.

<table><tbody><tr><th>Feature</th><th>Spider</th><th>10.0</th></tr>
<tr><td><strong>Clustering and High Availability</strong></td><td></td><td></td></tr>
<tr><td>Commit, Rollback transactions on multiple backend</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Multiplexing to a number of replicas using xa protocol 2PC</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Split brain resolution based on a majority decision, failed node is remove from the list of replicas</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Enable a failed backend to re enter the cluster transparently</td><td>No</td><td>No</td></tr>
<tr><td>Synchronize DDL to backend, table modification, schema changes</td><td>No</td><td>No</td></tr>
<tr><td>Synchronize DDL to other Spider</td><td>No</td><td>No</td></tr>
<tr><td>GTID tracking per table on XA error</td><td>No</td><td>Yes</td></tr>
<tr><td>Transparent partitioning</td><td>No</td><td>No</td></tr>
<tr><td>Covered by generic SQL test case</td><td>Yes</td><td>Yes</td></tr>
<tr><td><strong>Heterogenous Backends</strong></td><td></td><td></td></tr>
<tr><td>MariaDB and MySQL database backend</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Oracle database backend, if build from source against the client library 'ORACLE_HOME'</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Local table attachment</td><td>Yes</td><td>Yes</td></tr>
<tr><td><strong>Performance</strong></td><td></td><td></td></tr>
<tr><td>Index Condition Pushdown</td><td>No</td><td>No</td></tr>
<tr><td>Engine Condition Pushdown</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Concurrent backend scan</td><td>Yes</td><td>No</td></tr>
<tr><td>Concurrent partition scan</td><td>Yes</td><td>No</td></tr>
<tr><td>Batched key access</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Block hash join</td><td>No</td><td>Yes</td></tr>
<tr><td>HANDLER backend propagation</td><td>Yes</td><td>Yes</td></tr>
<tr><td>HANDLER backend translation from SQL</td><td>Yes</td><td>Yes</td></tr>
<tr><td>HANDLER OPEN cache per connection</td><td>No</td><td>Yes</td></tr>
<tr><td>HANDLER use prepared statement</td><td>No</td><td>Yes</td></tr>
<tr><td>HANDLER_SOCKET protocol backend propagation</td><td>Yes</td><td>Yes</td></tr>
<tr><td>HANDLER_SOCKET backend translation from SQL</td><td>No</td><td>No</td></tr>
<tr><td>Map reduce for <code>ORDER BY</code> ... LIMIT</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Map reduce for <code>MAX &amp; MIN &amp; SUM</code></td><td>Yes</td><td>Yes</td></tr>
<tr><td>Map reduce for some <code>GROUP BY</code></td><td>Yes</td><td>Yes</td></tr>
<tr><td>Batch multiple WRITES in auto commit to reduce network round trip</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Relaxing backend consistency</td><td>Yes</td><td>Yes</td></tr>
<tr><td><strong>Execution Control</strong></td><td></td><td></td></tr>
<tr><td>Configuration at table and partition level, settings can change per data collection</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Configurable empty result set on errors. For API that does not have transactions replay</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Query Cache tuning per table of the on remote backend</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Index Hint per table imposed on remote backend</td><td>Yes</td><td>Yes</td></tr>
<tr><td>SSL connections to remote backend connections</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Table definition discovery from remote backend</td><td>Yes</td><td>F(*)</td></tr>
<tr><td>Direct SQL execution to backend via UDF</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Table re synchronization between backends via UDF</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Maintain Index and Table Statistics of remote backends</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Can use Independent Index and Table Statistics</td><td>No</td><td>Yes</td></tr>
<tr><td>Maintain local or remote table increments</td><td>Yes</td><td>Yes</td></tr>
<tr><td>LOAD DATA INFILE translate to bulk inserting</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Performance Schema Probes</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Load Balance Reads to replicate weight control</td><td>Yes</td><td>Yes</td></tr>
<tr><td>Fine tuning tcp timeout, connections retry</td><td>Yes</td><td>Yes</td></tr>
</tbody></table>