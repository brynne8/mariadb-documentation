# COLUMN_JSON

##### MariaDB starting with [10.0.1](/kb/en/mariadb-1001-release-notes/)

COLUMN_JSON was introduced in [MariaDB 10.0.1](/kb/en/mariadb-1001-release-notes/)

## Syntax

```sql
COLUMN_JSON(dyncol_blob)
```

## Description

Returns a JSON representation of data in `dyncol_blob`. Can also be used to display nested columns. See [dynamic columns](/sql-statements-structure/nosql/dynamic-columns/) for more information.

## Example

```sql
select item_name, COLUMN_JSON(dynamic_cols) from assets;
+-----------------+----------------------------------------+
| item_name       | COLUMN_JSON(dynamic_cols)              |
+-----------------+----------------------------------------+
| MariaDB T-shirt | {"size":"XL","color":"blue"}           |
| Thinkpad Laptop | {"color":"black","warranty":"3 years"} |
+-----------------+----------------------------------------+
```

Limitation: `COLUMN_JSON` will decode nested dynamic columns at a nesting level of not more than 10 levels deep. Dynamic columns that are nested deeper than 10 levels will be shown as BINARY string, without encoding.