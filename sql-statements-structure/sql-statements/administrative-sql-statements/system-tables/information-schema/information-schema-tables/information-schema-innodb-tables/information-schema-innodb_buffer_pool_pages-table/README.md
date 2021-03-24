# Information Schema INNODB_BUFFER_POOL_PAGES Table

The [Information Schema](/kb/en/information_schema/) `INNODB_BUFFER_POOL_PAGES` table is a Percona enhancement, and is only available for XtraDB, not InnoDB (see [XtraDB and InnoDB](/kb/en/xtradb-and-innodb/)). It contains a record for each page in the [buffer pool](/kb/en/xtradbinnodb-memory-buffer/).

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>PAGE_TYPE</code></td><td>Type of page; one of <code>index</code>, <code>undo_log</code>, <code>inode</code>, <code>ibuf_free_list</code>, <code>allocated, bitmap</code>, <code>sys</code>, <code>trx_sys</code>, <code>fsp_hdr</code>, <code>xdes</code>, <code>blob</code>, <code>zblob</code>, <code>zblob2</code> and <code>unknown</code>.</td></tr>
<tr><td><code>SPACE_ID</code></td><td>Tablespace ID.</td></tr>
<tr><td><code>PAGE_NO</code></td><td>Page offset within tablespace.</td></tr>
<tr><td><code>LRU_POSITION</code></td><td>Page position in the LRU (least-recently-used) list.</td></tr>
<tr><td><code>FIX_COUNT</code></td><td>Page reference count, incremented each time the page is accessed. <code>0</code> if the page is not currently being accessed.</td></tr>
<tr><td><code>FLUSH_TYPE</code></td><td>Flush type of the most recent flush.<code>0</code> (LRU), <code>2</code> (flush_list)</td></tr>
</tbody></table>