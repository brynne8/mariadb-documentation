# BACKUP TABLE (removed)

##### MariaDB until [5.3](/kb/en/what-is-mariadb-53/)

BACKUP TABLE was removed in [MariaDB 5.5](/kb/en/what-is-mariadb-55/).

## Syntax

```sql
BACKUP TABLE tbl_name [, tbl_name] ... TO '/path/to/backup/directory'
```

## Description

<strong>Note:</strong> Like [RESTORE TABLE](/kb/en/restore-table/), this command was not reliable and has been removed in current versions of MariaDB.

For doing a backup of MariaDB use [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) or [MariaDB Backup](/kb/en/mariadb-backup/). See [Backing Up and Restoring](/kb/en/backing-up-and-restoring/).