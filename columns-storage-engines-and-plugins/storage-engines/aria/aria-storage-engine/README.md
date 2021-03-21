# Aria Storage Engine

The [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) storage engine is compiled in by default from [MariaDB 5.1](/kb/en/what-is-mariadb-51/) and it is required to be 'in use' when mysqld is started.

From [MariaDB 10.4](/kb/en/what-is-mariadb-104/), all [system tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/) are Aria.

Additionally, internal on-disk tables are in the Aria table format instead of
the [MyISAM](/kb/en/myisam/) table format. This should speed up some [GROUP BY](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/group-by/)
and [DISTINCT](/built-in-functions/aggregate-functions/count-distinct/) queries because Aria has better caching than
MyISAM.

Note: The <strong><em>Aria</em></strong> storage engine was previously called <em><strong>Maria</strong></em> (see
[The Aria Name](/columns-storage-engines-and-plugins/storage-engines/aria/the-aria-name/) for details on the
rename) and in previous versions of [MariaDB](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/) the engine was still called
Maria.

The following table options to Aria tables in [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) and [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/)#:

- <strong>`TRANSACTIONAL= 0 <code>|` 1</code> :</strong> If the `TRANSACTIONAL` table option is set for an Aria table, then the table will be crash-safe. This is implemented by logging any changes to the table to Aria's transaction log, and syncing those writes at the end of the statement. This will marginally slow down writes and updates. However, the benefit is that if the server dies before the statement ends, all non-durable changes will roll back to the state at the beginning of the statement. This also needs up to 6 bytes more for each row and key to store the transaction id (to allow concurrent insert's and selects).
<ul start="1"><li>`TRANSACTIONAL=1` is not supported for partitioned tables.
</li><li>An Aria table's default value for the `TRANSACTIONAL` table option depends on the table's value for the `ROW_FORMAT` table option. See below for more details.
</li><li>If the `TRANSACTIONAL` table option is set for an Aria table, the table does not actually support transactions. See [MDEV-21364](https://jira.mariadb.org/browse/MDEV-21364) for more information. In this context, <em>transactional</em> just means <em>crash-safe</em>.
</li></ul>
- <strong>`PAGE_CHECKSUM= 0 <code>|` 1</code> :</strong> If index and data should use
  page checksums for extra safety.
- <strong>`TABLE_CHECKSUM= 0 <code>|` 1</code> :</strong>
  Same as `CHECKSUM` in MySQL 5.1
- <strong>`ROW_FORMAT=PAGE <code>|` FIXED `|` DYNAMIC</code> :</strong> The table's [row format](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-formats/).
<ul start="1"><li>The default value is `PAGE`.
</li><li>To emulate MyISAM, set `ROW_FORMAT=FIXED` or `ROW_FORMAT=DYNAMIC`
</li></ul>

The `TRANSACTIONAL` and `ROW_FORMAT` table options interact as follows:

- If `TRANSACTIONAL=1` is set, then the only supported row format is `PAGE`. If `ROW_FORMAT` is set to some other value, then Aria issues a warning, but still forces the row format to be `PAGE`.
- If `TRANSACTIONAL=0` is set, then the table will be not be crash-safe, and any row format is supported.
- If `TRANSACTIONAL` is not set to any value, then any row format is supported. If `ROW_FORMAT` is set, then the table will use that row format. Otherwise, the table will use the default `PAGE` row format. In this case, if the table uses the `PAGE` row format, then it will be crash-safe. If it uses some other row format, then it will not be crash-safe.

Some other improvements are:

- [CHECKSUM TABLE](/sql-statements-structure/sql-statements/table-statements/checksum-table/) now ignores values in NULL fields. This
  makes `CHECKSUM TABLE` faster and fixes some cases where
  same table definition could give different checksum values depending on [row
  format](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-formats/). The disadvantage is that the value is now different compared to other
  MySQL installations. The new checksum calculation is fixed for all table
  engines that uses the default way to calculate and MyISAM which does the
  calculation internally. Note: Old MyISAM tables with internal checksum will
  return the same checksum as before. To fix them to calculate according to new
  rules you have to do an [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/). You can use the old
  ways to calculate checksums by using the option <code class="fixed" style="white-space:pre-wrap">--old</code> to mysqld or set the
  system variable '`@@old`' to `1` when you
  do `CHECKSUM TABLE ...  EXTENDED;`
- At startup Aria will check the Aria logs and automatically recover the tables
  from the last checkpoint if mysqld was not taken down correctly.

## mysqld Startup Options for Aria

For a full list, see [Aria System Variables](/kb/en/aria-server-system-variables/).

In normal operations, the only variables you have to consider are:

- [aria-pagecache-buffer-size](/kb/en/aria-server-system-variables/#aria_pagecache_buffer_size)
<ul start="1"><li>This is where all index and data pages are cached. The bigger this is, the faster
   Aria will work.
</li></ul>
- [aria-block-size](%5Baria-server-system-variables#aria_block_size)
<ul start="1"><li>The default value 8192, should be ok for most cases. The only problem with a higher
   value is that it takes longer to find a packed key in the block as one has to
   search roughly 8192/2 to find each key.  We plan to fix this by adding a
   dictionary at the end of the page to be able to do a binary search within
   the block before starting a scan.  Until this is done and key lookups takes
   too long time even if you are not hitting disk, then you should consider
   making this smaller.
</li><li>Possible values to try are `2048`, `4096` or `8192`
</li><li>Note that you can't change this without dumping, deleting old tables and
   deleting all log files and then restoring your Aria tables. (This is the
   only option that requires a dump and load)
</li></ul>
- [aria-log-purge-type](/kb/en/aria-server-system-variables/#aria_log_purge_type)
<ul start="1"><li>Set this to "`at_flush`" if you want to keep a copy of the transaction logs
   (good as an extra backup). The logs will stay around until you
   execute [FLUSH ENGINE LOGS](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/).
</li></ul>

## See Also

- [Aria FAQ](/columns-storage-engines-and-plugins/storage-engines/aria/aria-faq/)