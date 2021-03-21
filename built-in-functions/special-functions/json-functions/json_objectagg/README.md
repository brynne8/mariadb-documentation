# JSON_OBJECTAGG

##### MariaDB starting with [10.5.0](/kb/en/mariadb-1050-release-notes/)

JSON_OBJECTAGG was added in [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/).

## Syntax

```sql
JSON_OBJECTAGG(key, value)
```

## Description

`JSON_OBJECTAGG` returns a JSON object containing key-value pairs. It takes two expressions that evaluate to a single value, or two column names, as arguments, the first used as a key, and the second as a value.

Returns NULL in the case of an error, or if the result contains no rows.

`JSON_OBJECTAGG` cannot currently be used as a [window function](/built-in-functions/special-functions/window-functions).

## Examples

```sql
select * from t1;
+------+-------+
| a    | b     |
+------+-------+
|    1 | Hello |
|    1 | World |
|    2 | This  |
+------+-------+

SELECT JSON_OBJECTAGG(a, b) FROM t1;
+----------------------------------------+
| JSON_OBJECTAGG(a, b)                   |
+----------------------------------------+
| {"1":"Hello", "1":"World", "2":"This"} |
+----------------------------------------+
```