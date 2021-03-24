# Information Schema INNODB_CMP and INNODB_CMP_RESET Tables

The `INNODB_CMP` and `INNODB_CMP_RESET` tables contain status information on compression operations related to [compressed XtraDB/InnoDB tables](/kb/en/innodb-storage-formats/#compressed).

The <a undefined>PROCESS</a> privilege is required to query this table.

These tables contain the following columns:

<table><tbody><tr><th>Column Name</th><th>Description</th></tr>
<tr><td><code>PAGE_SIZE</code></td><td>Compressed page size, in bytes. This value is unique in the table; other values are totals which refer to pages of this size.</td></tr>
<tr><td><code>COMPRESS_OPS</code></td><td>How many times a page of the size <code>PAGE_SIZE</code> has been compressed. This happens when a new page is created because the compression log runs out of space. This value includes both successful operations and <em>compression failures</em>.</td></tr>
<tr><td><code>COMPRESS_OPS_OK</code></td><td>How many times a page of the size <code>PAGE_SIZE</code> has been successfully compressed. This value should be as close as possible to <code>COMPRESS_OPS</code>. If it is notably lower, either avoid compressing some tables, or increase the <code>KEY_BLOCK_SIZE</code> for some compressed tables.</td></tr>
<tr><td><code>COMPRESS_TIME</code></td><td>Time (in seconds) spent to compress pages of the size <code>PAGE_SIZE</code>. This value includes time spent in <em>compression failures</em>.</td></tr>
<tr><td><code>UNCOMPRESS_OPS</code></td><td>How many times a page of the size <code>PAGE_SIZE</code> has been uncompressed. This happens when an uncompressed version of a page is created in the buffer pool, or when a <em>compression failure</em> occurs.</td></tr>
<tr><td><code>UNCOMPRESS_TIME</code></td><td>Time (in seconds) spent to uncompress pages of the size <code>PAGE_SIZE</code>.</td></tr>
</tbody></table>

These tables can be used to measure the effectiveness of XtraDB/InnoDB table compression. When you have to decide a value for `KEY_BLOCK_SIZE`, you can create more than one version of the table (one for each candidate value) and run a realistic workload on them. Then, these tables can be used to see how the operations performed with different page sizes.

`INNODB_CMP` and `INNODB_CMP_RESET` have the same columns and always contain the same values, but when `INNODB_CMP_RESET` is queried, both the tables are cleared. `INNODB_CMP_RESET` can be used, for example, if a script periodically logs the performances of compression in the last period of time. `INNODB_CMP` can be used to see the cumulated statistics.

## Examples

```sql
SELECT * FROM information_schema.INNODB_CMP\G
**************************** 1. row *****************************
      page_size: 1024
   compress_ops: 0
compress_ops_ok: 0
  compress_time: 0
 uncompress_ops: 0
uncompress_time: 0
...
```

## See Also

Other tables that can be used to monitor XtraDB/InnoDB compressed tables:

- [INNODB_CMP_PER_INDEX and INNODB_CMP_PER_INDEX_RESET](/kb/en/information_schemainnodb_cmp_per_index-and-innodb_cmp_per_index_reset-table/)
- [INNODB_CMPMEM and INNODB_CMPMEM_RESET](/kb/en/information_schemainnodb_cmpmem-and-innodb_cmpmem_reset-tables/)