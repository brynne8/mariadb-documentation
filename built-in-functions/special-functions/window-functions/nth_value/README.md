# NTH_VALUE

##### MariaDB starting with [10.2.2](/kb/en/mariadb-1022-release-notes/)

The NTH_VALUE() function was first introduced with other [window functions](/built-in-functions/special-functions/window-functions) in [MariaDB 10.2](/kb/en/what-is-mariadb-102/).

## Syntax

```sql
NTH_VALUE (expr[, num_row]) OVER ( 
  [ PARTITION BY partition_expression ] 
  [ ORDER BY order_list ]
)
```

## Description

The `NTH_VALUE` function returns the value evaluated at row number `num_row` of the window frame, starting from 1, or NULL if the row does not exist.