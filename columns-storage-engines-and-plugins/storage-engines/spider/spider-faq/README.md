# Spider FAQ

### What does "[ERROR] mysqld: Can't find record in 'spider_tables'" mean?

This happens when you have a Spider table defined that does not point to an existing table on a data node.

### Are there minimum Spider settings?

```sql
myisam-recover=FORCE,BACKUP
```

##### MariaDB until [10.1.1](/kb/en/mariadb-1011-release-notes/)

```sql
optimizer_switch='engine_condition_pushdown=on'
```

##### MariaDB until [10.3.7](/kb/en/mariadb-1037-release-notes/)

When using spider_autoincrement_mode = 0, partitioned Spider tables work as spider_autoincrement_mode = 1
see :  [MDEV-21404](https://jira.mariadb.org/browse/MDEV-21404)

### What does "select spider_ping_table()" in the general log mean?

This is used by Spider monitoring to ask other monitoring nodes the status of a table.

### Do I need a primary key on physical tables?

Not having a primary key will generate errors for resynchronizing tables via spider_copy_table().

### Can I use Spider on top of Galera shards?

Yes, XA transactions can be disabled from Spider. Until Galera 4.0 fully supports xa transactions, spider can point to a maxscale proxy that can manage transparent node election in case of failure inside a shard group. Note that disabling XA will break cross shard WRITES in case of transaction ROLLBACK.
This architecture need to be used with care if you have a highly transactional workload that can generate cross shard deadlocks.

### What are the most used architectures for Spider HA?

- Delegation of shard node replication using asynchronous replication and slave election with GTID.
- Delegation of shard node replication via active passive HA solutions.
- Shard builds via replication into Spider tables is interesting when you can route READS to a pool of Spider nodes reattaching the shards.

### What are the most used architectures for Spider Map Reduce?

- Map reduce in Spider is limited to a single table. Building spider on top of some views can eliminate the need to use joins.
- Replication to universal tables to every shard is commonly used to enable the views on each shard.

### What about Grants on shards?

- When using MRR and BKA (and you do so with network storage), when Spider needs to create temporary tables on the backends, use the CREATE TEMPORARY TABLES privilege. Spider can still switch to a lower performance solution using [spider_bka_mode=2](/kb/en/spider-server-system-variables/#spider_bka_mode), or Query push down or range predicate using [spider_bka_mode=0](/kb/en/spider-server-system-variables/#spider_bka_mode)