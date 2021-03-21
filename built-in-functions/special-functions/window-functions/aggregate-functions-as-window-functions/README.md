# Aggregate Functions as Window Functions

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

[Window functions](/built-in-functions/special-functions/window-functions) were first introduced in [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/).

It is possible to use [aggregate functions](/built-in-functions/aggregate-functions) as window functions. An aggregate function used as a window function must have the `OVER` clause. For example, here's [COUNT()](/built-in-functions/aggregate-functions/count) used as a window function:

```sql
select COUNT(*) over  (order by column) from table;
```

MariaDB currently allows these aggregate functions to be used as window functions:

- [AVG](/built-in-functions/aggregate-functions/avg)
- [BIT_AND](/built-in-functions/aggregate-functions/bit_and)
- [BIT_OR](/built-in-functions/aggregate-functions/bit_or)
- [BIT_XOR](/built-in-functions/aggregate-functions/bit_xor)
- [COUNT](/built-in-functions/aggregate-functions/count)
- [JSON_ARRAYAGG](/built-in-functions/special-functions/json-functions/json_arrayagg)
- [JSON_OBJECTAGG](/built-in-functions/special-functions/json-functions/json_objectagg)
- [MAX](/built-in-functions/aggregate-functions/max)
- [MIN](/built-in-functions/aggregate-functions/min)
- [STD](/built-in-functions/aggregate-functions/std)
- [STDDEV](/built-in-functions/aggregate-functions/stddev)
- [STDDEV_POP](/built-in-functions/aggregate-functions/stddev_pop)
- [STDDEV_SAMP](/built-in-functions/aggregate-functions/stddev_samp)
- [SUM](/built-in-functions/aggregate-functions/sum)
- [VAR_POP](/built-in-functions/aggregate-functions/var_pop)
- [VAR_SAMP](/built-in-functions/aggregate-functions/var_samp)
- [VARIANCE](/built-in-functions/aggregate-functions/variance)