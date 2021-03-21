# UTC_TIMESTAMP

## Syntax

```sql
UTC_TIMESTAMP
UTC_TIMESTAMP([precision])
```

## Description

Returns the current [UTC](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/coordinated-universal-time/) date and time as a value in 'YYYY-MM-DD
HH:MM:SS' or YYYYMMDDHHMMSS.uuuuuu format, depending on whether the
function is used in a string or numeric context.

The optional <em>precision</em> determines the microsecond precision. See [Microseconds in MariaDB](/built-in-functions/date-time-functions/microseconds-in-mariadb/).

## Examples

```sql
SELECT UTC_TIMESTAMP(), UTC_TIMESTAMP() + 0;
+---------------------+-----------------------+
| UTC_TIMESTAMP()     | UTC_TIMESTAMP() + 0   |
+---------------------+-----------------------+
| 2010-03-27 17:33:16 | 20100327173316.000000 |
+---------------------+-----------------------+
```

With precision:

```sql
SELECT UTC_TIMESTAMP(4);
+--------------------------+
| UTC_TIMESTAMP(4)         |
+--------------------------+
| 2018-07-10 07:51:09.1019 |
+--------------------------+
```

## See Also

- [Time Zones](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones/)
- [Microseconds in MariaDB](/built-in-functions/date-time-functions/microseconds-in-mariadb/)