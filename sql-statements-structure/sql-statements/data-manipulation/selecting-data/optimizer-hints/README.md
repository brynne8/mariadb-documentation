# Optimizer Hints

## Optimizer hints

There are some options available in [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) to affect the execution plan.  These are known as optimizer hints.

#### HIGH PRIORITY

`HIGH_PRIORITY` gives the statement a higher priority. If the table is locked, high priority `SELECT`s will be executed as soon as the lock is released, even if other statements are queued. `HIGH_PRIORITY` applies only if the storage engine only supports table-level locking (`MyISAM`, `MEMORY`, `MERGE`). See [HIGH_PRIORITY and LOW_PRIORITY clauses](/kb/en/high_priority-and-low_priority-clauses/) for details.

#### SQL_CACHE / SQL_NO_CACHE

If the [query_cache_type](/kb/en/server-system-variables/#query_cache_type) system variable is set to 2 or `DEMAND`, and the current statement is cacheable, `SQL_CACHE` causes the query to be cached and `SQL_NO_CACHE` causes the query not to be cached. For `UNION`s, `SQL_CACHE` or `SQL_NO_CACHE` should be specified for the first query. See also [The Query Cache](/kb/en/the-query-cache/) for more detail and a list of the types of statements that aren't cacheable.

#### SQL_BUFFER_RESULT

`SQL_BUFFER_RESULT` forces the optimizer to use a temporary table to process the result. This is useful to free locks as soon as possible.

#### SQL_SMALL_RESULT / SQL_BIG_RESULT

`SQL_SMALL_RESULT` and `SQL_BIG_RESULT` tell the optimizer whether the result is very big or not. Usually, `GROUP BY` and `DISTINCT` operations are performed using a temporary table. Only if the result is very big, using a temporary table is not convenient. The optimizer automatically knows if the result is too big, but you can force the optimizer to use a temporary table with `SQL_SMALL_RESULT`, or avoid the temporary table using `SQL_BIG_RESULT`.

#### STRAIGHT_JOIN

`STRAIGHT_JOIN` applies to the [JOIN](/kb/en/join/) queries, and tells the optimizer that the tables must be read in the order they appear in the `SELECT`. For `const` and `system` table this options is sometimes ignored.

#### SQL_CALC_FOUND_ROWS

`SQL_CALC_FOUND_ROWS` is only applied when using the `LIMIT` clause. If this option is used, MariaDB will count how many rows would match the query, without the `LIMIT` clause. That number can be retrieved in the next query, using [FOUND_ROWS()](/built-in-functions/secondary-functions/information-functions/found_rows).

#### USE/FORCE/IGNORE INDEX

`USE INDEX`, `FORCE INDEX` and `IGNORE INDEX` constrain the query planning to a specific index.

For further information about some of these options, see [How to force query plans](/kb/en/how-to-force-query-plans/).