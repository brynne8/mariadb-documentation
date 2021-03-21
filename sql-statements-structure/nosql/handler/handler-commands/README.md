# HANDLER Commands

## Syntax

```sql
HANDLER tbl_name OPEN [ [AS] alias]
HANDLER tbl_name READ index_name { = | >= | <= | < } (value1,value2,...)
    [ WHERE where_condition ] [LIMIT ... ]
HANDLER tbl_name READ index_name { FIRST | NEXT | PREV | LAST }
    [ WHERE where_condition ] [LIMIT ... ]
HANDLER tbl_name READ { FIRST | NEXT }
    [ WHERE where_condition ] [LIMIT ... ]
HANDLER tbl_name CLOSE
```

## Description

The `HANDLER` statement provides direct access to table
storage engine interfaces for key lookups and key or table scans. It is available for at least [Aria](/kb/en/aria-formerly-known-as-maria/), [Memory](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine), [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine) and [InnoDB](/kb/en/xtradb-and-innodb/) tables (and should work with most 'normal' storage engines, but not with system tables, [MERGE](/columns-storage-engines-and-plugins/storage-engines/merge) or [views](/programming-customizing-mariadb/views)).

`HANDLER ... OPEN` opens a table, allowing it to be accessible to subsequent `HANDLER ... READ` statements. The table can either be opened using an alias (which must then be used by `HANDLER ... READ`, or a table name.

The table object is only closed when `HANDLER ... CLOSE` is called by the session, and is not shared by other sessions.

[Prepared statements](/sql-statements-structure/sql-statements/prepared-statements) work with `HANDLER READ`, which gives a much higher performance (50% speedup) as there is no parsing and all data is transformed in binary (without conversions to text, as with the normal protocol).

The HANDLER command does not work with [partitioned tables](/kb/en/managing-mariadb-partitioning/).

## Key Lookup

A key lookup is started with:

```sql
HANDLER tbl_name READ index_name { = | >= | <= | < }  (value,value) [LIMIT...]
```

The values stands for the value of each of the key columns. For most key types (except for HASH keys in MEMORY storage engine) you can use a prefix subset of it's columns.

If you are using LIMIT, then in case of &gt;= or &gt; then there is an implicit NEXT implied, while if you are using &lt;= or &lt; then there is an implicit PREV implied.

After the initial read, you can use

```sql
HANDLER tbl_name READ index_name NEXT [ LIMIT ... ]
or
HANDLER tbl_name READ index_name PREV [ LIMIT ... ]
```

to scan the rows in key order.

Note that the row order is not defined for keys with duplicated values and will vary from engine to engine.

## Key Scans

You can scan a table in key order by doing:

```sql
HANDLER tbl_name READ index_name FIRST [ LIMIT ... ]
HANDLER tbl_name READ index_name NEXT  [ LIMIT ... ]
```

or, if the handler supports backwards key scans (most do):

```sql
HANDLER tbl_name READ index_name LAST [ LIMIT ... ]
HANDLER tbl_name READ index_name PREV [ LIMIT ... ]
```

## Table Scans

You can scan a table in row order by doing:

```sql
HANDLER tbl_name READ FIRST [ LIMIT ... ]
HANDLER tbl_name READ NEXT  [ LIMIT ... ]
```

## Limitations

As this is a direct interface to the storage engine, some limitations may apply for what you can do and what happens if the table changes. Here follows some of the common limitations:

### Finding 'Old Rows'

HANDLER READ is not transaction safe, consistent or atomic.  It's ok for the storage engine to returns rows that existed when you started the scan but that were later deleted. This can happen as the storage engine may cache rows as part of the scan from a previous read.

You may also find rows committed since the scan originally started.

### Invisible Columns

`HANDLER ... READ` also reads the data of [invisible-columns](/sql-statements-structure/sql-statements/data-definition/create/invisible-columns).

### System-Versioned Tables

`HANDLER ... READ` reads everything from [system-versioned tables](/sql-statements-structure/temporal-tables/system-versioned-tables), and so includes `row_start` and `row_end` fields, as well as all rows that have since been deleted or changed, including when history partitions are used.

### Other Limitations

- If you do an [ALTER TABLE](/kb/en/alter-table-statement/), all your HANDLER's for that table are automatically closed.
- If you do an ALTER TABLE for a table that is used by some other connection with HANDLER, the ALTER TABLE will wait for the HANDLER to be closed.
- For HASH keys, you must use all key parts when searching for a row.
- For HASH keys, you can't do a key scan of all values. You can only find all rows with the same key value.
- While each HANDLER READ command is atomic, if you do a scan in many steps, then some engines may give you error 1020 if the table changed between the commands. Please refer to the [specific engine handler page](/kb/en/handler-handler/) if this happens.

## Error Codes

- Error 1031 (ER_ILLEGAL_HA) Table storage engine for 't1' doesn't have this option
<ul start="1"><li>If you get this for HANDLER OPEN it means the storage engine doesn't support HANDLER calls.
</li><li>If you get this for HANDLER READ it means you are trying to use an incomplete HASH key.
</li></ul>
- Error 1020 (ER_CHECKREAD) Record has changed since last read in table '...'
<ul start="1"><li>This means that the table changed between two reads and the handler can't handle this case for the given scan.
</li></ul>

## See Also

- [What is MariaDB 5.3](/kb/en/what-is-mariadb-53/)