# Setting up a Replication Slave with Mariabackup

The terms <em>master</em> and <em>slave</em> have historically been used in replication, but the terms terms <em>primary</em> and <em>replica</em> are now preferred. The old terms are used throughout the documentation, and in MariaDB commands, although [MariaDB 10.5](/kb/en/what-is-mariadb-105/) has begun the process of renaming. The documentation will follow over time. See [MDEV-18777](https://jira.mariadb.org/browse/MDEV-18777) to follow progress on this effort.

Mariabackup makes it very easy to set up a [replication slave](/kb/en/high-availability-performance-tuning-mariadb-replication/) using a [full backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup). This page documents how to set up a replication slave from a backup.

If you are using [MariaDB Galera Cluster](/kb/en/galera/), then you may want to try one of the following pages instead:

- [Configuring MariaDB Replication between MariaDB Galera Cluster and MariaDB Server](/replication/galera-cluster/using-mariadb-replication-with-mariadb-galera-cluster/using-mariadb-replication-with-mariadb-galera-cluster-configuring-mariadb-r)
- [Configuring MariaDB Replication between Two MariaDB Galera Clusters](/replication/galera-cluster/using-mariadb-replication-with-mariadb-galera-cluster/configuring-mariadb-replication-between-two-mariadb-galera-clusters)

## Backup the Database and Prepare It

The first step is to simply take and prepare a fresh [full backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup) of a database server in the [replication topology](/kb/en/replication-overview/#common-replication-setups). If the source database server is the desired replication master, then we do not need to add any additional options when taking the full backup. For example:

```sql
$ mariabackup --backup \
   --target-dir=/var/mariadb/backup/ \
   --user=mariabackup --password=mypassword
```

If the source database server is a [replication slave](/kb/en/high-availability-performance-tuning-mariadb-replication/) of the desired replication master, then we should add the <a undefined>--slave-info</a> option, and possibly the <a undefined>--safe-slave-backup</a> option. For example:

```sql
$ mariabackup --backup \
   --slave-info --safe-slave-backup \
   --target-dir=/var/mariadb/backup/ \
   --user=mariabackup --password=mypassword
```

And then we would prepare the backup as you normally would. For example:

```sql
$ mariabackup --prepare \
   --target-dir=/var/mariadb/backup/
```

## Copy the Backup to the New Slave

Once the backup is done and prepared, we can copy it to the new slave. For example:

```sql
$ rsync -avP /var/mariadb/backup dbserver2:/var/mariadb/backup
```

## Restore the Backup on the New Slave

At this point, we can restore the backup to the <a undefined>datadir</a>, as you normally would. For example:

```sql
$ mariabackup --copy-back \
   --target-dir=/var/mariadb/backup/
```

And adjusting file permissions, if necessary:

```sql
$ chown -R mysql:mysql /var/lib/mysql/
```

## Create a Replication User on the Master

Before the new slave can begin replicating from the master, we need to [create a user account](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user) on the master that the slave can use to connect, and we need to [grant](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) the user account the <a undefined>REPLICATION SLAVE</a> privilege. For example:

```sql
CREATE USER 'repl'@'dbserver2' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.*  TO 'repl'@'dbserver2';
```

## Configure the New Slave

Before we start the server on the new slave, we need to configure it. At the very least, we need to ensure that it has a unique <a undefined>server_id</a> value. We also need to make sure other replication settings are what we want them to be, such as the various [GTID system variables](/kb/en/gtid/#system-variables-for-global-transaction-id), if those apply in the specific environment.

Once configuration is done, we can [start the MariaDB Server process](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/) on the new slave.

## Start Replication on the New Slave

At this point, we need to get the replication coordinates of the master from the original backup directory.

If we took the backup on the master, then the coordinates will be in the <a undefined>xtrabackup_binlog_info</a> file. If we took the backup on another slave and if we provided the <a undefined>--slave-info</a> option, then the coordinates will be in the file <a undefined>xtrabackup_slave_info</a> file.

Mariabackup dumps replication coordinates in two forms: [GTID](/replication/standard-replication/gtid) coordinates and [binary log](/mariadb-administration/server-monitoring-logs/binary-log) file and position coordinates, like the ones you would normally see from <a undefined>SHOW MASTER STATUS</a> output. We can choose which set of coordinates we would like to use to set up replication.

For example:

```sql
mariadb-bin.000096 568 0-1-2
```

Regardless of the coordinates we use, we will have to set up the master connection using [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) and then start the replication threads with <a undefined>START SLAVE</a>.

### GTIDs

If we want to use GTIDs, then we will have to first set <a undefined>gtid_slave_pos</a> to the [GTID](/replication/standard-replication/gtid) coordinates that we pulled from either the <a undefined>xtrabackup_binlog_info</a> file or the <a undefined>xtrabackup_slave_info</a> file in the backup directory. For example:

```sql
$ cat xtrabackup_binlog_info
mariadb-bin.000096 568 0-1-2
```

And then we would set `MASTER_USE_GTID=slave_pos` in the [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) command. For example:

```sql
SET GLOBAL gtid_slave_pos = "0-1-2";
CHANGE MASTER TO 
   MASTER_HOST="dbserver1", 
   MASTER_PORT=3310, 
   MASTER_USER="repl",  
   MASTER_PASSWORD="password", 
   MASTER_USE_GTID=slave_pos;
START SLAVE;
```

### File and Position

If we want to use the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) file and position coordinates, then we would set `MASTER_LOG_FILE` and `MASTER_LOG_POS` in the [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) command to the file and position coordinates that we pulled; either the <a undefined>xtrabackup_binlog_info</a> file or the <a undefined>xtrabackup_slave_info</a> file in the backup directory, depending on whether the backup was taken from the master or from a slave of the master. For example:

```sql
CHANGE MASTER TO 
   MASTER_HOST="dbserver1", 
   MASTER_PORT=3310, 
   MASTER_USER="repl",  
   MASTER_PASSWORD="password", 
   MASTER_LOG_FILE='mariadb-bin.000096',
   MASTER_LOG_POS=568;
START SLAVE;
```

## Check the Status of the New Slave

We should be done setting up the slave now, so we should check its status with <a undefined>SHOW SLAVE STATUS</a>. For example:

```sql
SHOW SLAVE STATUS\G
```