# Information Schema INNODB_BUFFER_POOL_STATS Table

The [Information Schema](/kb/en/information_schema/) `INNODB_BUFFER_POOL_STATS` table contains information about pages in the [buffer pool](/kb/en/xtradbinnodb-memory-buffer/), similar to what is returned with the [SHOW ENGINE INNODB STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine-innodb-status/) statement.

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>POOL_ID</code></td><td>Buffer Pool identifier. From <a href="/kb/en/mariadb-1051-release-notes/">MariaDB 10.5.1</a> returns a value of 0, since multiple InnoDB buffer pool instances has been removed.</td></tr>
<tr><td><code>POOL_SIZE</code></td><td>Size in pages of the buffer pool.</td></tr>
<tr><td><code>FREE_BUFFERS</code></td><td>Number of free pages in the buffer pool.</td></tr>
<tr><td><code>DATABASE_PAGES</code></td><td>Total number of pages in the buffer pool.</td></tr>
<tr><td><code>OLD_DATABASE_PAGES</code></td><td>Number of pages in the <em>old</em> sublist.</td></tr>
<tr><td><code>MODIFIED_DATABASE_PAGES</code></td><td>Number of dirty pages.</td></tr>
<tr><td><code>PENDING_DECOMPRESS</code></td><td>Number of pages pending decompression.</td></tr>
<tr><td><code>PENDING_READS</code></td><td>Pending buffer pool level reads.</td></tr>
<tr><td><code>PENDING_FLUSH_LRU</code></td><td>Number of pages in the LRU pending flush.</td></tr>
<tr><td><code>PENDING_FLUSH_LIST</code></td><td>Number of pages in the flush list pending flush.</td></tr>
<tr><td><code>PAGES_MADE_YOUNG</code></td><td>Pages moved from the <em>old</em> sublist to the <em>new</em> sublist.</td></tr>
<tr><td><code>PAGES_NOT_MADE_YOUNG</code></td><td>Pages that have remained in the <em>old</em> sublist without moving to the <em>new</em> sublist.</td></tr>
<tr><td><code>PAGES_MADE_YOUNG_RATE</code></td><td>Hits that cause blocks to move to the top of the <em>new</em> sublist.</td></tr>
<tr><td><code>PAGES_MADE_NOT_YOUNG_RATE</code></td><td>Hits that do not cause blocks to move to the top of the <em>new</em> sublist due to the <code><a href="/kb/en/xtradbinnodb-server-system-variables/#innodb_old_blocks_time">innodb_old_blocks</a></code> delay not being met.</td></tr>
<tr><td><code>NUMBER_PAGES_READ</code></td><td>Number of pages read.</td></tr>
<tr><td><code>NUMBER_PAGES_CREATED</code></td><td>Number of pages created.</td></tr>
<tr><td><code>NUMBER_PAGES_WRITTEN</code></td><td>Number of pages written.</td></tr>
<tr><td><code>PAGES_READ_RATE</code></td><td>Number of pages read since the last printout divided by the time elapsed, giving pages read per second.</td></tr>
<tr><td><code>PAGES_CREATE_RATE</code></td><td>Number of pages created since the last printout divided by the time elapsed, giving pages created per second.</td></tr>
<tr><td><code>PAGES_WRITTEN_RATE</code></td><td>Number of pages written since the last printout divided by the time elapsed, giving pages written per second.</td></tr>
<tr><td><code>NUMBER_PAGES_GET</code></td><td>Number of logical read requests.</td></tr>
<tr><td><code>HIT_RATE</code></td><td>Buffer pool hit rate.</td></tr>
<tr><td><code>YOUNG_MAKE_PER_THOUSAND_GETS</code></td><td>For every 1000 gets, the number of pages made young.</td></tr>
<tr><td><code>NOT_YOUNG_MAKE_PER_THOUSAND_GETS</code></td><td>For every 1000 gets, the number of pages not made young.</td></tr>
<tr><td><code>NUMBER_PAGES_READ_AHEAD</code></td><td>Number of pages read ahead.</td></tr>
<tr><td><code>NUMBER_READ_AHEAD_EVICTED</code></td><td>Number of pages read ahead by the read-ahead thread that were later evicted without being accessed by any queries.</td></tr>
<tr><td><code>READ_AHEAD_RATE</code></td><td>Pages read ahead since the last printout divided by the time elapsed, giving read-ahead rate per second.</td></tr>
<tr><td><code>READ_AHEAD_EVICTED_RATE</code></td><td>Read-ahead pages not accessed since the last printout divided by time elapsed, giving the number of read-ahead pages evicted without access per second.</td></tr>
<tr><td><code>LRU_IO_TOTAL</code></td><td>Total least-recently used I/O.</td></tr>
<tr><td><code>LRU_IO_CURRENT</code></td><td>Least-recently used I/O for the current interval.</td></tr>
<tr><td><code>UNCOMPRESS_TOTAL</code></td><td>Total number of pages decompressed.</td></tr>
<tr><td><code>UNCOMPRESS_CURRENT</code></td><td>Number of pages decompressed in the current interval</td></tr>
</tbody></table>

