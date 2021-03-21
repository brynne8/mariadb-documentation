# SPIDER_BG_DIRECT_SQL

## Syntax

```sql
SPIDER_BG_DIRECT_SQL('sql', 'tmp_table_list', 'parameters')
```

## Description

Executes the given SQL statement in the background on the remote server, as defined in the parameters listing.  If the query returns a result-set, it sttores the results in the given temporary table.  When the given SQL statement executes successfully, this function returns the number of called UDF's.  It returns `0` when the given SQL statement fails.

This function is a [UDF](/programming-customizing-mariadb/user-defined-functions) installed with the [Spider](/columns-storage-engines-and-plugins/storage-engines/spider) storage engine.

## Examples

```sql
SELECT SPIDER_BG_DIRECT_SQL('SELECT * FROM example_table',  '', 
   'srv "node1", port "8607"') AS "Direct Query";
+--------------+
| Direct Query | 
+--------------+
|            1 |
+--------------+
```

## Parameters

#### `error_rw_mode`

- <strong>Description:</strong> Returns empty results on network error.
<ul start="1"><li>`0` : Return error on getting network error.
</li><li>`1`: Return 0 records on getting network error.
</li></ul>
- <strong>Default Table Value:</strong> `0`
- <strong>DSN Parameter Name:</strong> `erwm`

## See also

- [SPIDER_DIRECT_SQL](/columns-storage-engines-and-plugins/storage-engines/spider/spider-functions/spider_direct_sql)