# JSON_INSERT

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

JSON functions were added in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

## Syntax

```sql
JSON_INSERT(json_doc, path, val[, path, val] ...)
```

## Description

Inserts data into a JSON document, returning the resulting document or NULL if any argument is null.

An error will occur if the JSON document is not invalid, or if any of the paths are invalid or contain a `*` or `**` wildcard.

JSON_INSERT can only insert data while [JSON_REPLACE](/built-in-functions/special-functions/json-functions/json_replace) can only update. [JSON_SET](/built-in-functions/special-functions/json-functions/json_set) can update or insert data.

## Examples

```sql
SET @json = '{ "A": 0, "B": [1, 2]}';

SELECT JSON_INSERT(@json, '$.C', '[3, 4]');
+--------------------------------------+
| JSON_INSERT(@json, '$.C', '[3, 4]')  |
+--------------------------------------+
| { "A": 0, "B": [1, 2], "C":"[3, 4]"} |
+--------------------------------------+
```

## See Also

- [JSON video tutorial](https://www.youtube.com/watch?v=sLE7jPETp8g) covering JSON_INSERT.