## Examples

```sql
DESC information_schema.innodb_buffer_pool_stats;
+----------------------------------+---------------------+------+-----+---------+-------+
| Field                            | Type                | Null | Key | Default | Extra |
+----------------------------------+---------------------+------+-----+---------+-------+
| POOL_ID                          | bigint(21) unsigned | NO   |     | 0       |       |
| POOL_SIZE                        | bigint(21) unsigned | NO   |     | 0       |       |
| FREE_BUFFERS                     | bigint(21) unsigned | NO   |     | 0       |       |
| DATABASE_PAGES                   | bigint(21) unsigned | NO   |     | 0       |       |
| OLD_DATABASE_PAGES               | bigint(21) unsigned | NO   |     | 0       |       |
| MODIFIED_DATABASE_PAGES          | bigint(21) unsigned | NO   |     | 0       |       |
| PENDING_DECOMPRESS               | bigint(21) unsigned | NO   |     | 0       |       |
| PENDING_READS                    | bigint(21) unsigned | NO   |     | 0       |       |
| PENDING_FLUSH_LRU                | bigint(21) unsigned | NO   |     | 0       |       |
| PENDING_FLUSH_LIST               | bigint(21) unsigned | NO   |     | 0       |       |
| PAGES_MADE_YOUNG                 | bigint(21) unsigned | NO   |     | 0       |       |
| PAGES_NOT_MADE_YOUNG             | bigint(21) unsigned | NO   |     | 0       |       |
| PAGES_MADE_YOUNG_RATE            | double              | NO   |     | 0       |       |
| PAGES_MADE_NOT_YOUNG_RATE        | double              | NO   |     | 0       |       |
| NUMBER_PAGES_READ                | bigint(21) unsigned | NO   |     | 0       |       |
| NUMBER_PAGES_CREATED             | bigint(21) unsigned | NO   |     | 0       |       |
| NUMBER_PAGES_WRITTEN             | bigint(21) unsigned | NO   |     | 0       |       |
| PAGES_READ_RATE                  | double              | NO   |     | 0       |       |
| PAGES_CREATE_RATE                | double              | NO   |     | 0       |       |
| PAGES_WRITTEN_RATE               | double              | NO   |     | 0       |       |
| NUMBER_PAGES_GET                 | bigint(21) unsigned | NO   |     | 0       |       |
| HIT_RATE                         | bigint(21) unsigned | NO   |     | 0       |       |
| YOUNG_MAKE_PER_THOUSAND_GETS     | bigint(21) unsigned | NO   |     | 0       |       |
| NOT_YOUNG_MAKE_PER_THOUSAND_GETS | bigint(21) unsigned | NO   |     | 0       |       |
| NUMBER_PAGES_READ_AHEAD          | bigint(21) unsigned | NO   |     | 0       |       |
| NUMBER_READ_AHEAD_EVICTED        | bigint(21) unsigned | NO   |     | 0       |       |
| READ_AHEAD_RATE                  | double              | NO   |     | 0       |       |
| READ_AHEAD_EVICTED_RATE          | double              | NO   |     | 0       |       |
| LRU_IO_TOTAL                     | bigint(21) unsigned | NO   |     | 0       |       |
| LRU_IO_CURRENT                   | bigint(21) unsigned | NO   |     | 0       |       |
| UNCOMPRESS_TOTAL                 | bigint(21) unsigned | NO   |     | 0       |       |
| UNCOMPRESS_CURRENT               | bigint(21) unsigned | NO   |     | 0       |       |
+----------------------------------+---------------------+------+-----+---------+-------+
```