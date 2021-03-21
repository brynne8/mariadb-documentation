# Query Limits and Timeouts

This article describes the different methods MariaDB provides to limit/timeout a query:

## [LIMIT](/kb/en/select/#limit)

```sql
SELECT ... LIMIT row_count
or
SELECT ... LIMIT offset, row_count
or
SELECT ... LIMIT row_count OFFSET offset
```

The [LIMIT](/kb/en/select/#limit) clause restricts the number of returned rows.

##### MariaDB starting with [10.0.0](/kb/en/mariadb-1000-release-notes/)

## [LIMIT ROWS EXAMINED](/replication/optimization-and-tuning/query-optimizations/limit-rows-examined)

```sql
SELECT ... LIMIT ROWS EXAMINED rows_limit;
```

Stops the query after 'rows_limit' number of rows have been examined.

## sql_safe_updates

If the [sql_safe_updates](/kb/en/server-system-variables/#sql_safe_updates) variable is set, one can't execute an [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) or [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete)
statement unless one specifies a key constraint in the <code class="fixed" style="white-space:pre-wrap">WHERE</code> clause or provide a <code class="fixed" style="white-space:pre-wrap">LIMIT</code> clause (or both).

```sql
SET @@SQL_SAFE_UPDATES=1
UPDATE tbl_name SET not_key_column=val;
-> ERROR 1175 (HY000): You are using safe update mode 
  and you tried to update a table without a WHERE that uses a KEY column
```

## sql_select_limit

[sql_select_limit](/kb/en/server-system-variables/#sql_select_limit) acts as an automatic <code class="fixed" style="white-space:pre-wrap">LIMIT row_count</code> to any [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) query.

```sql
SET @@SQL_SELECT_LIMIT=1000
SELECT * from big_table;
```

The above is the same as:

```sql
SELECT * from big_table LIMIT 1000;
```

## max_join_size

If the [max_join_size](/kb/en/server-system-variables/#max_join_size) variable (also called `sql_max_join_size`) is set, then it will limit
any SELECT statements that probably need to examine more than 
<code class="fixed" style="white-space:pre-wrap">MAX_JOIN_SIZE</code> rows.

```sql
SET @@MAX_JOIN_SIZE=1000;
SELECT count(null_column) from big_table;
->ERROR 1104 (42000): The SELECT would examine more than MAX_JOIN_SIZE rows; 
  check your WHERE and use SET SQL_BIG_SELECTS=1 or SET MAX_JOIN_SIZE=# if the SELECT is okay
```

##### MariaDB starting with [10.1.1](/kb/en/mariadb-1011-release-notes/)

## max_statement_time

If the [max_statement_time](/kb/en/server-system-variables/#max_statement_time) variable is set, any query (excluding stored procedures) taking longer than the value of `max_statement_time` (specified in seconds) to execute will be aborted. This can be set globally, by session, as well as per user and per query. See [Aborting statements that take longer than a certain time to execute](/kb/en/aborting-statements-that-take-longer-than-a-certain-time-to-execute/).

## See Also

- [WAIT and NOWAIT](/sql-statements-structure/sql-statements/transactions/wait-and-nowait)
- [Aborting statements that take longer than a certain time to execute](/replication/optimization-and-tuning/query-optimizations/aborting-statements)
- [lock_wait_timeout](/kb/en/server-system-variables/#lock_wait_timeout) variable