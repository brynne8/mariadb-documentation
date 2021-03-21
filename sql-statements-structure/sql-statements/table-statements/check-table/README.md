# CHECK TABLE

## Syntax

```sql
CHECK TABLE tbl_name [, tbl_name] ... [option] ...

option = {FOR UPGRADE | QUICK | FAST | MEDIUM | EXTENDED | CHANGED}
```

## Description

`CHECK TABLE` checks a table or tables for errors. `CHECK TABLE` works for
[Archive](/columns-storage-engines-and-plugins/storage-engines/archive/), [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/), [CSV](/columns-storage-engines-and-plugins/storage-engines/csv/), [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb/), and [MyISAM](/kb/en/myisam/) tables. For Aria and MyISAM tables, the
key statistics are updated as well. For CSV, see also [Checking and Repairing CSV Tables](/columns-storage-engines-and-plugins/storage-engines/csv/checking-and-repairing-csv-tables/).

As an alternative, [myisamchk](/clients-utilities/myisam-clients-and-utilities/myisamchk/) is a commandline tool for checking MyISAM tables when the tables are not being accessed.

For checking [dynamic columns](/sql-statements-structure/nosql/dynamic-columns/) integrity, <a undefined>COLUMN_CHECK()</a> can be used.

`CHECK TABLE` can also check views for problems, such as tables
that are referenced in the view definition that no longer exist.

`CHECK TABLE` is also supported for partitioned tables. You can
use <code class="highlight fixed" style="white-space:pre-wrap">[ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) ... CHECK PARTITION</code> 
to check one or more partitions.

The meaning of the different options are as follows - note that this can vary a bit between
storage engines:

<table><tbody><tr><td>FOR UPGRADE</td><td>Do a very quick check if the storage format for the table has changed so that one needs to do a REPAIR. This is only needed when one upgrades between major versions of MariaDB or MySQL. This is usually done by <a href="/kb/en/upgrading-to-mariadb-from-mysql/">running mysql_upgrade</a>.</td></tr>
<tr><td>FAST</td><td>Only check tables that has not been closed properly or are marked as corrupt. Only supported by the MyISAM and Aria engines. For other  engines the table is checked normally</td></tr>
<tr><td>CHANGED</td><td>Check only tables that has changed since last REPAIR / CHECK. Only supported by the MyISAM and Aria engines. For other  engines the table is checked normally.</td></tr>
<tr><td>QUICK</td><td>Do a fast check. For MyISAM and Aria engine this means we skip checking the delete link chain which may take some time.</td></tr>
<tr><td>MEDIUM</td><td>Scan also the data files. Checks integrity between data and index files with checksums. In most cases this should find all possible errors.</td></tr>
<tr><td>EXTENDED</td><td>Does a full check to verify every possible error. For MyISAM and Aria we verify for each row that all it keys exists and points to the row. This may take a long time on big tables!</td></tr>
</tbody></table>

For most cases running `CHECK TABLE` without options or `MEDIUM` should be
good enough.

The [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine supports [progress reporting](/kb/en/progress-reporting/) for this statement.

If you want to know if two tables are identical, take a look
at <code class="highlight fixed" style="white-space:pre-wrap">[CHECKSUM TABLE](/sql-statements-structure/sql-statements/table-statements/checksum-table/)</code>.

## XtraDB/InnoDB

If `CHECK TABLE` finds an error in an InnoDB table, MariaDB might shutdown to prevent the error propagation. In this case, the problem will be reported in the error log. Otherwise, since [MariaDB 5.5](/kb/en/what-is-mariadb-55/), the table or an index might be marked as corrupted, to prevent use. This does not happen with some minor problems, like a wrong number of entries in a secondary index. Those problems are reported in the output of `CHECK TABLE`.

Each  tablespace contains a header with metadata. This header is not checked by this statement.

During the execution of `CHECK TABLE`, other threads may be blocked.