# UNIX_TIMESTAMP

## Syntax

```sql
UNIX_TIMESTAMP()
UNIX_TIMESTAMP(date)
```

## Description

If called with no argument, returns a Unix timestamp (seconds since
'1970-01-01 00:00:00' [UTC](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/coordinated-universal-time)) as an unsigned integer. If `UNIX_TIMESTAMP()`
is called with a date argument, it returns the value of the argument as seconds
since '1970-01-01 00:00:00' UTC. date may be a [DATE](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/date) string, a
[DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime) string, a [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp), or a number in
the format YYMMDD or YYYYMMDD. The server interprets date as a value in the
current [time zone](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones) and converts it to an internal value in [UTC](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/coordinated-universal-time). Clients can set
their time zone as described in [time zones](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones).

The inverse function of `UNIX_TIMESTAMP()` is [FROM_UNIXTIME()](/built-in-functions/date-time-functions/from_unixtime)

`UNIX_TIMESTAMP()` supports [microseconds](/built-in-functions/date-time-functions/microseconds-in-mariadb).

Timestamps in MariaDB have a maximum value of 2147483647, equivalent to 2038-01-19 05:14:07. This is due to the underlying 32-bit limitation. Using the function on a date beyond this will result in NULL being returned. Use [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime) as a storage type if you require dates beyond this.

### Error Handling

Returns NULL for wrong arguments to `UNIX_TIMESTAMP()`. In MySQL and MariaDB before 5.3 wrong arguments to `UNIX_TIMESTAMP()` returned 0.

### Compatibility

As you can see in the examples above, UNIX_TIMESTAMP(constant-date-string) returns a timestamp with 6 decimals while [MariaDB 5.2](/kb/en/what-is-mariadb-52/) and before returns it without decimals. This can cause a problem if you are using UNIX_TIMESTAMP() as a partitioning function. You can fix this by using [FLOOR](/built-in-functions/numeric-functions/floor)(UNIX_TIMESTAMP(..)) or changing the date string to a date number, like 20080101000000.

## Examples

```sql
SELECT UNIX_TIMESTAMP();
+------------------+
| UNIX_TIMESTAMP() |
+------------------+
|       1269711082 |
+------------------+

SELECT UNIX_TIMESTAMP('2007-11-30 10:30:19');
+---------------------------------------+
| UNIX_TIMESTAMP('2007-11-30 10:30:19') |
+---------------------------------------+
|                     1196436619.000000 |
+---------------------------------------+

SELECT UNIX_TIMESTAMP("2007-11-30 10:30:19.123456");
+----------------------------------------------+
| unix_timestamp("2007-11-30 10:30:19.123456") |
+----------------------------------------------+
|                            1196411419.123456 |
+----------------------------------------------+

SELECT FROM_UNIXTIME(UNIX_TIMESTAMP('2007-11-30 10:30:19'));
+------------------------------------------------------+
| FROM_UNIXTIME(UNIX_TIMESTAMP('2007-11-30 10:30:19')) |
+------------------------------------------------------+
| 2007-11-30 10:30:19.000000                           |
+------------------------------------------------------+

SELECT FROM_UNIXTIME(FLOOR(UNIX_TIMESTAMP('2007-11-30 10:30:19')));
+-------------------------------------------------------------+
| FROM_UNIXTIME(FLOOR(UNIX_TIMESTAMP('2007-11-30 10:30:19'))) |
+-------------------------------------------------------------+
| 2007-11-30 10:30:19                                         |
+-------------------------------------------------------------+
```

## See Also

- [FROM_UNIXTIME()](/built-in-functions/date-time-functions/from_unixtime)