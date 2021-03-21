# Loading Data Into MyRocks

Being a write-optimized storage engine, MyRocks has special ways to load data much faster than normal INSERTs would.

See

- [http://myrocks.io/docs/getting-started/](http://myrocks.io/docs/getting-started/); the section about "Migrating from InnoDB to MyRocks in production" has some clues.

- [https://github.com/facebook/mysql-5.6/wiki/Data-Loading](https://github.com/facebook/mysql-5.6/wiki/Data-Loading) covers the topic in greater detail.

Note 
When one loads data with [rocksdb_bulk_load=1](/kb/en/myrocks-system-variables/#rocksdb_bulk_load) and the data conflicts with the data already in the database, one may get non-trivial errors, for example:

```sql
ERROR 1105 (HY000): [./.rocksdb/test.t1_PRIMARY_2_0.bulk_load.tmp] bulk load error: 
  Invalid argument: External file requires flush
```