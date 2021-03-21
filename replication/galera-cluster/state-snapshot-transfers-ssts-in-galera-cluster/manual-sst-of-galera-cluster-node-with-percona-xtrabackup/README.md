# Manual SST of Galera Cluster Node With Percona XtraBackup

Mariabackup should be used instead of XtraBackup on all supported releases. See [manual SST with Mariabackup](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/manual-sst-of-galera-cluster-node-with-mariabackup/).

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/) is the recommended backup method to use instead of Percona XtraBackup.

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/), Percona XtraBackup is <strong>not supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), Percona XtraBackup is only <strong>partially supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

Sometimes it can be helpful to perform a "manual SST" when Galera's [normal SSTs](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts/) fail. This can be especially useful when the cluster's <a undefined>datadir</a> is very large, since a normal SST can take a long time to fail in that case.

A manual SST essentially consists of taking a backup of the donor, loading the backup on the joiner, and then manually editing the cluster state on the joiner node. This page will show how to perform this process with [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/).

## Process

- Check that the donor and joiner nodes have the same XtraBackup version.

```sql
innobackupex --version
```

- Create backup directory on donor.

```sql
MYSQL_BACKUP_DIR=/mysql_backup
mkdir $MYSQL_BACKUP_DIR
```

- Take a [full backup](https://www.percona.com/doc/percona-xtrabackup/2.4/howtos/recipes_ibkx_local.html) the of the donor node with `innobackupex`. The <a undefined>--galera-info</a> option should also be provided, so that the node's cluster state is also backed up.

```sql
DB_USER=sstuser
DB_USER_PASS=password
innobackupex --user=$DB_USER --password=$DB_USER_PASS --galera-info --no-timestamp $MYSQL_BACKUP_DIR
```

- Verify that the MariaDB Server process is stopped on the joiner node. This will depend on your [service manager](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

For example, on [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) systems, you can execute::

```sql
systemctl status mariadb
```

- Create the backup directory on the joiner node.

```sql
MYSQL_BACKUP_DIR=/mysql_backup
mkdir $MYSQL_BACKUP_DIR
```

- Copy the backup from the donor node to the joiner node.

```sql
OS_USER=dba
JOINER_HOST=dbserver2.mariadb.com
rsync -av $MYSQL_BACKUP_DIR/* ${OS_USER}@${JOINER_HOST}:${MYSQL_BACKUP_DIR}
```

- [Prepare the backup](https://www.percona.com/doc/percona-xtrabackup/2.4/howtos/recipes_ibkx_local.html#prepare-the-backup) on the joiner node.

```sql
innobackupex --apply-log $MYSQL_BACKUP_DIR
```

- Get the Galera Cluster version ID from the donor node's `grastate.dat` file.

```sql
MYSQL_DATADIR=/var/lib/mysql
cat $MYSQL_DATADIR/grastate.dat | grep version
```

For example, a very common version number is "2.1".

- Get the node's cluster state from the <a undefined>--xtrabackup_galera_info</a> file in the backup that was copied to the joiner node.

```sql
cat $MYSQL_BACKUP_DIR/xtrabackup_galera_info
```

Example output:

```sql
d38587ce-246c-11e5-bcce-6bbd0831cc0f:1352215
```

This output is in the format:

```sql
uuid:seqno
```

- Create a `grastate.dat` file in the backup directory of the joiner node. The Galera Cluster version ID, the cluster uuid, and the seqno from previous steps will be used to fill in the relevant fields.

For example, with the example values from the last two steps, we could do:

```sql
sudo tee $MYSQL_BACKUP_DIR/grastate.dat <<EOF
# GALERA saved state
version: 2.1
uuid:    d38587ce-246c-11e5-bcce-6bbd0831cc0f
seqno:   1352215
safe_to_bootstrap: 0
EOF
```

- Remove the existing contents of the <a undefined>datadir</a> on the joiner node.

```sql
MYSQL_DATADIR=/var/lib/mysql
rm -Rf $MYSQL_DATADIR/*
```

- Copy the contents of the backup directory to the <a undefined>datadir</a> the on joiner node.

```sql
cp -R $MYSQL_BACKUP_DIR/* $MYSQL_DATADIR/
```

- Make sure the permissions of the <a undefined>datadir</a> are correct on the joiner node.

```sql
chown -R mysql:mysql $MYSQL_DATADIR/
```

- Start the MariaDB Server process on the joiner node. This will depend on your [service manager](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

For example, on [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) systems, you can execute::

```sql
systemctl start mariadb
```

- Watch the MariaDB [error log](/mariadb-administration/server-monitoring-logs/error-log/) on the joiner node and verify that the node does not need to perform a [normal SSTs](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/introduction-to-state-snapshot-transfers-ssts/) due to the manual SST.

```sql
tail -f /var/log/mysql/mysqld.log
```