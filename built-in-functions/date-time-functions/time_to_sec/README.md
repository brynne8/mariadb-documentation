# TIME_TO_SEC

## Syntax

```sql
TIME_TO_SEC(time)
```

## Description

Returns the time argument, converted to seconds.

The value returned by `TIME_TO_SEC` is of type [DOUBLE](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/double). Before [MariaDB 5.3](/kb/en/what-is-mariadb-53/) (and MySQL 5.6), the type was [INT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/int). The returned value preserves microseconds of the argument. See also [Microseconds in MariaDB](/built-in-functions/date-time-functions/microseconds-in-mariadb).

## Examples

```sql
SELECT TIME_TO_SEC('22:23:00');
+-------------------------+
| TIME_TO_SEC('22:23:00') |
+-------------------------+
|                   80580 |
+-------------------------+
```

```sql
SELECT TIME_TO_SEC('00:39:38');
+-------------------------+
| TIME_TO_SEC('00:39:38') |
+-------------------------+
|                    2378 |
+-------------------------+
```

```sql
SELECT TIME_TO_SEC('09:12:55.2355');
+------------------------------+
| TIME_TO_SEC('09:12:55.2355') |
+------------------------------+
|                   33175.2355 |
+------------------------------+
1 row in set (0.000 sec)
```