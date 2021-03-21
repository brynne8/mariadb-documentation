# INSERT DELAYED

## Syntax

```sql
INSERT DELAYED ...
```

## Description

The <code class="highlight fixed" style="white-space:pre-wrap">DELAYED</code> option for the [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/)
statement is a MariaDB/MySQL extension to standard SQL that is very useful if you have
clients that cannot or need not wait for the <code class="highlight fixed" style="white-space:pre-wrap">INSERT</code> to
complete. This is a common situation when you use MariaDB for logging and you
also periodically run <code class="highlight fixed" style="white-space:pre-wrap">SELECT</code> and <code class="highlight fixed" style="white-space:pre-wrap">UPDATE</code>
statements that take a long time to complete.

When a client uses <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code>, it gets an okay from the
server at once, and the row is queued to be inserted when the table is not in
use by any other thread.

Another major benefit of using <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> is that
inserts from many clients are bundled together and written in one block. This
is much faster than performing many separate inserts.

Note that <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> is slower than a normal
 <code class="highlight fixed" style="white-space:pre-wrap">INSERT</code> if the table is not otherwise in use. There is also
the additional overhead for the server to handle a separate thread for each
table for which there are delayed rows. This means that you should use
<code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> only when you are really sure that you need
it.

The queued rows are held only in memory until they are inserted into the table.
This means that if you terminate mysqld forcibly (for example, with kill -9) or
if mysqld dies unexpectedly, any queued rows that have not been written to disk
are lost.

The number of concurrent `INSERT DELAYED` threads is limited by the <a undefined>max_delayed_threads</a> server system variables. If it is set to 0, `INSERT DELAYED` is disabled. The session value can be equal to the global value, or 0 to disable this statement for the current session. If this limit has been reached, the `DELAYED` clause will be silently ignore for subsequent statements (no error will be produced).

### Limitations

There are some limitations on the use of <code class="highlight fixed" style="white-space:pre-wrap">DELAYED</code>:

- <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> works only with [MyISAM](/columns-storage-engines-and-plugins/storage-engines/myisam-storage-engine/), [MEMORY](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/memory-storage-engine/), [ARCHIVE](/columns-storage-engines-and-plugins/storage-engines/archive/),
  and [BLACKHOLE](/columns-storage-engines-and-plugins/storage-engines/blackhole/) tables. If you execute INSERT DELAYED with another storage engine, you will get an error like this: `ERROR 1616 (HY000): DELAYED option not supported for table 'tab_name'`
- For MyISAM tables, if there are no free blocks in the middle of the data
  file, concurrent SELECT and INSERT statements are supported. Under these
  circumstances, you very seldom need to use <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code>
  with MyISAM.
- <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> should be used only for
  <code class="highlight fixed" style="white-space:pre-wrap">INSERT</code> statements that specify value lists. The server
  ignores <code class="highlight fixed" style="white-space:pre-wrap">DELAYED</code> for <code class="highlight fixed" style="white-space:pre-wrap">INSERT ... SELECT</code>
  or <code class="highlight fixed" style="white-space:pre-wrap">INSERT ... ON DUPLICATE KEY UPDATE</code> statements.
- Because the <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> statement returns immediately,
  before the rows are inserted, you cannot use
  <code class="highlight fixed" style="white-space:pre-wrap">LAST_INSERT_ID()</code> to get the
  <code class="highlight fixed" style="white-space:pre-wrap">AUTO_INCREMENT</code> value that the statement might generate.
- <code class="highlight fixed" style="white-space:pre-wrap">DELAYED</code> rows are not visible to <code class="highlight fixed" style="white-space:pre-wrap">SELECT</code>
  statements until they actually have been inserted.
- After <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code>, [ROW_COUNT()](/built-in-functions/secondary-functions/information-functions/row_count/) returns the number of the rows you tried to insert, not the number of the successful writes.
- <code class="highlight fixed" style="white-space:pre-wrap">DELAYED</code> is ignored on slave replication servers, so that 
  <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> is treated as a normal
  <code class="highlight fixed" style="white-space:pre-wrap">INSERT</code> on slaves. This is because
  <code class="highlight fixed" style="white-space:pre-wrap">DELAYED</code> could cause the slave to have different data than
  the master. <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> statements are not [safe for replication](/kb/en/unsafe-statements-for-replication/).
- Pending <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> statements are lost if a table is
  write locked and ALTER TABLE is used to modify the table structure.
- <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> is not supported for views. If you try, you will get an error like this: `ERROR 1347 (HY000): 'view_name' is not BASE TABLE`
- <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> is not supported for [partitioned tables](/kb/en/managing-mariadb-partitioning/).
- <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> is not supported within [stored programs](/kb/en/stored-programs-and-views/).
- <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> does not work with [triggers](/programming-customizing-mariadb/triggers-events/triggers/).
- <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> does not work if there is a check constraint in place.
- <code class="highlight fixed" style="white-space:pre-wrap">INSERT DELAYED</code> does not work if [skip-new](/kb/en/mysqld-options/#-skip-new) mode is active.

## See Also

- [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/)
- [INSERT SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select/)
- [HIGH_PRIORITY and LOW_PRIORITY](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/high_priority-and-low_priority/)
- [Concurrent Inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts/)
- [INSERT - Default &amp; Duplicate Values](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-default-duplicate-values/)
- [INSERT IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-ignore/)
- [INSERT ON DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/)