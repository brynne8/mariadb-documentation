# RESTORE TABLE (removed)

##### MariaDB until [5.3](/kb/en/what-is-mariadb-53/)

RESTORE TABLE was removed in [MariaDB 5.5](/kb/en/what-is-mariadb-55/).

## Syntax

```sql
RESTORE TABLE tbl_name [, tbl_name] ... FROM '/path/to/backup/directory'```

## Description

<strong>Note:</strong> Like [BACKUP TABLE](/kb/en/backup-table/), this command was not reliable and has been removed in current versions of MariaDB. For doing a backup of MariaDB use [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump), [mysqlhotcopy](/clients-utilities/backup-restore-and-import-clients/mysqlhotcopy) or [XtraBackup](/kb/en/backup-restore-and-import-xtrabackup/). See [Backing Up and Restoring](/kb/en/backing-up-and-restoring/).

<code class="highlight fixed" style="white-space:pre-wrap">RESTORE TABLE</code> restores the table or tables from a backup
that was made with <code class="highlight fixed" style="white-space:pre-wrap">[BACKUP TABLE](/kb/en/backup-table/)</code>. The
directory should be specified as a full path name.

Existing tables are not overwritten; if you try to restore over an existing
table, an error occurs. Just as for <code class="highlight fixed" style="white-space:pre-wrap">BACKUP TABLE</code>,
<code class="highlight fixed" style="white-space:pre-wrap">RESTORE TABLE</code> works only for [MyISAM](/kb/en/myisam/) tables.
Restored tables are not replicated from master to slave.

The backup for each table consists of its .frm format file and .MYD
data file. The restore operation restores those files, and then uses
them to rebuild the .MYI index file. Restoring takes longer than
backing up due to the need to rebuild the indexes. The more indexes the
table has, the longer it takes.