# JSON_ARRAY

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

JSON functions were added in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

## Syntax

```sql
JSON_ARRAY([value[, value2] ...])
```

## Description

Returns a JSON array containing the listed values. The list can be empty.

## Example

```sql
SELECT Json_Array(56, 3.1416, 'My name is "Foo"', NULL);
+--------------------------------------------------+
| Json_Array(56, 3.1416, 'My name is "Foo"', NULL) |
+--------------------------------------------------+
| [56, 3.1416, "My name is \"Foo\"", null]         |
+--------------------------------------------------+
```