# last_insert_grn_id

## Syntax

```sql
last_insert_grn_id()
```

## Description

`last_insert_grn_id` is a [user-defined function](/programming-customizing-mariadb/user-defined-functions/) (UDF) included with the [Mroonga storage engine](/columns-storage-engines-and-plugins/storage-engines/mroonga/). It returns the unique Groonga id of the last-inserted record. See [Creating Mroonga User-Defined Functions](/columns-storage-engines-and-plugins/storage-engines/mroonga/mroonga-user-defined-functions/creating-mroonga-user-defined-functions/) for details on creating this UDF if required.

## Examples

```sql
SELECT last_insert_grn_id();
+----------------------+
| last_insert_grn_id() |
+----------------------+
|                    3 |
+----------------------+
```

## See Also

- [Creating Mroonga User-Defined Functions](/columns-storage-engines-and-plugins/storage-engines/mroonga/mroonga-user-defined-functions/creating-mroonga-user-defined-functions/)