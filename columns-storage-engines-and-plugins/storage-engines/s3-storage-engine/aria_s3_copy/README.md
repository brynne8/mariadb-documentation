# aria_s3_copy

##### MariaDB starting with [10.5](/kb/en/what-is-mariadb-105/)

The [S3 storage engine](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine) has been available since [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/).

`aria_s3_copy` is a tool for copying an [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) table to and from [S3](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine).

The Aria table must be non transactional and have [ROW_FORMAT=PAGE](/kb/en/aria-storage-formats/#page).

For `aria_s3_copy` to work reliably, the table should not be changed by the MariaDB server during the copy, and one should have first performed [FLUSH TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush) to ensure that the table is properly closed.

Example of properly created Aria table:

```sql
create table test1 (a int) transactional=0 row_format=PAGE engine=aria;
```

Note that when using [ALTER TABLE table_name ENGINE=S3](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/using-the-s3-storage-engine) this restriction doesn't apply.

### Main Arguments

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td>-?, --help</td><td>Display this help and exit.</td></tr>
<tr><td>-k, --s3-access-key=name</td><td>AWS access key ID</td></tr>
<tr><td>-r, --s3-region=name</td><td>AWS region</td></tr>
<tr><td>-K, --s3-secret-key=name</td><td>AWS secret access key ID</td></tr>
<tr><td>-b, --s3-bucket=name</td><td>AWS prefix for tables</td></tr>
<tr><td>-h, --s3-host-name=name</td><td>Host name to S3 provider</td></tr>
<tr><td></td></tr>
<tr><td>-c, --compress</td><td>Use compression</td></tr>
<tr><td>-o, --op=name</td><td>Operation to excecute. One of 'from_s3', 'to_s3' or 'delete_from_s3'</td></tr>
<tr><td>-d, --database=name</td><td>Database for copied table (second prefix). If not given, the directory of the table file is used</td></tr>
<tr><td>-B, --s3-block-size=#</td><td>Block size for data/index blocks in s3</td></tr>
<tr><td>-L, --s3-protocol-version=name</td><td>Protocol used to communication with S3. One of "Amazon" or "Original".</td></tr>
<tr><td>-f, --force</td><td>Force copy even if target exists</td></tr>
<tr><td>-v, --verbose</td><td>Write more information</td></tr>
<tr><td>-V, --version</td><td>Print version and exit.</td></tr>
<tr><td>-#, --debug[=name]</td><td>Output debug log. Often this is 'd:t:o,filename'.</td></tr>
<tr><td>--s3-debug</td><td>Output debug log from marias3 to stdout</td></tr>
</tbody></table>

### Typical Configuration in a my.cnf File

```sql
[aria_s3_copy]
s3-bucket=mariadb
s3-access-key=xxxx
s3-secret-key=xxx
s3-region=eu-north-1
#s3-host-name=s3.amazonaws.com
#s3-protocol-version=Amazon
verbose=1
op=to
```

### Example Usage

The following code will copy an existing Aria table named `test1` to S3.
If the `--database` option is not given, then the directory name where the table files exist will be used as the database.

```sql
shell> aria_s3_copy --force --op=to --database=foo --compress --verbose --s3_block_size=4M test1
Delete of aria table: foo.test1
Delete of index information foo/test1/index
Delete of data information foo/test1/data
Delete of base information and frm
Copying frm file test1.frm
Copying aria table: foo.test1 to s3
Creating aria table information foo/test1/aria
Copying index information foo/test1/index
.
Copying data information foo/test1/data
.
```

When using `--verbose`, `aria_s3_copy` will write a dot for each #/79 part of the file copied.

## See Also

[Using the S3 storage engine](/kb/en/using-the-s3-storage-engine/#aria_s3_copy)