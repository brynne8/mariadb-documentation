# Microseconds in MariaDB

The [TIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/time/), [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime/), and [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp/) types, along with
the temporal functions, [CAST](/built-in-functions/string-functions/cast/) and [dynamic columns](/sql-statements-structure/nosql/dynamic-columns/), support microseconds. The datetime precision of a column can be specified when creating the table with [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/), for example:

```sql
CREATE TABLE example(
  col_microsec DATETIME(6),
  col_millisec TIME(3)
);
```

Generally, the precision can be specified for any `TIME`, `DATETIME`, or `TIMESTAMP` column, in parentheses, after the type name. The datetime precision specifies number of digits after the decimal dot and can be any integer number from 0 to 6. If no precision is specified it is assumed to be 0, for backward compatibility reasons.

A datetime precision can be specified wherever a type name is used. For example:

- when declaring arguments of stored routines.
- when specifying a return type of a stored function.
- when declaring variables.
- in a `CAST` function:<pre class="fixed"><span class="k">create</span> <span class="n">function</span> <span class="nf">example</span><span class="p">(</span><span class="n">x</span> <span class="kt">datetime</span><span class="p">(</span><span class="mi">5</span><span class="p">))</span> <span class="n">returns</span> <span class="kt">time</span><span class="p">(</span><span class="mi">4</span><span class="p">)</span>
<span class="n">begin</span>
  <span class="k">declare</span> <span class="n">y</span> <span class="kt">timestamp</span><span class="p">(</span><span class="mi">6</span><span class="p">);</span>
  <span class="k">return</span> <span class="nf">cast</span><span class="p">(</span><span class="n">x</span> <span class="k">as</span> <span class="kt">time</span><span class="p">(</span><span class="mi">2</span><span class="p">));</span>
<span class="n">end</span><span class="p">;</span>
</pre>

## Additional Information

- when comparing anything to a temporal value (`DATETIME`, `TIME`, [DATE](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/date/), or `TIMESTAMP`), both values are compared as temporal values, not as strings.
- The [INFORMATION_SCHEMA.COLUMNS table](/kb/en/information-schema-columns-table/) has a new column `DATETIME_PRECISION`
- [NOW()](/built-in-functions/date-time-functions/now/), [CURTIME()](/built-in-functions/date-time-functions/curtime/), [UTC_TIMESTAMP()](/built-in-functions/date-time-functions/utc_timestamp/), [UTC_TIME()](/built-in-functions/date-time-functions/utc_time/), [CURRENT_TIME()](/built-in-functions/date-time-functions/current_time/), [CURRENT_TIMESTAMP()](/built-in-functions/date-time-functions/current_timestamp/), [LOCALTIME()](/built-in-functions/date-time-functions/localtime/) and [LOCALTIMESTAMP()](/built-in-functions/date-time-functions/localtimestamp/) now accept datetime precision as an optional argument. For example:<pre class="fixed"><span class="k">SELECT</span> <span class="nf">CURTIME</span><span class="p">(</span><span class="mi">4</span><span class="p">);</span>
<span class="o">--&gt;</span> <span class="mi">10</span><span class="p">:</span><span class="mi">11</span><span class="p">:</span><span class="mi">12</span><span class="p">.</span><span class="mi">3456</span>
</pre>
- [TIME_TO_SEC()](/built-in-functions/date-time-functions/time_to_sec/) and [UNIX_TIMESTAMP()](/built-in-functions/date-time-functions/unix_timestamp/) preserve microseconds of the argument. These functions will return a [decimal](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/decimal/) number if the result non-zero datetime precision and an [integer](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int/) otherwise (for backward compatibility).<pre class="fixed"><span class="k">SELECT</span> <span class="nf">TIME_TO_SEC</span><span class="p">(</span><span class="s1">'10:10:10.12345'</span><span class="p">);</span>
<span class="o">--&gt;</span> <span class="mi">36610</span><span class="p">.</span><span class="mi">12345</span>
</pre>
- Current versions of this patch fix a bug in the following optimization: in
  certain queries with `DISTINCT` MariaDB can ignore this clause if it can
  prove that all result rows are unique anyway, for example, when a primary key
  is compared with a constant. Sometimes this optimization was applied
  incorrectly, though <span>—</span> for example, when comparing a
  string with a date constant. This is now fixed.
- `DATE_ADD()` and `DATE_SUB()` functions can now take a `TIME`
  expression as an argument (not just `DATETIME` as before).<pre class="fixed"><span class="k">SELECT</span> <span class="kt">TIME</span><span class="p">(</span><span class="s1">'10:10:10'</span><span class="p">)</span> <span class="o">+</span> <span class="k">INTERVAL</span> <span class="mi">100</span> <span class="n">MICROSECOND</span><span class="p">;</span>
<span class="o">--&gt;</span> <span class="mi">10</span><span class="p">:</span><span class="mi">10</span><span class="p">:</span><span class="mi">10</span><span class="p">.</span><span class="mi">000100</span>
</pre>
- The `event_time` field in the [mysql.general_log](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlgeneral_log-table/) table and the `start_time`, `query_time`, and `lock_time` fields in the [mysql.slow_log](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlslow_log-table/) table now store values with microsecond precision.
- This patch fixed a bug when comparing a temporal value using the `BETWEEN` operator and one of the operands is `NULL`.
- The old syntax `TIMESTAMP(N)`, where `N` is the display width, is no longer supported. It was deprecated in MySQL 4.1.0 (released on
  2003-04-03).
- when a `DATETIME` value is compared to a `TIME` value, the latter is treated as a full datetime with a zero date part, similar to comparing `DATE` to a `DATETIME`, or to comparing `DECIMAL` numbers.
  Earlier versions of MariaDB used to compare only the time part of both operands in such a case.
- In MariaDB, an extra column [TIME_MS](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/time_ms-column-in-information_schemaprocesslist/) has been added to the <a undefined>INFORMATION_SCHEMA.PROCESSLIST</a> table, as well as to the output of [SHOW FULL PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/).

<strong>Note:</strong> When you convert a temporal value to a value with a smaller
precision, it will be truncated, not rounded. This is done to guarantee that the date part is not
changed. For example:

```sql
SELECT CAST('2009-12-31 23:59:59.998877' as DATETIME(3));
-> 2009-12-31 23:59:59.998
```

## MySQL 5.6 Microseconds

MySQL 5.6 introduced microseconds using a slightly different implementation to [MariaDB 5.3](/kb/en/what-is-mariadb-53/). Since [MariaDB 10.1](/kb/en/what-is-mariadb-101/), MariaDB has defaulted to the MySQL format, by means of the [--mysql56-temporal-format](/kb/en/server-system-variables/#mysql56_temporal_format) variable. The MySQL version requires slightly [more storage](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements/) but has some advantages in permitting the eventual support of negative dates, and in replication.

## See Also

- [Data Type Storage Requirements](/columns-storage-engines-and-plugins/data-types/data-type-storage-requirements/)