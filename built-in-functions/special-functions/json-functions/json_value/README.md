# JSON_VALUE

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

JSON functions were added in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

## Syntax

```sql
JSON_VALUE(json_doc, path)
```

## Description

Given a JSON document, returns the scalar specified by the path. Returns NULL if not given a valid JSON document, or if there is no match.

## Examples

```sql
select json_value('{"key1":123}', '$.key1');
+--------------------------------------+
| json_value('{"key1":123}', '$.key1') |
+--------------------------------------+
| 123                                  |
+--------------------------------------+

select json_value('{"key1": [1,2,3], "key1":123}', '$.key1');
+-------------------------------------------------------+
| json_value('{"key1": [1,2,3], "key1":123}', '$.key1') |
+-------------------------------------------------------+
| 123                                                   |
+-------------------------------------------------------+
```