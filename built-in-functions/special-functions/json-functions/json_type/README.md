# JSON_TYPE

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

JSON functions were added in [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/).

## Syntax

```sql
JSON_TYPE(json_val)
```

## Description

Returns the type of a JSON value, or NULL if the argument is null.

An error will occur if the argument is an invalid JSON value.

The following is a complete list of the possible return types:

<table><tbody><tr><th>Return type</th><th>Value</th></tr>
<tr><td>ARRAY</td><td>JSON array</td></tr>
<tr><td>BIT</td><td>MariaDB BIT scalar</td></tr>
<tr><td>BLOB</td><td>MariaDB binary types (BINARY, VARBINARY or BLOB)</td></tr>
<tr><td>BOOLEAN</td><td>JSON true/false literals</td></tr>
<tr><td>DATE</td><td>MariaDB DATE scalar</td></tr>
<tr><td>DATETIME</td><td>MariaDB DATETIME or TIMESTAMP scalar</td></tr>
<tr><td>DECIMAL</td><td>MariaDB DECIMAL or NUMERIC scalar</td></tr>
<tr><td>DOUBLE</td><td>MariaDB DOUBLE FLOAT scalar</td></tr>
<tr><td>INTEGER</td><td>MariaDB integer types (TINYINT, SMALLINT, MEDIUMINT, INT or BIGINT)</td></tr>
<tr><td>NULL</td><td>JSON null literal or NULL argument</td></tr>
<tr><td>OBJECT</td><td>JSON object</td></tr>
<tr><td>OPAQUE</td><td>Any valid JSON value that is not one of the other types.</td></tr>
<tr><td>STRING</td><td>MariaDB character types (CHAR, VARCHAR, TEXT, ENUM or SET)</td></tr>
<tr><td>TIME</td><td>MariaDB TIME scalar</td></tr>
</tbody></table>

## Examples

```sql
SELECT JSON_TYPE('{"A": 1, "B": 2, "C": 3}');
+---------------------------------------+
| JSON_TYPE('{"A": 1, "B": 2, "C": 3}') |
+---------------------------------------+
| OBJECT                                |
+---------------------------------------+
```