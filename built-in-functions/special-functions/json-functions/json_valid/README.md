# JSON_VALID

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

JSON functions were added in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

## Syntax

```sql
JSON_VALID(value)
```

## Description

Indicates whether the given value is a valid JSON document or not. Returns `1` if valid, `0` if not, and NULL if the argument is NULL.

From [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/), the JSON_VALID function is automatically used as a [CHECK constraint](/kb/en/constraint/#check-constraints) for the [JSON data type alias](/columns-storage-engines-and-plugins/data-types/string-data-types/json-data-type) in order to ensure that a valid json document is inserted.

## Examples

```sql
SELECT JSON_VALID('{"id": 1, "name": "Monty"}');
+------------------------------------------+
| JSON_VALID('{"id": 1, "name": "Monty"}') |
+------------------------------------------+
|                                        1 |
+------------------------------------------+

SELECT JSON_VALID('{"id": 1, "name": "Monty", "oddfield"}');
+------------------------------------------------------+
| JSON_VALID('{"id": 1, "name": "Monty", "oddfield"}') |
+------------------------------------------------------+
|                                                    0 |
+------------------------------------------------------+
```

## See Also

- [JSON video tutorial](https://www.youtube.com/watch?v=sLE7jPETp8g) covering JSON_VALID.