# JSON_SET

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

JSON functions were added in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

## Syntax

```sql
JSON_SET(json_doc, path, val[, path, val] ...)
```

## Description

Updates or inserts data into a JSON document, returning the result, or NULL if any of the arguments are NULL or the optional path fails to find an object.

An error will occur if the JSON document is invalid, the path is invalid or if the path contains a * or <strong> wildcard.</strong>

JSON_SET can update or insert data, while [JSON_REPLACE](/built-in-functions/special-functions/json-functions/json_replace) can only update, and [JSON_INSERT](/built-in-functions/special-functions/json-functions/json_insert) only insert.

## Examples

```sql
SELECT JSON_SET(Priv, '$.locked', 'true') FROM mysql.global_priv
```