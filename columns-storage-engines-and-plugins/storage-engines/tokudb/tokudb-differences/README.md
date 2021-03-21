# TokuDB Differences

TokuDB has been deprecated by its upstream maintainer. It is disabled from [MariaDB 10.5](/kb/en/what-is-mariadb-105/) and has been been removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/) - [MDEV-19780](https://jira.mariadb.org/browse/MDEV-19780). We recommend [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks/) as a long-term migration path.

Because we, the MariaDB developers, don't want to add a lot of new features or big code changes to a stable release, not all TokuDB features will be merged at once into MariaDB. Instead they will be added in stages.

On this page we list all the known differences between the TokuDB from [Tokutek](http://www.tokutek.com) and the default MariaDB version from [MariaDB.org](https://downloads.mariadb.org):

## All MariaDB versions

- TokuDB is not the default storage engine.
<ul start="1"><li>If you want to enable this, you have to start mysqld with: <code class="highlight fixed" style="white-space:pre-wrap">--default-storage-engine=tokudb</code>.
</li></ul>
- Auto increment for second part of a key behaves as documented (and as it does in MyISAM and other storage engines).
- The DDL syntax is different. While binaries from Tokutek have the patched SQL parser, TokuDB in MariaDB uses the special [Storage Engine API extension](/columns-storage-engines-and-plugins/storage-engines/storage-engines-storage-engine-development/engine-defined-new-tablefieldindex-attributes/). Thus in Tokutek binaries you write <code class="fixed" style="white-space:pre-wrap">CLUSTERED KEY (columns)</code> and, for example, <code class="fixed" style="white-space:pre-wrap">ROW_FORMAT=TOKUDB_LZMA</code>. And in MariaDB you write <code class="fixed" style="white-space:pre-wrap">KEY (columns) CLUSTERING=YES</code> and <code class="fixed" style="white-space:pre-wrap">COMPRESSION=TOKUDB_LZMA</code>.

## Features missing in [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

- No online [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/).
<ul start="1"><li>All alter table that changes data or indexes requires a table copy.
</li></ul>
- No online [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/).
- No <code class="highlight fixed" style="white-space:pre-wrap">INSERT NOAR</code> or <code class="highlight fixed" style="white-space:pre-wrap">UPDATE NOAR</code> commands.
- No gdb stack trace on sigsegv
- IMPORTANT: the compression type does not default to the [tokudb_row_format](/kb/en/tokudb-system-variables/#tokudb_row_format) session variable as it does with Tokutek's builds. If  <code class="fixed" style="white-space:pre-wrap">COMPRESSION=</code> is not included in <code class="fixed" style="white-space:pre-wrap">CREATE TABLE</code> or <code class="fixed" style="white-space:pre-wrap">ALTER TABLE ENGINE=TokuDB</code> then the TokuDB table will be uncompressed (before 5.5.37) or zlib-compressed (5.5.37 and later).

## Features missing in [MariaDB 10.0](/kb/en/what-is-mariadb-100/)

[MariaDB 10.0](/kb/en/what-is-mariadb-100/) (starting from 10.0.5) has online [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/). So the features missing will be:

- No <code class="highlight fixed" style="white-space:pre-wrap">INSERT NOAR</code> or <code class="highlight fixed" style="white-space:pre-wrap">UPDATE NOAR</code> commands.
<ul start="1"><li>We are working with Tokutek to improve this feature before adding it to MariaDB.
</li></ul>
- No online [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/) before [10.0.11](/kb/en/mariadb-10011-release-notes/) ([r4199](http://bazaar.launchpad.net/~maria-captains/maria/10.0/revision/4199))
- No gdb stack trace on sigsegv
- Before 10.0.10 the compression type did not default to the [tokudb_row_format](/kb/en/tokudb-system-variables/#tokudb_row_format) session variable. If  <code class="fixed" style="white-space:pre-wrap">COMPRESSION=</code> was not included in <code class="fixed" style="white-space:pre-wrap">CREATE TABLE</code> or <code class="fixed" style="white-space:pre-wrap">ALTER TABLE ENGINE=TokuDB</code> then the TokuDB table was created uncompressed.

## Version of the TokuDB plugin included on MariaDB

This is found on the [TokuDB](/columns-storage-engines-and-plugins/storage-engines/tokudb/) page.