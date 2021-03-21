# Replication as a Backup Solution

[Replication](/replication) can be used to support the [backup](/kb/en/backing-up-and-restoring/) strategy.

Replication alone is <em>not</em> sufficient for backup. It assists in protecting against hardware failure on the master server, but does not protect against data loss. An accidental or malicious `DROP DATABASE` or `TRUNCATE TABLE` statement will be replicated onto the slaves as well. Care needs to be taken to prevent data getting out of sync between the master and the slave.

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

Replication is most commonly used to support backups as follows:

- A master server replicates to a slave server
- Backups are then run off the slave without any impact on the master.

Backups can have a significant effect on a server, and a high-availability master may not be able to be stopped, locked or simply handle the extra load of a backup. Running the backup from a slave has the advantage of being able to shutdown or lock the slave and perform a backup without any impact on the primary server.

Note that when backing up off a slave server, it is important to ensure that the servers keep the data in sync. See for example [Replication and Foreign Keys](/replication/standard-replication/replication-and-foreign-keys) for a situation when identical statements can result in different data on a slave and a master.

## See Also

- [Replication](/replication)
- [Replication Compatibility](/kb/en/mariadb-vs-mysql-compatibility/#replication-compatibility)
- [Backing Up and Restoring](/kb/en/backing-up-and-restoring/)