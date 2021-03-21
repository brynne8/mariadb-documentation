# Configuring MariaDB Replication between MariaDB Galera Cluster and MariaDB Server

[MariaDB replication](/kb/en/high-availability-performance-tuning-mariadb-replication/) can be used to replication between [MariaDB Galera Cluster](/replication/galera-cluster) and MariaDB Server. This article will discuss how to do that.

## Configuring the Cluster

Before we set up replication, we need to ensure that the cluster is configured properly. This involves the following steps:

- Set <a undefined>log_slave_updates=ON</a> on all nodes in the cluster. See [Configuring MariaDB Galera Cluster: Writing Replicated Write Sets to the Binary Log](/kb/en/configuring-mariadb-galera-cluster/#writing-replicated-write-sets-to-the-binary-log) and [Using MariaDB Replication with MariaDB Galera Cluster: Configuring a Cluster Node as a Replication Master](/kb/en/using-mariadb-replication-with-mariadb-galera-cluster-using-mariadb-replica/#configuring-a-cluster-node-as-a-replication-master) for more information on why this is important. This is also needed to [enable wsrep GTID mode](/kb/en/using-mariadb-gtids-with-mariadb-galera-cluster/#enabling-wsrep-gtid-mode).

- Set <a undefined>server_id</a> to the same value on all nodes in the cluster. See [Using MariaDB Replication with MariaDB Galera Cluster: Setting server_id on Cluster Nodes](/kb/en/using-mariadb-replication-with-mariadb-galera-cluster-using-mariadb-replica/#setting-server_id-on-cluster-nodes) for more information on what this means.

### Configuring Wsrep GTID Mode

If you want to use [GTID](/replication/standard-replication/gtid) replication, then you also need to configure some things to [enable wsrep GTID mode](/kb/en/using-mariadb-gtids-with-mariadb-galera-cluster/#enabling-wsrep-gtid-mode). For example:

- <a undefined>wsrep_gtid_mode=ON</a> needs to be set on all nodes in the cluster.

- <a undefined>wsrep_gtid_domain_id</a> needs to be set to the same value on all nodes in the cluster, so that each cluster node uses the same domain when assigning [GTIDs](/replication/standard-replication/gtid) for Galera Cluster's write sets.

- <a undefined>log_slave_updates</a> needs to be enabled on all nodes in the cluster. See [MDEV-9855](https://jira.mariadb.org/browse/MDEV-9855) about that.

- <a undefined>log_bin</a> needs to be set to the same path on all nodes in the cluster. See [MDEV-9856](https://jira.mariadb.org/browse/MDEV-9856) about that.

And as an extra safety measure:

- <a undefined>gtid_domain_id</a> should be set to a different value on all nodes in a given cluster, and each of these values should be different than the configured <a undefined>wsrep_gtid_domain_id</a> value. This is to prevent a node from using the same domain used for Galera Cluster's write sets when assigning [GTIDs](/replication/standard-replication/gtid) for non-Galera transactions, such as DDL executed with <a undefined>wsrep_sst_method=RSU</a> set or DML executed with <a undefined>wsrep_on=OFF</a> set.

## Configuring the Slave

Before we set up replication, we also need to ensure that the MariaDB Server slave is configured properly. This involves the following steps:

- Set <a undefined>server_id</a> to a different value than the one that the cluster nodes are using.

- Set <a undefined>gtid_domain_id</a> to a value that is different than the <a undefined>wsrep_gtid_domain_id</a> and <a undefined>gtid_domain_id</a> values that the cluster nodes are using.

- Set <a undefined>log_bin</a> and <a undefined>log_slave_updates=ON</a> if you want the slave to log the transactions that it replicates.

## Setting up Replication

Our process to set up replication is going to be similar to the process described at [Setting up a Replication Slave with Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/setting-up-a-replication-slave-with-mariabackup), but it will be modified a bit to work in this context.

### Start the Cluster

The very first step is to start the nodes in the first cluster. The first node will have to be [bootstrapped](/kb/en/getting-started-with-mariadb-galera-cluster/#bootstrapping-a-new-cluster). The other nodes can be [started normally](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

Once the nodes are started, you need to pick a specific node that will act as the replication master for the MariaDB Server.

### Backup the Database on the Cluster's Master Node and Prepare It

The first step is to simply take and prepare a fresh [full backup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup) of the node that you have chosen to be the replication master. For example:

```sql
$ mariabackup --backup \
   --target-dir=/var/mariadb/backup/ \
   --user=mariabackup --password=mypassword
```

And then you would prepare the backup as you normally would. For example:

```sql
$ mariabackup --prepare \
   --target-dir=/var/mariadb/backup/ 
```

### Copy the Backup to the Slave

Once the backup is done and prepared, you can copy it to the MariaDB Server that will be acting as slave. For example:

```sql
$ rsync -avrP /var/mariadb/backup dc2-dbserver1:/var/mariadb/backup
```

### Restore the Backup on the Second Cluster's Slave

At this point, you can restore the backup to the <a undefined>datadir</a>, as you normally would. For example:

```sql
$ mariabackup --copy-back \
   --target-dir=/var/mariadb/backup/ 
```

And adjusting file permissions, if necessary:

```sql
$ chown -R mysql:mysql /var/lib/mysql/
```

### Start the New Slave

Now that the backup has been restored to the MariaDB Server slave, you can [start the MariaDB Server process](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

### Create a Replication User on the Cluster's Master

Before the MariaDB Server slave can begin replicating from the cluster's master, you need to [create a user account](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user) on the master that the slave can use to connect, and you need to [grant](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) the user account the <a undefined>REPLICATION SLAVE</a> privilege. For example:

```sql
CREATE USER 'repl'@'dc2-dbserver1' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.*  TO 'repl'@'dc2-dbserver1';
```

### Start Replication on the New Slave

At this point, you need to get the replication coordinates of the master from the original backup.

The coordinates will be in the <a undefined>xtrabackup_binlog_info</a> file.

Mariabackup dumps replication coordinates in two forms: [GTID strings](/replication/standard-replication/gtid) and [binary log](/mariadb-administration/server-monitoring-logs/binary-log) file and position coordinates, like the ones you would normally see from <a undefined>SHOW MASTER STATUS</a> output. In this case, it is probably better to use the [GTID](/replication/standard-replication/gtid) coordinates.

For example:

```sql
mariadb-bin.000096 568 0-1-2
```

Regardless of the coordinates you use, you will have to set up the master connection using [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) and then start the replication threads with <a undefined>START SLAVE</a>.

#### GTIDs

If you want to use GTIDs, then you will have to first set <a undefined>gtid_slave_pos</a>  to the [GTID](/replication/standard-replication/gtid) coordinates that we pulled from the <a undefined>xtrabackup_binlog_info</a> file, and we would set `MASTER_USE_GTID=slave_pos` in the [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) command. For example:

```sql
SET GLOBAL gtid_slave_pos = "0-1-2";
CHANGE MASTER TO 
   MASTER_HOST="c1dbserver1", 
   MASTER_PORT=3310, 
   MASTER_USER="repl",  
   MASTER_PASSWORD="password", 
   MASTER_USE_GTID=slave_pos;
START SLAVE;
```

#### File and Position

If you want to use the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) file and position coordinates, then you would set `MASTER_LOG_FILE` and `MASTER_LOG_POS` in the [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) command to the file and position coordinates that we pulled the <a undefined>xtrabackup_binlog_info</a> file. For example:

```sql
CHANGE MASTER TO 
   MASTER_HOST="c1dbserver1", 
   MASTER_PORT=3310, 
   MASTER_USER="repl",  
   MASTER_PASSWORD="password", 
   MASTER_LOG_FILE='mariadb-bin.000096',
   MASTER_LOG_POS=568,
START SLAVE;
```

### Check the Status of the New Slave

You should be done setting up the slave now, so you should check its status with <a undefined>SHOW SLAVE STATUS</a>. For example:

```sql
SHOW SLAVE STATUS\G
```

Now that the MariaDB Server is up, ensure that it does not start accepting writes yet if you want to set up [circular replication](/kb/en/replication-overview/#ring-replication) between the cluster and the MariaDB Server.

## Setting up Circular Replication

You can also set up [circular replication](/kb/en/replication-overview/#ring-replication) between the cluster and MariaDB Server, which means that the MariaDB Server replicates from the cluster, and the cluster also replicates from the MariaDB Server.

### Create a Replication User on the MariaDB Server Master

Before circular replication can begin, you also need to [create a user account](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user) on the MariaDB Server, since it will be acting as replication master to the cluster's slave, and you need to [grant](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) the user account the <a undefined>REPLICATION SLAVE</a> privilege. For example:

```sql
CREATE USER 'repl'@'c1dbserver1' IDENTIFIED BY 'password';
GRANT REPLICATION SLAVE ON *.*  TO 'repl'@'c1dbserver1';
```

### Start Circular Replication on the Cluster

How this is done would depend on whether you want to use the [GTID](/replication/standard-replication/gtid) coordinates or the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) file and position coordinates.

Regardless, you need to ensure that the second cluster is not accepting any writes other than those that it replicates from the cluster at this stage.

#### GTIDs

To get the GTID coordinates on the MariaDB Server you can check <a undefined>gtid_current_pos</a> by executing:

```sql
SHOW GLOBAL VARIABLES LIKE 'gtid_current_pos';
```

Then on the node acting as slave in the cluster, you can set up replication by setting <a undefined>gtid_slave_pos</a> to the GTID that was returned and then executing [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to):

```sql
SET GLOBAL gtid_slave_pos = "0-1-2";
CHANGE MASTER TO 
   MASTER_HOST="c2dbserver1", 
   MASTER_PORT=3310, 
   MASTER_USER="repl",  
   MASTER_PASSWORD="password", 
   MASTER_USE_GTID=slave_pos;
START SLAVE;
```

#### File and Position

To get the [binary log](/mariadb-administration/server-monitoring-logs/binary-log) file and position coordinates on the MariaDB Server, you can execute <a undefined>SHOW MASTER STATUS</a>:

```sql
SHOW MASTER STATUS
```

Then on the node acting as slave in the cluster, you would set `master_log_file` and `master_log_pos` in the [CHANGE MASTER TO](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to) command. For example:

```sql
CHANGE MASTER TO 
   MASTER_HOST="c2dbserver1", 
   MASTER_PORT=3310, 
   MASTER_USER="repl",  
   MASTER_PASSWORD="password", 
   MASTER_LOG_FILE='mariadb-bin.000096',
   MASTER_LOG_POS=568;
START SLAVE;
```

### Check the Status of the Circular Replication

You should be done setting up the circular replication on the node in the first cluster now, so you should check its status with <a undefined>SHOW SLAVE STATUS</a>. For example:

```sql
SHOW SLAVE STATUS\G
```