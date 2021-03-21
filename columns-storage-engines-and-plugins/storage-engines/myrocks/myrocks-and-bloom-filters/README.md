# MyRocks and Bloom Filters

Bloom filters are used to reduce read amplification. Bloom filters can be set on a per-column family basis (see [myrocks-column-families](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-column-families/)).

## Bloom Filter Parameters

- How many bits to use
- whole_key_filtering=true/false
- Whether the bloom filter is for the entire key or for the prefix. In case of a prefix, you need to look at the index definition and compute the desired prefix length.

### Computing Prefix Length

- It's 4 bytes for `index_nr`
- Then, for fixed-size columns (integer, date[time], decimal) it is key_length as shown by `EXPLAIN`.  For VARCHAR columns, determining the length is tricky   (It depends on the values stored in the table. Note that MyRocks encodes VARCHARs with  "Variable-Length Space-Padded Encoding" format).

## Configuring Bloom Filter

To enable 10-bit bloom filter for 8-byte prefix length for column family "cf1",  put this into my.cnf:

```sql
rocksdb_override_cf_options='cf1={block_based_table_factory={filter_policy=bloomfilter:10:false;whole_key_filtering=0;};prefix_extractor=capped:8};'
```

and restart the server.

Check if the column family actually uses the bloom filter:

```sql
select * 
from information_schema.rocksdb_cf_options 
where 
  cf_name='cf1' and
  option_type IN ('TABLE_FACTORY::FILTER_POLICY','PREFIX_EXTRACTOR');
```

```sql
+---------+------------------------------+----------------------------+
| CF_NAME | OPTION_TYPE                  | VALUE                      |
+---------+------------------------------+----------------------------+
| cf1     | PREFIX_EXTRACTOR             | rocksdb.CappedPrefix.8     |
| cf1     | TABLE_FACTORY::FILTER_POLICY | rocksdb.BuiltinBloomFilter |
+---------+------------------------------+----------------------------+
```

## Checking if Bloom Filter is Useful

Watch these status variables:

```sql
show status like '%bloom%';
+-------------------------------------+-------+
| Variable_name                       | Value |
+-------------------------------------+-------+
| Rocksdb_bloom_filter_prefix_checked | 1     |
| Rocksdb_bloom_filter_prefix_useful  | 0     |
| Rocksdb_bloom_filter_useful         | 0     |
+-------------------------------------+-------+
```

Other useful variables are:

- `rocksdb_force_flush_memtable_now` - bloom filter is only used when reading data from disk. If you are doing testing, flush the data to disk first.
- `rocksdb_skip_bloom_filter_on_read` - skip using the bloom filter (default is FALSE).