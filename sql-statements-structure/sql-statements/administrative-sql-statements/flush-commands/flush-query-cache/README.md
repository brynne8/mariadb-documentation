# FLUSH QUERY CACHE

## Description

You can defragment [the query cache](/kb/en/the-query-cache/) to better utilize its memory with
the `FLUSH QUERY CACHE` statement. The statement does not remove any queries from the cache.

The [RESET QUERY CACHE](/sql-statements-structure/sql-statements/administrative-sql-statements/reset) statement removes all query results from the query cache.
The [FLUSH TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) statement also does this.