# Using the S3 Storage Engine

##### MariaDB starting with [10.5](/kb/en/what-is-mariadb-105/)

The [S3 storage engine](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/) has been available since [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/).

The [S3 storage engine](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/) is read only and allows one to archive MariaDB
tables in Amazon S3, or any third-party public or private cloud that
implements S3 API (of which there are many), but still have them
accessible for reading in MariaDB.

## Installing the Plugin

As of [MariaDB 10.5.7](/kb/en/mariadb-1057-release-notes/), the S3 storage engine is currently [gamma maturity](/kb/en/release-criteria/), so the following step can be omitted.

On earlier releases, when it was [alpha maturity](/kb/en/release-criteria/), it will not load by default on a stable release of the server due to the default value of the [plugin_maturity](/kb/en/server-system-variables/#plugin_maturity) variable. Set to `alpha` (or below) in your config file to permit installation of the plugin:

```sql
[mysqld]
plugin-maturity = alpha
```

and restart the server.

Now [install the plugin library](/kb/en/plugin-overview/#installing-a-plugin), for example:

```sql
INSTALL SONAME 'ha_s3';
```

If the library is not available, for example:

```sql
INSTALL SONAME 'ha_s3';
ERROR 1126 (HY000): Can't open shared library '/var/lib/mysql/lib64/mysql/plugin/ha_s3.so' 
  (errno: 13, cannot open shared object file: No such file or directory)
```

you may need to install a separate package for the S3 storage engine, for example:

```sql
shell> yum install MariaDB-s3-engine
```

## Moving Data to S3

To move data from an existing table to S3, one can run:

```sql
ALTER TABLE old_table ENGINE=S3
```

To get data back to a 'normal' table one can do:

```sql
ALTER TABLE s3_table ENGINE=INNODB
```

## New Options for [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/)

- <strong>`S3_BLOCK_SIZE` :</strong> Set to 4M as default. This is the block size for all index and data pages stored in S3.
- <strong>`COMPRESSION_ALGORITHM` :</strong> Set to 'none' as default. Which compression algorithm to use for block stored in S3.  Options are: `none` or `zlib`.

[ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) can be used on S3 tables as normal to add columns or change column definitions.

## mysqld Startup Options for S3

To be able to use S3 for storage one <strong>*must</strong>* define how to access S3 and where data are stored in S3:

- <strong>[s3_access_key](/kb/en/s3-storage-engine-system-variables/#s3_access_key):</strong> The AWS access key to access your data
- <strong>[s3_secret_key](/kb/en/s3-storage-engine-system-variables/#s3_secret_key):</strong> The AWS secret key to access your data
- <strong>[s3_bucket](/kb/en/s3-storage-engine-system-variables/#s3_bucket): </strong> The AWS bucket where your data should be stored. All MariaDB table data is stored in this bucket.
- <strong>[s3_region](/kb/en/s3-storage-engine-system-variables/#s3_region): </strong> The AWS region where your data should be stored.

If you are going to use a master-slave setup, you should look at the following variables:

- <strong>[s3_replicate_alter_as_create_select](/kb/en/s3-storage-engine-system-variables/#s3-replicate-alter-as-create-select): </strong> When converting an S3 table to local table, log all rows in binary log. Defaults to <code class="fixed" style="white-space:pre-wrap">TRUE</code>. This allows the slave to replicate <code class="fixed" style="white-space:pre-wrap">CREATE TABLE .. SELECT FROM s3_table</code> even it the slave doesn't have access to the original <code class="fixed" style="white-space:pre-wrap">s3_table</code>.
- <strong>[s3_slave_ignore_updates](/kb/en/s3-storage-engine-system-variables/#s3-slave-ignore-updates): </strong> Should be set if master and slave share the same S3 instance. This tells the slave that it can ignore any updates to the S3 tables as they are already applied on the master. Defaults to <code class="fixed" style="white-space:pre-wrap">FALSE</code>.

The above defaults assume that the master and slave don't share the same S3 instance.

Other, less critical options, are:

- <strong>[s3_host_name](/kb/en/s3-storage-engine-system-variables/#s3_host_name):</strong> Hostname for the S3 service. "s3.amazonaws.com", Amazon S3 service, by default.
- <strong>[s3_protocol_version](/kb/en/s3-storage-engine-system-variables/#s3_protocol_version):</strong>  Protocol used to communication with S3. One of "Auto", "Amazon" or "Original" where "Auto" is the default. If you get errors like "8 Access Denied" when you are connecting to another service provider, then try to change this option.  The reason for this variable is that Amazon has changed some parts of the S3 protocol since they originally introduced it but other service providers are still using the original protocol.
- <strong>[s3_block_size](/kb/en/s3-storage-engine-system-variables/#s3_block_size):</strong> Set to 4M as default.  This is the default block size for a table, if not specified in [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/).
- <strong>[s3_pagecache_buffer_size](/kb/en/s3-storage-engine-system-variables/#s3_pagecache_buffer_size):</strong> Default 128M. The size of the buffer used for index blocks for S3 tables. Increase this to get better index handling (for all reads and multiple writes) to as much as you can afford.

Last some options you probably don't have to ever touch:

- <strong>[s3_pagecache_age_threshold](/kb/en/s3-storage-engine-system-variables/#s3_pagecache_age_threshold) : </strong> Default 300: This characterizes the number of hits a hot block has to be untouched until it is considered aged enough to be downgraded to a warm block. This specifies the percentage ratio of that number of hits to the total number of blocks in the page cache.
- <strong>[s3_pagecache_division_limit](/kb/en/s3-storage-engine-system-variables/#s3_pagecache_division_limit): </strong> Default 100. The minimum percentage of warm blocks in key cache.
- <strong>[s3_pagecache_file_hash_size](/kb/en/s3-storage-engine-system-variables/#s3_pagecache_file_hash_size): </strong> Default 512. Number of hash buckets for open files.  If you have a lot of S3 files open you should increase this for faster flush of changes. A good value is probably 1/10 of number of possible open S3 files.
- <strong>[s3_debug](/kb/en/s3-storage-engine-system-variables/#s3_debug): </strong> Default 0. Generates a trace file from libmarias3 on stderr (mysqld.err) for debugging  the S3 protocol.

## Typical my.cnf Entry

```sql
[mariadb-10.5]
s3=ON
s3-bucket=mariadb
s3-access-key=xxxx
s3-secret-key=xxx
s3-region=eu-north-1
s3-host-name=s3.amazonaws.com
# Master and slave shares same S3 tables.
s3-slave-ignore-updates=1

[aria_s3_copy]
s3-bucket=mariadb
s3-access-key=xxxx
s3-secret-key=xxx
s3-region=eu-north-1
s3-host-name=s3.amazonaws.com
```

## Typical Usage Case for S3 Tables

The typical use case would be that there exists tables that after some
time would become fairly inactive, but are still important so that they
can not be removed. In that case, an option is to move such a table
to an archiving service, which is accessible through an S3 API.

Notice that S3 means the Cloud Object Storage API defined by Amazon
AWS. Often the whole of Amazon’s Cloud Object Storage is referred to
as S3. In the context of the S3 archive storage engine, it refers to
the API itself that defines how to store objects in a cloud service,
being it Amazon’s or someone else’s. OpenStack for example provides an
S3 API for storing objects.

The main benefit of storing things in an S3 compatible storage is that the
cost of storage is much cheaper than many other alternatives. Many S3
implementations also provide reliable long-term storage.

## Operations Allowed on S3 Tables

- [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) S3 supports all types, keys and other options that are supported by the [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine/) engine. One can also perform [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) on an S3 table to add or modify columns etc.
- [DROP TABLE](/sql-statements-structure/sql-statements/data-definition/drop/drop-table/)
- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)  Any SELECT operations you can perform on a normal table should work with an S3 table.
- [SHOW TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-tables/) will show all tables that exist in the current defined S3 location.
- S3 tables can be part of [partitions](/mariadb-administration/partitioning-tables/partitions-files/). See Discovery below.

## Discovery

The S3 storage engine supports full [MariaDB discovery](/columns-storage-engines-and-plugins/storage-engines/storage-engines-storage-engine-development/table-discovery/). This means that if
you have the S3 storage engine enabled and properly configured, the
table stored in S3 will automatically be discovered when it's accessed with
[SHOW TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-tables/), [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) or any other operation that
tries to access it. In the case of SELECT, the .frm file from S3 will
be copied to the local storage to speed up future accesses.

When an S3 table is opened for the first time (it's not in the table cache)
and there is a local .frm file, the S3 engine will check if it's still
relevant, and if not, update or delete the .frm file.

This means that if the table definition changes on S3 and it's in the
local cache, one has to execute [FLUSH TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) to
get MariaDB to notice the change and update the .frm file.

If partitioning S3 tables are used, the partition definitions will also be stored on S3 storage and will be discovered by other servers.

Discovery of S3 tables is not done for tables in the [mysql databases](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/) to make mysqld boot faster and more securely.

## Replication

S3 works with [replication](/replication/standard-replication/replication-overview/). One can use replication in two different scenarios:

- The master and slave share the same S3 storage. In this case the master will make all changes to the S3 data and the slave will ignore any changes in the replication stream to S3 data . This scenario is achieved by setting [s3_slave_ignore_updates](/kb/en/s3-storage-engine-system-variables/#s3-slave-ignore-updates) to 1.
- The master and slave don't share the same S3 storage or the slave uses another storage engine for the S3 tables. This scenario is achieved by setting [s3_slave_ignore_updates](/kb/en/s3-storage-engine-system-variables/#s3-slave-ignore-updates) to 0.

## aria_s3_copy

[aria_s3_copy](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/aria_s3_copy/) is an external tool that one can use to copy [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine/) tables to and from S3.  Use `aria_s3_copy --help` to get the options of how to use it.

## mysqldump

- [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) will by default ignore S3 tables. If `mysqldump` is run with the `--copy-s3-tables` option, the resulting file will contain a CREATE statement for a similar [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/aria-storage-engine/) table, followed by the table data and ending with an `ALTER TABLE xxx ENGINE=S3`.

## Current Limitations

- [mysql-test-run](/kb/en/mysql-test-run/) doesn't by default test the S3 engine as we can't embed AWS keys into mysql-test-run.
- Slaves should not access S3 tables while they are ALTERed! This is because there is no locking implemented to S3 between servers. However, after a table (either the original S3 table or the partitioned S3 table) is changed on the master, the slave will notice this on the next access and update its local definition.

### Limitations in [ALTER .. PARTITION](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/)

All [ALTER PARTITION](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) operations are supported on S3 partitioning tables except:

- REBUILD PARTITION
- TRUNCATE PARTITION
- REORGANIZE PARTITION

## Performance Considerations

Depending on your connection speed to your S3 provider, there can be some notable slowdowns in some
operations.

### Discovery

As S3 is supporting discovery (automatically making tables available that are in S3) this can cause some
small performance problems if the S3 engine is enabled. Partitioning S3 tables also support discovery.

- CREATE TABLE is a bit slower as the S3 engine has to check if the to-be-created table is already S3.
- Queries on information_schema tables are slower as S3 has to check if there is new tables in S3.
- DROP of non existing tables are slower as S3 has to check if the table is in S3.

There are no performance degradations when accessing existing tables on the server. Accessing the S3
table the first time will copy the .frm file from S3 to the local disk, speeding up future accesses to the table.

### Caching

- Accessing a table on S3 can take some time , especially if you are using big packets ([s3_block_size](/kb/en/s3-storage-engine-system-variables/#s3_block_size)).  However the second access to the same data should be fast as it's then cached in the S3 page cache.

### Things to Try to Increase Performance

If you have performance problems with the S3 engine, here are some things you can try:

- Decreasing [s3_block_size](/kb/en/s3-storage-engine-system-variables/#s3_block_size). This can be done both globally and per table.
- Use COMPRESSION_ALGORITHM=zlib when creating the table. This will decrease the amount of data transferred from S3 to the local cache.
- Increasing the size of the s3 page cache: [s3_pagecache_buffer_size](/kb/en/s3-storage-engine-system-variables/#s3_pagecache_buffer_size)

Try also to execute the query twice to check if the problem is that the data was not properly cached.  When data is cached locally the performance should be excellent.

## Future Development Ideas

- Store aws keys and region in the mysql.servers table (as [Spider](/columns-storage-engines-and-plugins/storage-engines/spider/) and [FederatedX](/kb/en/federatedx/)). This will allow one to have different tables on different S3 servers.
- Store s3 bucket, access_key and secret key in a cache to better be able to better to reuse connections. This would save some memory and make some S3 accesses a bit faster as we could reuse old connections.

## Troubleshooting S3 on SELinux

If you get errors such as:

```sql
ERROR 3 (HY000): Got error from put_object(bubu/produkt/frm): 5 Couldn't connect to server
```

one reason could be that your system doesn't allow MariaDB to connect to ports other than 3306.
The fix is to change `/etc/selinux/config` to `SELINUX=disabled`.

## See Also

- [S3 storage engine internals](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/s3-storage-engine-internals/)