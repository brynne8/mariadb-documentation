# S3 Storage Engine System Variables

##### MariaDB starting with [10.5](/kb/en/what-is-mariadb-105/)

The [S3 storage engine](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/) has been available since [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/).

This page documents system variables related to the [S3 storage engine](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/).

See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a complete list of system variables and instructions on setting system variables.

Also see the [Full list of MariaDB options, system and status variables](/mariadb-administration/variables-and-modes/full-list-of-mariadb-options-system-and-status-variables/)

## Variables

#### `s3_access_key`

- <strong>Description:</strong> The AWS access key to access your data. See [mysqld startup options for S3](/kb/en/using-the-s3-storage-engine/#mysqld-startup-options-for-s3).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-access-key=val</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> (Empty)
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `s3_block_size`

- <strong>Description:</strong> The default block size for a table, if not specified in [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/). Set to 4M as default. See [mysqld startup options for S3](/kb/en/using-the-s3-storage-engine/#mysqld-startup-options-for-s3).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-block-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Numeric
- <strong>Default Value:</strong> `4194304`
- <strong>Range:</strong> `4194304` to `16777216`
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `s3_bucket`

- <strong>Description:</strong> The AWS bucket where your data should be stored. All MariaDB table data is stored in this bucket. See [mysqld startup options for S3](/kb/en/using-the-s3-storage-engine/#mysqld-startup-options-for-s3).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-bucket=val</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> `MariaDB`
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `s3_debug`

- <strong>Description:</strong> Generates a trace file from libmarias3 on stderr (mysqld.err) for debugging  the S3 protocol.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-debug{=0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> Boolean
- <strong>Valid Values:</strong> 0 or 1
- <strong>Default Value:</strong> `0`
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `s3_host_name`

- <strong>Description:</strong> Hostname for the S3 service. "s3.amazonaws.com", Amazon S3 service, by default
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-host-name=val</code>
- <strong>Scope:</strong> Globa;
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> `s3.amazonaws.com`
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `s3_pagecache_age_threshold`

- <strong>Description:</strong> This characterizes the number of hits a hot block has to be untouched until it is considered aged enough to be downgraded to a warm block. This specifies the percentage ratio of that number of hits to the total number of blocks in the page cache.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-pagecache-age-threshold=val</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Numeric
- <strong>Default Value:</strong> `300`
- <strong>Range:</strong> `100` to `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `s3_pagecache_buffer_size`

- <strong>Description:</strong> The size of the buffer used for index blocks for S3 tables. Increase this to get better index handling (for all reads and multiple writes) to as much as you can afford. Size can be adjusted in blocks of `8192`.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-pagecache-buffer-size=val</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> Numeric
- <strong>Default Value:</strong> `134217728` (128M)
- <strong>Range:</strong> `33554432` to `18446744073709551615`
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `s3_pagecache_division_limit`

- <strong>Description:</strong> The minimum percentage of warm blocks in key cache.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-pagecache-division-limit=val</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Numeric
- <strong>Default Value:</strong> `100`
- <strong>Range:</strong> `1` to `100`
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `s3_pagecache_file_hash_size`

- <strong>Description:</strong> Number of hash buckets for open files. Default 512. If you have a lot of S3 files open you should increase this for faster flush of changes. A good value is probably 1/10 of number of possible open S3 files.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-pagecache-file-hash-size=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> Numeric
- <strong>Default Value:</strong> `512`
- <strong>Range:</strong> `32` to `16384`
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `s3_port`

- <strong>Description:</strong> The TCP port number on the S3 host to connect to. A values of 0 means determine automatically.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-port=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> Numeric
- <strong>Default Value:</strong> `0`
- <strong>Range:</strong> `0` to `65535`
- <strong>Introduced:</strong> [MariaDB 10.5.7](/kb/en/mariadb-1057-release-notes/)

---

#### `s3_protocol_version`

- <strong>Description:</strong>  Protocol used to communication with S3. One of "Auto", "Amazon" or "Original" where "Auto" is the default. If you get errors like "8 Access Denied" when you are connecting to another service provider, then try to change this option. The reason for this variable is that Amazon has changed some parts of the S3 protocol since they originally introduced it but other service providers are still using the original protocol.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-protocol-version=val</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> Enum
- <strong>Valid Values:</strong> `Auto`, `Amazon` or `Original`
- <strong>Default Value:</strong> `Auto`
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `s3_region`

- <strong>Description:</strong> The AWS region where your data should be stored. See [mysqld startup options for S3](/kb/en/using-the-s3-storage-engine/#mysqld-startup-options-for-s3).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-region=val</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> (Empty)
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `s3_replicate_alter_as_create_select`

- <strong>Description:</strong>  When converting S3 table to local table, log all rows in binary log. This allows the slave to replicate <code class="fixed" style="white-space:pre-wrap">CREATE TABLE .. SELECT FROM s3_table</code> even it the slave doesn't have access to the original <code class="fixed" style="white-space:pre-wrap">s3_table</code>.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-replicate-alter-as-create-select{=0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `1`
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `s3_secret_key`

- <strong>Description:</strong> The AWS secret key to access your data. See [mysqld startup options for S3](/kb/en/using-the-s3-storage-engine/#mysqld-startup-options-for-s3).
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-secret-key=val</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> String
- <strong>Default Value:</strong> (Empty)
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `s3_slave_ignore_updates`

- <strong>Description:</strong> Should be set if master and slave share the same S3 instance. This tells the slave that it can ignore any updates to the S3 tables as they are already applied on the master.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-slave-ignore-updates{=0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `0`
- <strong>Introduced:</strong> [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/)

---

#### `s3_use_http`

- <strong>Description:</strong> If enabled, HTTP will be used instead of HTTPS.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--s3-use-http{=0|1}</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> Boolean
- <strong>Default Value:</strong> `0`
- <strong>Introduced:</strong> [MariaDB 10.5.7](/kb/en/mariadb-1057-release-notes/)

---

## See Also

[Using the S3 Storage Engine](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/using-the-s3-storage-engine/)