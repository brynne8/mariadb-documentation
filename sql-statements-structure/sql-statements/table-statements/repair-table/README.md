# REPAIR TABLE

## Syntax

```sql
REPAIR [NO_WRITE_TO_BINLOG | LOCAL] TABLE
    tbl_name [, tbl_name] ...
    [QUICK] [EXTENDED] [USE_FRM]
```

## Description

<code class="fixed" style="white-space:pre-wrap">REPAIR TABLE</code> repairs a possibly corrupted table. By default,
it has the same effect as

```sql
myisamchk --recover tbl_name
```

or

```sql
aria_chk --recover tbl_name
```

See [aria_chk](/clients-utilities/aria-clients-and-utilities/aria_chk) and [myisamchk](/clients-utilities/myisam-clients-and-utilities/myisamchk) for more.

<code class="fixed" style="white-space:pre-wrap">REPAIR TABLE</code> works for [Archive](/columns-storage-engines-and-plugins/storage-engines/archive), [Aria](/columns-storage-engines-and-plugins/storage-engines/aria), [CSV](/columns-storage-engines-and-plugins/storage-engines/csv) and [MyISAM](/kb/en/myisam/) tables. For [XtraDB/InnoDB](/kb/en/xtradb-and-innodb/), see [recovery modes](/kb/en/xtradbinnodb-recovery-modes/). For CSV, see also [Checking and Repairing CSV Tables](/columns-storage-engines-and-plugins/storage-engines/csv/checking-and-repairing-csv-tables). For Archive, this statement also improves compression. If the storage engine does not support this statement, a warning is issued.

This statement requires [SELECT and INSERT privileges](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) for the table.

By default, `REPAIR TABLE` statements are written to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) and will be [replicated](/replication). The `NO_WRITE_TO_BINLOG` keyword (`LOCAL` is an alias) will ensure the statement is not written to the binary log.

When an index is recreated, the storage engine may use a configurable buffer in the process. Incrementing the buffer speeds up the index creation. [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) and [MyISAM](/kb/en/myisam/) allocate a buffer whose size is defined by <a undefined>aria_sort_buffer_size</a> or <a undefined>myisam_sort_buffer_size</a>, also used for [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table).

<code class="fixed" style="white-space:pre-wrap">REPAIR TABLE</code> is also supported for partitioned tables.
However, the <code class="fixed" style="white-space:pre-wrap">USE_FRM</code> option cannot be used with this statement
on a partitioned table.

<code class="highlight fixed" style="white-space:pre-wrap">[ALTER TABLE ... REPAIR PARTITION](/sql-statements-structure/sql-statements/data-definition/alter/alter-table)</code> can be used
to repair one or more partitions.

The [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) storage engine supports [progress reporting](/kb/en/progress-reporting/) for this statement.