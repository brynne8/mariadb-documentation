# JSON_EXTRACT

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

JSON functions were added in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

## Syntax

```sql
JSON_EXTRACT(json_doc, path[, path] ...)
```

## Description

Extracts data from a JSON document. The extracted data is selected from the parts matching the path arguments. Returns all matched values; either as a single matched value, or, if the arguments could return multiple values, a result autowrapped as an array in the matching order.

Returns NULL if no paths match or if any of the arguments are NULL.

An error will occur if any path argument is not a valid path, or if the json_doc argument is not a valid JSON document.

The path expression be a [JSONPath expression](/built-in-functions/special-functions/json-functions/jsonpath-expressions/) as supported by MariaDB

## Examples

```sql
SET @json = '[1, 2, [3, 4]]';

SELECT JSON_EXTRACT(@json, '$[1]');
+-----------------------------+
| JSON_EXTRACT(@json, '$[1]') |
+-----------------------------+
| 2                           |
+-----------------------------+

SELECT JSON_EXTRACT(@json, '$[2]');
+-----------------------------+
| JSON_EXTRACT(@json, '$[2]') |
+-----------------------------+
| [3, 4]                      |
+-----------------------------+

SELECT JSON_EXTRACT(@json, '$[2][1]');
+--------------------------------+
| JSON_EXTRACT(@json, '$[2][1]') |
+--------------------------------+
| 4                              |
+--------------------------------+
```

## See Also

- [JSON video tutorial](https://www.youtube.com/watch?v=sLE7jPETp8g) covering JSON_EXTRACT.