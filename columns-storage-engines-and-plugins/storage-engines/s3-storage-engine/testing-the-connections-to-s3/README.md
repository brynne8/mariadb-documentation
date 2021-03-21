# Testing the Connections to S3

##### MariaDB starting with [10.5](/kb/en/what-is-mariadb-105/)

The [S3 storage engine](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine) has been available since [MariaDB 10.5.4](/kb/en/mariadb-1054-release-notes/).

If you can't get the S3 storage engine to work, here are some steps to help verify where the problem could be.

## S3 Connection Variables

In most cases the problem is to correctly set the S3 connection variables.

The variables are:

- <strong>[s3_access_key](/kb/en/s3-storage-engine-system-variables/#s3_access_key):</strong> The AWS access key to access your data
- <strong>[s3_secret_key](/kb/en/s3-storage-engine-system-variables/#s3_secret_key):</strong> The AWS secret key to access your data
- <strong>[s3_bucket](/kb/en/s3-storage-engine-system-variables/#s3_bucket): </strong> The AWS bucket where your data should be stored. All MariaDB table data is stored in this bucket.
- <strong>[s3_region](/kb/en/s3-storage-engine-system-variables/#s3_region): </strong> The AWS region where your data should be stored.
- <strong>[s3_host_name](/kb/en/s3-storage-engine-system-variables/#s3_host_name):</strong> Hostname for the S3 service.
- <strong>[s3_protocol_version](/kb/en/s3-storage-engine-system-variables/#s3_protocol_version):</strong> Protocol used to communicate with S3. One of "Amazon" or "Original"

There are several ways to ensure you get them right:

### Using aria_s3_copy to Test the Connection

[aria_s3_copy](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/aria_s3_copy) is a tool that allows you to copy [aria](/columns-storage-engines-and-plugins/storage-engines/aria) tables to and from S3.  It's useful for testing the connection as it allows you to specify all s3 options on the command line.

Execute the following sql commands to create a trivial sql table:

```sql
use test;
create table s3_test (a int) engine=aria row_format=page transactional=0;
insert into s3_test values (1),(2);
flush tables s3_test;
```

Now you can use the [aria_s3_copy](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/aria_s3_copy) tool to copy this to S3 from your
shell/the command line:

```sql
shell> cd mariadb-data-directory/test
shell> aria_s3_copy --op=to --verbose --force --**other*options* s3_test.frm

Copying frm file s3_test.frm
Copying aria table: test.s3_test to s3
Creating aria table information test/s3_test/aria
Copying index information test/s3_test/index
Copying data information test/s3_test/data
```

As you can see from the above, [aria_s3_copy](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/aria_s3_copy) is using the current directory as the database name.

You can also set the [aria_s3_copy](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/aria_s3_copy) options in your my.cnf file to avoid
some typing.

### Using mysql-test-run to Test the Connection and the S3 Storage Engine

One can use the [MariaDB test system](/clients-utilities/mysqltest) to run all default S3 test against your S3 storage.

To do that you have to locate the `mysql-test` directory in your system and
`cd` to it.

The config file for the S3 test system can be found at `suite/s3/my.cnf`.
To enable testing you have to edit this file and add the s3 connection options
to the end of the file. It should look something like this after editing:

```sql
!include include/default_mysqld.cnf
!include include/default_client.cnf

[mysqld.1]
s3=ON
#s3-host-name=s3.amazonaws.com
#s3-protocol-version=Amazon
s3-bucket=MariaDB
s3-access-key=
s3-secret-key=
s3-region=
```

You must give values for `s3-access-key`, `s3-secret-key` and `s3-region` that reflects your S3 provider. The `s3-bucket` name is defined by your administrator.

If you are not using Amazon Web Services as your S3 provider you must
also specify `s3-hostname` and possibly change
`s3-protocol-version` to "Original".

Now you can test the configuration:

```sql
shell> cd **mysql-test** directory
shell> ./mysql-test-run --suite=s3
...
s3.no_s3                                 [ pass ]      5
s3.alter                                 [ pass ]  11073
s3.arguments                             [ pass ]   2667
s3.basic                                 [ pass ]   2757
s3.discovery                             [ pass ]   7851
s3.select                                [ pass ]   1325
s3.unsupported                           [ pass ]    363
```

Note that there may be more tests in your output as we are constantly adding more tests to S3 when needed.

## What to Do Next

When you got the connection to work, you should add the options to your global my.cnf file.
Now you can start testing S3 from your [mysql command client](/clients-utilities/mysql-client/mysql-command-line-client) by converting some existing table to S3 with [ALTER TABLE ... ENGINE=S3](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/using-the-s3-storage-engine).

## See Also

- [Using the S3 Storage Engine](/columns-storage-engines-and-plugins/storage-engines/s3-storage-engine/using-the-s3-storage-engine)
- [Using MinIO with mysql-test-run to test the S3 storage engine](/clients-utilities/mysqltest/installing-minio-for-usage-with-mysql-test-run)