# JSON_CONTAINS

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

JSON functions were added in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

## Syntax

```sql
JSON_CONTAINS(json_doc, val[, path])
```

## Description

Returns whether or not the specified value is found in the given JSON document or, optionally, at the specified path within the document. Returns `1` if it does, `0` if not and NULL if any of the arguments are null. An error occurs if the document or path is not valid, or contains the `*` or `**` wildcards.

## Examples

```sql
SET @json = '{"A": 0, "B": {"C": 1}, "D": 2}';

SELECT JSON_CONTAINS(@json, '2', '$.A');
+----------------------------------+
| JSON_CONTAINS(@json, '2', '$.A') |
+----------------------------------+
|                                0 |
+----------------------------------+

SELECT JSON_CONTAINS(@json, '2', '$.D');
+----------------------------------+
| JSON_CONTAINS(@json, '2', '$.D') |
+----------------------------------+
|                                1 |
+----------------------------------+

SELECT JSON_CONTAINS(@json, '{"C": 1}', '$.A');
+-----------------------------------------+
| JSON_CONTAINS(@json, '{"C": 1}', '$.A') |
+-----------------------------------------+
|                                       0 |
+-----------------------------------------+

SELECT JSON_CONTAINS(@json, '{"C": 1}', '$.B');
+-----------------------------------------+
| JSON_CONTAINS(@json, '{"C": 1}', '$.B') |
+-----------------------------------------+
|                                       1 |
+-----------------------------------------+
```