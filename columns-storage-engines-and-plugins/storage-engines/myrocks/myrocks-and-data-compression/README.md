# MyRocks and Data Compression

MyRocks supports several compression algorithms.

## Supported Compression Algorithms

Supported compression algorithms can be checked like so:

```sql
show variables like 'rocksdb%compress%';
+-------------------------------------+------------------------------------+
| Variable_name                       | Value                              |
+-------------------------------------+------------------------------------+
| rocksdb_supported_compression_types | Snappy,Zlib,LZ4,LZ4HC,ZSTDNotFinal |
+-------------------------------------+------------------------------------+
```

Another way to make the check is to look into `#rocksdb/LOG` file in the data directory. It should have lines like:

```sql
2019/04/12-14:08:23.869919 7f839188b540 Compression algorithms supported:
2019/04/12-14:08:23.869920 7f839188b540         kZSTDNotFinalCompression supported: 1
2019/04/12-14:08:23.869922 7f839188b540         kZSTD supported: 1
2019/04/12-14:08:23.869923 7f839188b540         kXpressCompression supported: 0
2019/04/12-14:08:23.869924 7f839188b540         kLZ4HCCompression supported: 1
2019/04/12-14:08:23.869924 7f839188b540         kLZ4Compression supported: 1
2019/04/12-14:08:23.869925 7f839188b540         kBZip2Compression supported: 0
2019/04/12-14:08:23.869926 7f839188b540         kZlibCompression supported: 1
2019/04/12-14:08:23.869927 7f839188b540         kSnappyCompression supported: 1
```

## Compression Settings

Compression is set on a per-Column Family basis.  See [MyRocks Column Families](/columns-storage-engines-and-plugins/storage-engines/myrocks/myrocks-column-families).

### Checking Compression Settings

To check current compression settings for a column family one can use a query like so:

```sql
select * from information_schema.rocksdb_cf_options 
where option_type like '%ompression%' and cf_name='default';
```

The output will be like:

```sql
+---------+-----------------------------------------+---------------------------+
| CF_NAME | OPTION_TYPE                             | VALUE                     |
+---------+-----------------------------------------+---------------------------+
| default | COMPRESSION_TYPE                        | kSnappyCompression        |
| default | COMPRESSION_PER_LEVEL                   | NUL                       |
| default | COMPRESSION_OPTS                        | -14:32767:0               |
| default | BOTTOMMOST_COMPRESSION                  | kDisableCompressionOption |
| default | TABLE_FACTORY::VERIFY_COMPRESSION       | 0                         |
| default | TABLE_FACTORY::ENABLE_INDEX_COMPRESSION | 1                         |
+---------+-----------------------------------------+---------------------------+
```

Current column family settings will be used for the new SST files.

### Modifying Compression Settings

Compression settings are not dynamic parameters, one cannot change them by setting `@@rocksdb_update_cf_options`.

The procedure to change compression settings is as follows:

- Edit `my.cnf` to set `rocksdb_override_cf_options`.

Example:

```sql
rocksdb-override-cf-options='cf1={compression=kZSTD;bottommost_compression=kZSTD;}'
```

- Restart the server.

The data will not be re-compressed immediately. However, all new SST files will use the new compression settings, so as data gets inserted/updated the column family will gradually start using the new option.

### Caveat: syntax errors

Please note that `rocksdb-override-cf-options` syntax is quite strict.  Any typos will result in the parse error, and MyRocks plugin will not be loaded.  Depending on your configuration, the server may still start. If it does start, you can use this command to check if the plugin is loaded:

```sql
select * from information_schema.plugins where plugin_name='ROCKSDB'
```

(note that you need the "ROCKSDB" plugin. Other auxiliary plugins like "ROCKSDB_TRX" might still get loaded).

Another way is to detect the error is check the error log. When option parsing fails, it will contain messages like so:

```sql
2019-04-16 11:07:57 140283675678016 [Warning] Invalid cf config for cf1 in override options (options: cf1={compression=kLZ4Compression;bottommost_compression=kZSTDCompression;})
2019-04-16 11:07:57 140283675678016 [ERROR] RocksDB: Failed to initialize CF options map.
2019-04-16 11:07:57 140283675678016 [ERROR] Plugin 'ROCKSDB' init function returned error.
2019-04-16 11:07:57 140283675678016 [ERROR] Plugin 'ROCKSDB' registration as a STORAGE ENGINE failed.
```

## Checking How the Data is Compressed

A query to check what compression is used in the SST files that store the data for a given table (test.t1):

```sql
select
  SP.sst_name, SP.compression_algo
from
  information_schema.rocksdb_sst_props SP,
  information_schema.rocksdb_ddl D,
  information_schema.rocksdb_index_file_map IFM
where
  D.table_schema='test' and D.table_name='t1' and
  D.index_number= IFM.index_number and
  IFM.sst_name=SP.sst_name;
```

Example output:

```sql
+------------+------------------+
| sst_name   | compression_algo |
+------------+------------------+
| 000028.sst | Snappy           |
| 000028.sst | Snappy           |
| 000026.sst | Snappy           |
| 000026.sst | Snappy           |
+------------+------------------+
```