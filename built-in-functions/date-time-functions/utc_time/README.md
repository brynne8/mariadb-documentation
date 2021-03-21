# UTC_TIME

## Syntax

```sql
UTC_TIME
UTC_TIME([precision])
```

## Description

Returns the current [UTC time](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/coordinated-universal-time) as a value in 'HH:MM:SS' or HHMMSS.uuuuuu format, depending on whether the function is used in a string or numeric context.

The optional <em>precision</em> determines the microsecond precision. See [Microseconds in MariaDB](/built-in-functions/date-time-functions/microseconds-in-mariadb).

## Examples

```sql
SELECT UTC_TIME(), UTC_TIME() + 0;
+------------+----------------+
| UTC_TIME() | UTC_TIME() + 0 |
+------------+----------------+
| 17:32:34   |  173234.000000 |
+------------+----------------+
```

With precision:

```sql
SELECT UTC_TIME(5);
+----------------+
| UTC_TIME(5)    |
+----------------+
| 07:52:50.78369 |
+----------------+
```

## See Also

- [Microseconds in MariaDB](/built-in-functions/date-time-functions/microseconds-in-mariadb)