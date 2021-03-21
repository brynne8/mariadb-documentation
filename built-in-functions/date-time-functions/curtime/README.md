# CURTIME

## Syntax

```sql
CURTIME([precision])
```

## Description

Returns the current time as a value in 'HH:MM:SS' or HHMMSS.uuuuuu format, depending on whether the function is used in a string or numeric context. The value is expressed in the current [time zone](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones/).

The optional <em>precision</em> determines the microsecond precision. See [Microseconds in MariaDB](/built-in-functions/date-time-functions/microseconds-in-mariadb/).

## Examples

```sql
SELECT CURTIME();
+-----------+
| CURTIME() |
+-----------+
| 12:45:39  |
+-----------+

SELECT CURTIME() + 0;
+---------------+
| CURTIME() + 0 |
+---------------+
| 124545.000000 |
+---------------+
```

With precision:

```sql
SELECT CURTIME(2);
+-------------+
| CURTIME(2)  |
+-------------+
| 09:49:08.09 |
+-------------+
```

## See Also

- [Microseconds in MariaDB](/built-in-functions/date-time-functions/microseconds-in-mariadb/)