# JSON_DETAILED

##### MariaDB starting with [10.2.4](/kb/en/mariadb-1024-release-notes/)

This function was added in [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/).

## Syntax

```sql
JSON_DETAILED(json_doc[, tab_size])
```

## Description

Represents JSON in the most understandable way emphasizing nested structures.

## Example

```sql
SET @j = '{ "A":1,"B":[2,3]}';

SELECT @j;
+--------------------+
| @j                 |
+--------------------+
| { "A":1,"B":[2,3]} |
+--------------------+

SELECT JSON_DETAILED(@j);
+------------------------------------------------------------+
| JSON_DETAILED(@j)                                          |
+------------------------------------------------------------+
| {
    "A": 1,
    "B": 
    [
        2,
        3
    ]
} |
+------------------------------------------------------------+
```

## See Also

- [JSON video tutorial](https://www.youtube.com/watch?v=sLE7jPETp8g) covering JSON_DETAILED.