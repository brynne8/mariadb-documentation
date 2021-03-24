# Information Schema INNODB_BUFFER_POOL_PAGES_BLOB Table

The [Information Schema](/kb/en/information_schema/) `INNODB_BUFFER_POOL_PAGES_BLOB` table is a Percona enchancement, and is only available for XtraDB, not InnoDB (see [XtraDB and InnoDB](/kb/en/xtradb-and-innodb/)). It contains information about [buffer pool](/kb/en/xtradbinnodb-memory-buffer/) blob pages.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>SPACE_ID</code></td><td>Tablespace ID.</td></tr>
<tr><td><code>PAGE_NO</code></td><td>Page offset within tablespace.</td></tr>
<tr><td><code>COMPRESSED</code></td><td><code>1</code> if the blob contains compressed data, <code>0</code> if not.</td></tr>
<tr><td><code>PART_LEN</code></td><td>Page data length.</td></tr>
<tr><td><code>NEXT_PAGE_NO</code></td><td>Next page number.</td></tr>
<tr><td><code>LRU_POSITION</code></td><td>Page position in the LRU (least-recently-used) list.</td></tr>
<tr><td><code>FIX_COUNT</code></td><td>Page reference count, incremented each time the page is accessed. <code>0</code> if the page is not currently being accessed.</td></tr>
<tr><td><code>FLUSH_TYPE</code></td><td>Flush type of the most recent flush.<code>0</code> (LRU), <code>2</code> (flush_list)</td></tr>
</tbody></table>