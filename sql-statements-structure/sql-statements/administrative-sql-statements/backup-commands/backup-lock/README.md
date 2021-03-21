# BACKUP LOCK

##### MariaDB starting with [10.4.2](/kb/en/mariadb-1042-release-notes/)

The BACKUP LOCK command was introduced in [MariaDB 10.4.2](/kb/en/mariadb-1042-release-notes/).

BACKUP LOCK blocks a table from DDL statements. This is mainly intended to be used by tools like [mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/) that need to ensure there are no DDLs on a table while the table files are opened. For example, for an Aria table that stores data in 3 files with extensions .frm, .MAI and .MAD.
Normal read/write operations can continue as normal.

## Syntax

To lock a table:

```sql
BACKUP LOCK table_name
```

To unlock a table:

```sql
BACKUP UNLOCK
```

## Usage in a Backup Tool

```sql
BACKUP LOCK [database.]table_name;
 - Open all files related to a table (for example, t.frm, t.MAI and t.MYD)
BACKUP UNLOCK;
- Copy data
- Close files
```

This ensures that all files are from the same generation, that is created at the same time by the MariaDB server.
This works, because the open files will point to the original table files which will not be affected if there is
any ALTER TABLE while copying the files.

## Privileges

BACKUP LOCK requires the [RELOAD](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) privilege.

## Notes

- The idea is that the `BACKUP LOCK` should be held for as short a time as possible by the backup tool. The time to take an uncontested lock is very short! One can easily do 50,000 locks/unlocks per second on low end hardware.
- One should use different connections for [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage/) commands and `BACKUP LOCK`.

## Implementation

- Internally, BACKUP LOCK is implemented by taking an `MDLSHARED_HIGH_PRIO` MDL lock on the table object, which protects the table from any DDL operations.

## See Also

- [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage/)
- [MDEV-17309](https://jira.mariadb.org/browse/MDEV-17309) - BACKUP LOCK: DDL locking of tables during backup