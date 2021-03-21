# JSON_COMPACT

##### MariaDB starting with [10.2.4](/kb/en/mariadb-1024-release-notes/)

This function was added in [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/).

## Syntax

```sql
JSON_COMPACT(json_doc)
```

## Description

Removes all unnecessary spaces so the json document is as short as possible.

## Example

```sql
SET @j = '{ "A": 1, "B": [2, 3]}';

SELECT JSON_COMPACT(@j), @j;
+-------------------+------------------------+
| JSON_COMPACT(@j)  | @j                     |
+-------------------+------------------------+
| {"A":1,"B":[2,3]} | { "A": 1, "B": [2, 3]} |
+-------------------+------------------------+
```

## See Also

- [JSON video tutorial](https://www.youtube.com/watch?v=sLE7jPETp8g) covering JSON_COMPACT.