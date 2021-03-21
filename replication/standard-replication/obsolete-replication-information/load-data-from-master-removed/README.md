# LOAD DATA FROM MASTER (removed)

## Syntax

```sql
LOAD DATA FROM MASTER
```

## Description

This feature has been removed from recent versions of MariaDB.

Since the current implementation of <code class="highlight fixed" style="white-space:pre-wrap">LOAD DATA FROM MASTER</code>
and <code class="highlight fixed" style="white-space:pre-wrap">[LOAD TABLE FROM MASTER](/kb/en/load-table-from-master/)</code> is very limited, these statements are deprecated in versions 4.1 of MySQL and above. We will introduce a more
advanced technique (called "online backup") in a future version. That technique
will have the additional advantage of working with more storage engines.

For MySQL 5.1 and earlier, the recommended alternative solution to
using <code class="highlight fixed" style="white-space:pre-wrap">LOAD DATA FROM MASTER</code> or
 <code class="highlight fixed" style="white-space:pre-wrap">LOAD TABLE FROM MASTER</code> is using [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) or [mysqlhotcopy](/clients-utilities/backup-restore-and-import-clients/mysqlhotcopy/).
The latter requires Perl and two Perl modules (DBI and DBD:mysql) and works for
[MyISAM](/kb/en/myisam/) and [ARCHIVE](/columns-storage-engines-and-plugins/storage-engines/archive/) tables only. With mysqldump, you can create SQL dumps on the
master and pipe (or copy) these to a mysql client on the slave. This has the
advantage of working for all storage engines, but can be quite slow, since it
works using <code class="highlight fixed" style="white-space:pre-wrap">SELECT</code>.

This statement takes a snapshot of the master and copies it to the slave. It
updates the values of <code class="highlight fixed" style="white-space:pre-wrap">MASTER_LOG_FILE</code> and
 <code class="highlight fixed" style="white-space:pre-wrap">MASTER_LOG_POS</code> so that the slave starts replicating from
the correct position. Any table and database exclusion rules specified with the
 <code class="highlight fixed" style="white-space:pre-wrap">--replicate-*-do-*</code> and
 <code class="highlight fixed" style="white-space:pre-wrap">--replicate-*-ignore-*</code> options are honored.
 <code class="highlight fixed" style="white-space:pre-wrap">--replicate-rewrite-db</code> is not taken into account because a
user could use this option to set up a non-unique mapping such as
 <code class="highlight fixed" style="white-space:pre-wrap">--replicate-rewrite-db="db1-&gt;db3"</code> and
 <code class="highlight fixed" style="white-space:pre-wrap">--replicate-rewrite-db="db2-&gt;db3"</code>, which would confuse the
slave when loading tables from the master.

Use of this statement is subject to the following conditions:

- It works only for MyISAM tables. Attempting to load a non-MyISAM table
  results in the following error:
  <code class="highlight fixed" style="white-space:pre-wrap">ERROR 1189 (08S01): Net error reading from master</code>
- It acquires a global read lock on the master while taking the snapshot, which
  prevents updates on the master during the load operation.

If you are loading large tables, you might have to increase the values of
 <code class="highlight fixed" style="white-space:pre-wrap">net_read_timeout</code> and <code class="highlight fixed" style="white-space:pre-wrap">net_write_timeout</code> on
both the master and slave servers.
See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/).

Note that <code class="highlight fixed" style="white-space:pre-wrap">LOAD DATA FROM MASTER</code> does not copy any tables
from the mysql database. This makes it easy to have different users and
privileges on the master and the slave.

To use <code class="highlight fixed" style="white-space:pre-wrap">LOAD DATA FROM MASTER</code>, the replication account that
is used to connect to the master must have the <code class="highlight fixed" style="white-space:pre-wrap">RELOAD</code> and
 <code class="highlight fixed" style="white-space:pre-wrap">[SUPER](/kb/en/grant/#global-privileges)</code> privileges on the master and the
 <code class="highlight fixed" style="white-space:pre-wrap">SELECT</code> privilege for all master tables you want to load.
All master tables for which the user does not have the
 <code class="highlight fixed" style="white-space:pre-wrap">SELECT</code> privilege are ignored by
 <code class="highlight fixed" style="white-space:pre-wrap">LOAD DATA FROM MASTER</code>. This is because the master hides
them from the user: <code class="highlight fixed" style="white-space:pre-wrap">LOAD DATA FROM MASTER</code> calls
 <code class="highlight fixed" style="white-space:pre-wrap">SHOW DATABASES</code> to know the master databases to load, but
 <code class="highlight fixed" style="white-space:pre-wrap">[SHOW DATABASES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-databases/)</code> returns only databases
for which the user has some privilege.   On the slave side, the user that
issues <code class="highlight fixed" style="white-space:pre-wrap">LOAD DATA FROM MASTER</code> must have privileges for
dropping and creating the databases and tables that are copied.