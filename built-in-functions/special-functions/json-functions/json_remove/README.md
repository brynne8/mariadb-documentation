# JSON_REMOVE

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

JSON functions were added in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

## Syntax

```sql
JSON_REMOVE(json_doc, path[, path] ...)
```

## Description

Removes data from a JSON document returning the result, or NULL if any of the arguments are null. If the element does not exist in the document, no changes are made.

An error will occur if JSON document is invalid, the path is invalid or if the path contains a `*` or `**` wildcard.

Path arguments are evaluated from left to right, with the result from the earlier evaluation being used as the value for the next.

## Examples

```sql
SELECT JSON_REMOVE('{"A": 1, "B": 2, "C": {"D": 3}}', '$.C');
+-------------------------------------------------------+
| JSON_REMOVE('{"A": 1, "B": 2, "C": {"D": 3}}', '$.C') |
+-------------------------------------------------------+
| {"A": 1, "B": 2}                                      |
+-------------------------------------------------------+

SELECT JSON_REMOVE('["A", "B", ["C", "D"], "E"]', '$[1]');
+----------------------------------------------------+
| JSON_REMOVE('["A", "B", ["C", "D"], "E"]', '$[1]') |
+----------------------------------------------------+
| ["A", ["C", "D"], "E"]                             |
+----------------------------------------------------+
```

## See Also

- [JSON video tutorial](https://www.youtube.com/watch?v=sLE7jPETp8g) covering JSON_REMOVE.