# LOAD INDEX

## Syntax

```sql
LOAD INDEX INTO CACHE
  tbl_index_list [, tbl_index_list] ...

tbl_index_list:
  tbl_name
    [[INDEX|KEY] (index_name[, index_name] ...)]
    [IGNORE LEAVES]
```

## Description

The <code class="fixed" style="white-space:pre-wrap">LOAD INDEX INTO CACHE</code> statement preloads a table index into the key
cache to which it has been assigned by an explicit [<code class="fixed" style="white-space:pre-wrap">CACHE INDEX</code>](/sql-statements-structure/sql-statements/administrative-sql-statements/cache-index)
statement, or into the default key cache otherwise. 
<code class="fixed" style="white-space:pre-wrap">LOAD INDEX INTO CACHE</code> is used only for [MyISAM](/kb/en/myisam/) or [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) tables. Until [MariaDB 5.3](/kb/en/what-is-mariadb-53/), it was not supported for tables
having user-defined partitioning, but this limitation was removed in [MariaDB 5.5](/kb/en/what-is-mariadb-55/).

The <code class="fixed" style="white-space:pre-wrap">IGNORE LEAVES</code> modifier causes only blocks for the nonleaf nodes of
the index to be preloaded.