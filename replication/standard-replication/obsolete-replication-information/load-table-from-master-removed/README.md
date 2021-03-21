# LOAD TABLE FROM MASTER (removed)

## Syntax

```sql
LOAD TABLE tbl_name FROM MASTER
```

## Description

This feature has been removed from recent versions of MariaDB.

Since the current implementation of <code class="highlight fixed" style="white-space:pre-wrap">[LOAD DATA FROM MASTER](/replication/standard-replication/obsolete-replication-information/load-data-from-master-removed)</code>
and <code class="highlight fixed" style="white-space:pre-wrap">LOAD TABLE FROM MASTER</code> is very limited, these statements
are deprecated in versions 4.1 of MySQL and above. We will introduce a more
advanced technique (called "online backup") in a future version. That technique
will have the additional advantage of working with more storage engines.

For MariaDB and MySQL 5.1 and earlier, the recommended alternative solution to
using <code class="highlight fixed" style="white-space:pre-wrap">LOAD DATA FROM MASTER</code> or
 <code class="highlight fixed" style="white-space:pre-wrap">LOAD TABLE FROM MASTER</code> is using [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump) or [mysqlhotcopy](/clients-utilities/backup-restore-and-import-clients/mysqlhotcopy).
The latter requires Perl and two Perl modules (DBI and DBD:mysql) and works for
[MyISAM](/kb/en/myisam/) and [ARCHIVE](/columns-storage-engines-and-plugins/storage-engines/archive) tables only. With mysqldump, you can create SQL dumps on the
master and pipe (or copy) these to a mysql client on the slave. This has the
advantage of working for all storage engines, but can be quite slow, since it
works using <code class="highlight fixed" style="white-space:pre-wrap">SELECT</code>.

Transfers a copy of the table from the master to the slave. This statement is
implemented mainly debugging <code class="highlight fixed" style="white-space:pre-wrap">LOAD DATA FROM MASTER</code>
operations. To use <code class="highlight fixed" style="white-space:pre-wrap">LOAD TABLE</code>, the account used for
connecting to the master server must have the <code class="highlight fixed" style="white-space:pre-wrap">RELOAD</code> and
 <code class="highlight fixed" style="white-space:pre-wrap">[SUPER](/kb/en/grant/#global-privileges)</code> privileges on the master and the
 <code class="highlight fixed" style="white-space:pre-wrap">SELECT</code> privilege for the master table to load. On the slave
side, the user that issues <code class="highlight fixed" style="white-space:pre-wrap">LOAD TABLE FROM MASTER</code> must have
privileges for dropping and creating the table.

The conditions for <code class="highlight fixed" style="white-space:pre-wrap">LOAD DATA FROM MASTER</code> apply here as well.
For example, <code class="highlight fixed" style="white-space:pre-wrap">LOAD TABLE FROM MASTER</code> works only for MyISAM
tables. The timeout notes for <code class="highlight fixed" style="white-space:pre-wrap">LOAD DATA FROM MASTER</code> apply as
well.