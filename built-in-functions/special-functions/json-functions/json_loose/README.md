# JSON_LOOSE

##### MariaDB starting with [10.2.4](/kb/en/mariadb-1024-release-notes/)

This function was added in [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/).

## Syntax

```sql
JSON_LOOSE(json_doc)
```

## Description

Adds spaces to a JSON document to make it look more readable.

## Example

```sql
SET @j = '{ "A":1,"B":[2,3]}';

SELECT JSON_LOOSE(@j), @j;
+-----------------------+--------------------+
| JSON_LOOSE(@j)        | @j                 |
+-----------------------+--------------------+
| {"A": 1, "B": [2, 3]} | { "A":1,"B":[2,3]} |
+-----------------------+--------------------+
```