# Query Cache Thread States

This article documents thread states that are related to the [Query Cache](/replication/optimization-and-tuning/buffers-caches-and-threads/query-cache/). These correspond to the `STATE` values listed by the [SHOW PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/) statement or in the [Information Schema PROCESSLIST Table](/kb/en/information-schema-processlist-table/) as well as the `PROCESSLIST_STATE` value listed in the [Performance Schema threads Table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/performance-schema/performance-schema-tables/performance-schema-threads-table/).

<table><tbody><tr><th>Value</th><th>Description</th></tr>
<tr><td>checking privileges on cached query</td><td>Checking whether the user has permission to access a result in the query cache.</td></tr>
<tr><td>checking query cache for query</td><td>Checking whether the current query exists in the query cache.</td></tr>
<tr><td>invalidating query cache entries</td><td>Marking query cache entries as invalid as the underlying tables have changed.</td></tr>
<tr><td>sending cached result to client</td><td>A result found in the query cache is being sent to the client.</td></tr>
<tr><td>storing result in query cache</td><td>Saving the the result of a query into the query cache.</td></tr>
<tr><td>Waiting for query cache lock</td><td>Waiting to take a query cache lock.</td></tr>
</tbody></table>