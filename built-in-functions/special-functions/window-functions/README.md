# Window Functions

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

Window functions were first introduced in [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/).

Window functions perform calculations across a set of rows related to the current row

- [Window Functions Overview](/built-in-functions/special-functions/window-functions/window-functions-overview/) — Window functions perform calculations across a set of rows related to the current row.
- [AVG](/built-in-functions/aggregate-functions/avg/) — Returns the average value.
- [BIT_AND](/built-in-functions/aggregate-functions/bit_and/) — Bitwise AND.
- [BIT_OR](/built-in-functions/aggregate-functions/bit_or/) — Bitwise OR.
- [BIT_XOR](/built-in-functions/aggregate-functions/bit_xor/) — Bitwise XOR.
- [COUNT](/built-in-functions/aggregate-functions/count/) — Returns count of non-null values.
- [CUME_DIST](/built-in-functions/special-functions/window-functions/cume_dist/) — Window function that returns the cumulative distribution of a given row.
- [DENSE_RANK](/built-in-functions/special-functions/window-functions/dense_rank/) — Rank of a given row with identical values receiving the same result, no skipping.
- [FIRST_VALUE](/built-in-functions/special-functions/window-functions/first_value/) — Returns the first result from an ordered set.
- [JSON_ARRAYAGG](/built-in-functions/special-functions/json-functions/json_arrayagg/) — Returns a JSON array containing an element for each value in a given set of JSON or SQL values.
- [JSON_OBJECTAGG](/built-in-functions/special-functions/json-functions/json_objectagg/) — Returns a JSON object containing key-value pairs.
- [LAG](/built-in-functions/special-functions/window-functions/lag/) — Accesses data from a previous row in the same result set without the need for a self-join.
- [LAST_VALUE](/built-in-functions/secondary-functions/information-functions/last_value/) — Returns the last value in a list or set of values.
- [LEAD](/built-in-functions/special-functions/window-functions/lead/) — Accesses data from a following row in the same result set without the need for a self-join.
- [MAX](/built-in-functions/aggregate-functions/max/) — Returns the maximum value.
- [MEDIAN](/built-in-functions/special-functions/window-functions/median/) — Window function that returns the median value of a range of values.
- [MIN](/built-in-functions/aggregate-functions/min/) — Returns the minimum value.
- [NTH_VALUE](/built-in-functions/special-functions/window-functions/nth_value/) — Returns the value evaluated at the specified row number of the window frame.
- [NTILE](/built-in-functions/special-functions/window-functions/ntile/) — Returns an integer indicating which group a given row falls into.
- [PERCENT_RANK](/built-in-functions/special-functions/window-functions/percent_rank/) — Window function that returns the relative percent rank of a given row.
- [PERCENTILE_CONT](/built-in-functions/special-functions/window-functions/percentile_cont/) — Continuous percentile.
- [PERCENTILE_DISC](/built-in-functions/special-functions/window-functions/percentile_disc/) — Discrete percentile.
- [RANK](/built-in-functions/special-functions/window-functions/rank/) — Rank of a given row with identical values receiving the same result.
- [ROW_NUMBER](/built-in-functions/special-functions/window-functions/row_number/) — Row number of a given row with identical values receiving a different result.
- [STD](/built-in-functions/aggregate-functions/std/) — Population standard deviation.
- [STDDEV](/built-in-functions/aggregate-functions/stddev/) — Population standard deviation.
- [STDDEV_POP](/built-in-functions/aggregate-functions/stddev_pop/) — Returns the population standard deviation.
- [STDDEV_SAMP](/built-in-functions/aggregate-functions/stddev_samp/) — Standard deviation.
- [SUM](/built-in-functions/aggregate-functions/sum/) — Sum total.
- [VAR_POP](/built-in-functions/aggregate-functions/var_pop/) — Population standard variance.
- [VAR_SAMP](/built-in-functions/aggregate-functions/var_samp/) — Returns the sample variance.
- [VARIANCE](/built-in-functions/aggregate-functions/variance/) — Population standard variance.
- [Aggregate Functions as Window Functions](/built-in-functions/special-functions/window-functions/aggregate-functions-as-window-functions/) — It is possible to use aggregate functions as window functions.
- [ColumnStore Window Functions](/built-in-functions/special-functions/window-functions/window-functions-columnstore-window-functions/) — Summary of window function use with the ColumnStore engine
- [Window Frames](/built-in-functions/special-functions/window-functions/window-frames/) — Some window functions operate on window frames.