# SET STATEMENT

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

Per-query variables were introduced in [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

`SET STATEMENT` can be used to set the value of a system variable for the duration of the statement. It is also possible to set multiple variables.

## Syntax

```sql
SET STATEMENT var1=value1 [, var2=value2, ...] 
  FOR <statement>
```

where `varN` is a system variable (list of allowed variables is provided below),  and `valueN` is a constant literal.

## Description

`SET STATEMENT var1=value1 FOR stmt`

is roughly equivalent to

```sql
SET @save_value=@@var1;
SET SESSION var1=value1;
stmt;
SET SESSION var1=@save_value;
```

The server parses the whole statement before executing it, so any variables set in this fashion that affect the parser may not have the expected effect. Examples include the charset variables, sql_mode=ansi_quotes, etc.

## Examples

One can limit statement execution time <a undefined>max_statement_time</a>:

```sql
SET STATEMENT max_statement_time=1000 FOR SELECT ... ;
```

One can switch on/off individual optimizations:

```sql
SET STATEMENT optimizer_switch='materialization=off' FOR SELECT ....;
```

It is possible to enable MRR/BKA for a query:

```sql
SET STATEMENT  join_cache_level=6, optimizer_switch='mrr=on'  FOR SELECT ...
```

Note that it makes no sense to try to set a session variable inside a `SET STATEMENT`:

```sql
#USELESS STATEMENT
SET STATEMENT sort_buffer_size = 100000 for SET SESSION sort_buffer_size = 200000;
```

For the above, after setting sort_buffer_size to 200000 it will be reset to its original state (the state before the `SET STATEMENT` started) after the statement execution.

## Limitations

There are a number of variables that cannot be set on per-query basis. These include:

- `autocommit`
- `character_set_client`
- `character_set_connection`
- `character_set_filesystem`
- `collation_connection`
- `default_master_connection`
- `debug_sync`
- `interactive_timeout`
- `gtid_domain_id`
- `last_insert_id`
- `log_slow_filter`
- `log_slow_rate_limit`
- `log_slow_verbosity`
- `long_query_time`
- `min_examined_row_limit`
- `profiling`
- `profiling_history_size`
- `query_cache_type`
- `rand_seed1`
- `rand_seed2`
- `skip_replication`
- `slow_query_log`
- `sql_log_off`
- `tx_isolation`
- `wait_timeout`

## Source

- The feature was originally implemented as a Google Summer of Code 2009 project by Joseph Lukas.
- Percona Server 5.6 included it as [Per-query variable statement](http://www.percona.com/doc/percona-server/5.6/flexibility/per_query_variable_statement.html)
- MariaDB ported the patch and fixed <em>many</em> bugs. The task in MariaDB Jira is [MDEV-5231](https://jira.mariadb.org/browse/MDEV-5231).