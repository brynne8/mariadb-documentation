# SPIDER_FLUSH_TABLE_MON_CACHE

## Syntax

```sql
SPIDER_FLUSH_TABLE_MON_CACHE()
```

## Description

A [UDF](/programming-customizing-mariadb/user-defined-functions/) installed with the [Spider Storage Engine](/columns-storage-engines-and-plugins/storage-engines/spider/), this function is used for refreshing monitoring server information. It returns a value of `1`.

## Examples

```sql
SELECT SPIDER_FLUSH_TABLE_MON_CACHE();
+--------------------------------+
| SPIDER_FLUSH_TABLE_MON_CACHE() |
+--------------------------------+
|                              1 |
+--------------------------------+
```