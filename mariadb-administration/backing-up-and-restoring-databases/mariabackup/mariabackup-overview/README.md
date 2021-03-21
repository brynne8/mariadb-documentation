# Mariabackup Overview

##### MariaDB starting with [10.1.23](/kb/en/mariadb-10123-release-notes/)

Mariabackup was first released in [MariaDB 10.1.23](/kb/en/mariadb-10123-release-notes/) and [MariaDB 10.2.7](/kb/en/mariadb-1027-release-notes/). It was first released as GA in [MariaDB 10.1.26](/kb/en/mariadb-10126-release-notes/) and [MariaDB 10.2.10](/kb/en/mariadb-10210-release-notes/).

<strong>Mariabackup</strong> is an open source tool provided by MariaDB for performing physical online backups of [InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb), [Aria](/columns-storage-engines-and-plugins/storage-engines/aria) and [MyISAM](/kb/en/myisam/) tables. For InnoDB, “hot online” backups are possible. It was originally forked from [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/) 2.3.8. It is available on Linux and Windows.

## Backup Support for MariaDB-Exclusive Features

[MariaDB 10.1](/kb/en/what-is-mariadb-101/) introduced features that are exclusive to MariaDB, such as [InnoDB Page Compression](InnoDB_compression) and [Data-at-Rest Encryption](/kb/en/data-at-rest-encryption/). These exclusive features have been very popular with MariaDB users. However, existing backup solutions from the MySQL ecosystem, such as [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/), did not support full backup capability for these features.

To address the needs of our users, we decided to develop a backup solution that would fully support these popular MariaDB-exclusive features. We did this by creating Mariabackup, which is based on the well-known and commonly used backup tool called [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/). Mariabackup was originally extended from version 2.3.8.

### Supported Features

Mariabackup supports all of the main features of [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/) 2.3.8, plus:

- Backup/Restore of tables using [Data-at-Rest Encryption](/kb/en/data-at-rest-encryption/).
- Backup/Restore of tables using [InnoDB Page Compression](InnoDB_compression).
- [mariabackup SST method](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/mariabackup-sst-method) with Galera Cluster.
- Microsoft Windows support.
- Backup/Restore of tables using the [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks) storage engine starting with [MariaDB 10.2.16](/kb/en/mariadb-10216-release-notes/) and [MariaDB 10.3.8](/kb/en/mariadb-1038-release-notes/). See [Files Backed up by Mariabackup: MyRocks Data Files](/kb/en/files-backed-up-by-mariabackup/#myrocks-data-files) for more information.

#### Supported Features in MariaDB Enterprise Backup

[MariaDB Enterprise Backup](https://mariadb.com/docs/usage/mariadb-enterprise-backup/) supports some additional features, such as:

- Minimizes locks during the backup to permit more concurrency and to enable faster backups.
<ul start="1"><li>This relies on the usage of [BACKUP STAGE](/sql-statements-structure/sql-statements/administrative-sql-statements/backup-commands/backup-stage) commands and DDL logging.
</li><li>This includes no locking during the copy phase of [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table) statements, which tends to be the longest phase of these statements.
</li></ul>
- Provides optimal backup support for all storage engines that store things on local disk.

### Differences Compared to Percona XtraBackup

- Percona XtraBackup copies its [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) files to the file `xtrabackup_logfile`, while Mariabackup uses the file <a undefined>ib_logfile0</a>.

- Percona XtraBackup's [libgcrypt-based encryption of backups](https://www.percona.com/doc/percona-xtrabackup/2.3/backup_scenarios/encrypted_backup.html) is not supported by Mariabackup.

- There is no symbolic link from `mariabackup` to <a undefined>innobackupex</a>, as there is for <a undefined>xtrabackup</a>. Instead, `mariabackup` has the <a undefined>--innobackupex</a> command-line option to enable innobackupex-compatible options.

- The <a undefined>--compact</a> and <a undefined>--rebuild_indexes</a> options are not supported.

- Support for <a undefined>--stream=tar</a> was removed from Mariabackup in [MariaDB 10.1.24](/kb/en/mariadb-10124-release-notes/).

- The <a undefined>xbstream</a> utility has been renamed to `mbstream`. However, to select this output format when creating a backup, Mariabackup's <a undefined>--stream</a> option still expects the `xbstream` value.

- Mariabackup does not support [lockless binlog](https://www.percona.com/doc/percona-xtrabackup/2.3/advanced/lockless_bin-log.html).

#### Difference in Versioning Schemes

Each Percona XtraBackup release has two version numbers--the Percona XtraBackup version number and the version number of the MySQL Server release that it is based on. For example:

```sql
xtrabackup version 2.2.8 based on MySQL server 5.6.22
```

Each Mariabackup release only has one version number, and it is the same as the version number of the MariaDB Server release that it is based on. For example:

```sql
mariabackup based on MariaDB server 10.2.15-MariaDB Linux (x86_64)
```

See [Compatibility of Mariabackup Releases with MariaDB Server Releases](#compatibility-of-mariabackup-releases-with-mariadb-server-releases) for more information on Mariabackup versions.

## Compatibility of Mariabackup Releases with MariaDB Server Releases

A MariaDB Server version can often be backed up with most other Mariabackup releases in the same release series. For example, [MariaDB 10.2.21](/kb/en/mariadb-10221-release-notes/) and [MariaDB 10.2.22](/kb/en/mariadb-10222-release-notes/) are both in the [MariaDB 10.2](/kb/en/what-is-mariadb-102/) release series, so MariaDB Server from [MariaDB 10.2.21](/kb/en/mariadb-10221-release-notes/) could be backed up by Mariabackup from [MariaDB 10.2.22](/kb/en/mariadb-10222-release-notes/), or vice versa.

However, occasionally, a MariaDB Server or Mariabackup release will include bug fixes that will break compatibility with previous releases. For example, the fix for [MDEV-13564](https://jira.mariadb.org/browse/MDEV-13564) changed the [InnoDB redo log](/columns-storage-engines-and-plugins/storage-engines/innodb/innodb-redo-log) format in [MariaDB 10.2.19](/kb/en/mariadb-10219-release-notes/) which broke compatibility with previous releases. To be safest, a MariaDB Server release should generally be backed up with the Mariabackup release that has the same version number.

Mariabackup from [MariaDB 10.1](/kb/en/what-is-mariadb-101/) releases may also be able to back up MariaDB Server from [MariaDB 5.5](/kb/en/what-is-mariadb-55/) and [MariaDB 10.0](/kb/en/what-is-mariadb-100/) releases in many cases. However, this is not fully supported. See [MDEV-14936](https://jira.mariadb.org/browse/MDEV-14936) for more information.

## Installing Mariabackup

### Installing on Linux

The `mariabackup` executable is included in [binary tarballs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs) on Linux.

#### Installing with a Package Manager

Mariabackup can also be installed via a package manager on Linux. In order to do so, your system needs to be configured to install from one of the MariaDB repositories.

You can configure your package manager to install it from MariaDB Corporation's MariaDB Package Repository by using the [MariaDB Package Repository setup script](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage).

You can also configure your package manager to install it from MariaDB Foundation's MariaDB Repository by using the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/).

##### Installing with yum/dnf

On RHEL, CentOS, Fedora, and other similar Linux distributions, it is highly recommended to install the relevant [RPM package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm) from MariaDB's
repository using [yum](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/yum) or <a undefined>dnf</a>. Starting with RHEL 8 and Fedora 22, `yum` has been replaced by `dnf`, which is the next major version of `yum`. However, `yum` commands still work on many systems that use `dnf`. For example:

```sql
sudo yum install MariaDB-backup
```

##### Installing with apt-get

On Debian, Ubuntu, and other similar Linux distributions, it is highly recommended to install the relevant [DEB package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) from MariaDB's
repository using <a undefined>apt-get</a>. For example:

```sql
sudo apt-get install mariadb-backup
```

##### Installing with zypper

On SLES, OpenSUSE, and other similar Linux distributions, it is highly recommended to install the relevant [RPM package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm) from MariaDB's repository using [zypper](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/installing-mariadb-with-zypper). For example:

```sql
sudo zypper install MariaDB-backup
```

### Installing on Windows

The `mariabackup` executable is included in [MSI](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-msi-packages-on-windows) and [ZIP](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-windows-zip-packages) packages on Windows.

When using the [Windows MSI installer](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-msi-packages-on-windows), `mariabackup` can be installed by selecting <em>Backup utilities</em>:

<img src="/kb/en/mariabackup-overview/+image/mariadb_backup_windows" alt="mariadb_backup_windows" title="mariadb_backup_windows">

## Using Mariabackup

The command to use `mariabackup` and the general syntax is:

```sql
mariabackup <options>
```

For in-depth explanations on how to use Mariabackup, see:

- [Full Backup and Restore with Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/full-backup-and-restore-with-mariabackup)
- [Incremental Backup and Restore with Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/incremental-backup-and-restore-with-mariabackup)
- [Partial Backup and Restore with Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/partial-backup-and-restore-with-mariabackup)
- [Restoring Individual Tables and Partitions with Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/restoring-individual-tables-and-partitions-with-mariabackup)
- [Setting up a Replication Slave with Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/setting-up-a-replication-slave-with-mariabackup)
- [Using Encryption and Compression Tools With Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/using-encryption-and-compression-tools-with-mariabackup)

### Options

Options supported by Mariabackup can be found [here](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/mariabackup-options).

`mariabackup` will currently silently ignore unknown command-line options, so be extra careful about accidentally including typos in options or accidentally using options from later `mariabackup` versions. The reason for this is that `mariabackup` currently treats command-line options and options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) equivalently. When it reads from these [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), it has to read a lot of options from the [server option groups](#server-option-groups) read by [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options). However, `mariabackup` does not know about many of the options that it normally reads in these option groups. If `mariabackup` raised an error or warning when it encountered an unknown option, then this process would generate a large amount of log messages under normal use. Therefore, `mariabackup` is designed to silently ignore the unknown options instead. See [MDEV-18215](https://jira.mariadb.org/browse/MDEV-18215) about that.

### Option Files

In addition to reading options from the command-line, Mariabackup can also read options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).

The following options relate to how MariaDB command-line tools handles option files. They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file.</td></tr>
<tr><td><code>--defaults-file=# </code></td><td>Only read default options from the given option file.</td></tr>
<tr><td><code>--defaults-extra-file=# </code></td><td>Read this file after the global files are read.</td></tr>
<tr><td><code>--defaults-group-suffix=# </code></td><td>In addition to the default option groups, also read option groups with this suffix.</td></tr>
</tbody></table>

#### Server Option Groups

Mariabackup reads server options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mariabackup]</code></td><td>Options read by Mariabackup. Available starting with <a href="/kb/en/mariadb-10131-release-notes/">MariaDB 10.1.31</a> and <a href="/kb/en/mariadb-10213-release-notes/">MariaDB 10.2.13</a>.</td></tr>
<tr><td><code>[mariadb-backup]</code></td><td>Options read by Mariabackup.  Available starting with <a href="/kb/en/mariadb-10414-release-notes/">MariaDB 10.4.14</a> and <a href="/kb/en/mariadb-1054-release-notes/">MariaDB 10.5.4</a>.</td></tr>
<tr><td><code>[xtrabackup]</code></td><td>Options read by Mariabackup and <a href="/kb/en/percona-xtrabackup-overview/">Percona XtraBackup</a>.</td></tr>
<tr><td><code>[server]</code></td><td>Options read by MariaDB Server. Available starting with <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-10222-release-notes/">MariaDB 10.2.22</a>, and <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a>.</td></tr>
<tr><td><code>[mysqld]</code></td><td>&nbsp;Options read by <code>mysqld</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mysqld-X.Y]</code></td><td>&nbsp;Options read by a specific version of <code>mysqld</code>, which includes both MariaDB Server and MySQL Server. For example, <code>[mysqld-10.4]</code>. Available starting with <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-10222-release-notes/">MariaDB 10.2.22</a>, and <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a>.</td></tr>
<tr><td><code>[mariadb]</code></td><td>Options read by MariaDB Server. Available starting with <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-10222-release-notes/">MariaDB 10.2.22</a>, and <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a>.</td></tr>
<tr><td><code>[mariadb-X.Y]</code></td><td>&nbsp;Options read by a specific version of MariaDB Server. For example, <code>[mariadb-10.4]</code>. Available starting with <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-10222-release-notes/">MariaDB 10.2.22</a>, and <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a>.</td></tr>
<tr><td><code>[mariadbd]</code></td><td>Options read by MariaDB Server. Available starting with <a href="/kb/en/mariadb-10414-release-notes/">MariaDB 10.4.14</a> and <a href="/kb/en/mariadb-1054-release-notes/">MariaDB 10.5.4</a>.</td></tr>
<tr><td><code>[mariadbd-X.Y]</code></td><td>&nbsp;Options read by a specific version of MariaDB Server. For example, <code>[mariadbd-10.4]</code>. Available starting with <a href="/kb/en/mariadb-10414-release-notes/">MariaDB 10.4.14</a> and <a href="/kb/en/mariadb-1054-release-notes/">MariaDB 10.5.4</a>.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients. Available starting with <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-10222-release-notes/">MariaDB 10.2.22</a>, and <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a>.</td></tr>
<tr><td><code>[galera]</code></td><td>&nbsp;Options read by MariaDB Server, but only if it is compiled with <a href="/kb/en/galera/">Galera Cluster</a> support. In <a href="/kb/en/what-is-mariadb-101/">MariaDB 10.1</a> and later, all builds on Linux are compiled with <a href="/kb/en/galera/">Galera Cluster</a> support. When using one of these builds, options from this option group are read even if the <a href="/kb/en/galera/">Galera Cluster</a> functionality is not enabled. Available starting with <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-10222-release-notes/">MariaDB 10.2.22</a>, and <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a> on systems compiled with <a href="/kb/en/galera/">Galera Cluster</a> support.</td></tr>
</tbody></table>

#### Client Option Groups

Mariabackup reads client options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mariabackup]</code></td><td>Options read by Mariabackup. Available starting with <a href="/kb/en/mariadb-10131-release-notes/">MariaDB 10.1.31</a> and <a href="/kb/en/mariadb-10213-release-notes/">MariaDB 10.2.13</a>.</td></tr>
<tr><td><code>[mariadb-backup]</code></td><td>Options read by Mariabackup.  Available starting with <a href="/kb/en/mariadb-10414-release-notes/">MariaDB 10.4.14</a> and <a href="/kb/en/mariadb-1054-release-notes/">MariaDB 10.5.4</a>.</td></tr>
<tr><td><code>[xtrabackup]</code></td><td>Options read by Mariabackup and <a href="/kb/en/percona-xtrabackup-overview/">Percona XtraBackup</a>.</td></tr>
<tr><td><code>[client]</code></td><td>&nbsp;Options read by all MariaDB and MySQL <a href="/kb/en/clients-utilities/">client programs</a>, which includes both MariaDB and MySQL clients. For example, <code>mysqldump</code>.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients. Available starting with <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-10222-release-notes/">MariaDB 10.2.22</a>, and <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a>.</td></tr>
<tr><td><code>[client-mariadb]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a>. Available starting with <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-10222-release-notes/">MariaDB 10.2.22</a>, and <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a>.</td></tr>
</tbody></table>

### Authentication and Privileges

Mariabackup needs to authenticate with the database server when it performs a backup operation (i.e. when the <a undefined>--backup</a> option is specified). For most use cases, the user account that performs the backup needs to have the `RELOAD` , `PROCESS`, `LOCK TABLES` and `REPLICATION CLIENT` [global privileges](/kb/en/grant/#global-privileges) on the database server. For example:

```sql
CREATE USER 'mariabackup'@'localhost' IDENTIFIED BY 'mypassword';
GRANT RELOAD, PROCESS, LOCK TABLES, REPLICATION CLIENT ON *.* TO 'mariabackup'@'localhost';
```

If your database server is also using the [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks) storage engine, then the user account that performs the backup will also need the `SUPER` [global privilege](/kb/en/grant/#global-privileges). This is because Mariabackup creates a checkpoint of this data by setting the <a undefined>rocksdb_create_checkpoint</a> system variable, which requires this privilege. See [MDEV-20577](https://jira.mariadb.org/browse/MDEV-20577) for more information.

To use the <a undefined>--history</a> option, the backup user also needs to have the following privileges granted:

```sql
GRANT CREATE ON PERCONA_SCHEMA.* TO 'mariabackup'@'localhost';
GRANT INSERT ON PERCONA_SCHEMA.* TO 'mariabackup'@'localhost';
```

The user account information can be specified with the <a undefined>--user</a> and <a undefined>--password</a> command-line options. For example:

```sql
$ mariabackup --backup \
   --target-dir=/var/mariadb/backup/ \
   --user=mariabackup --password=mypassword
```

The user account information can also be specified in a supported [client option group](#client-option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariabackup]
user=mariabackup
password=mypassword
```

Mariabackup does not need to authenticate with the database server when preparing or restoring a backup.

### File System Permissions

Mariabackup has to read MariaDB's files from the file system. Therefore, when you run Mariabackup as a specific operating system user, you should ensure that user account has sufficient permissions to read those files.

If you are using Linux and if you installed MariaDB with a package manager, then MariaDB's files will probably be owned by the `mysql` user and the `mysql` group.

### Using Mariabackup with Data-at-Rest Encryption

Mariabackup supports [Data-at-Rest Encryption](/kb/en/data-at-rest-encryption/).

Mariabackup will query the server to determine which [key management and encryption plugin](/mariadb-administration/user-server-security/securing-mariadb/securing-mariadb-encryption/securing-mariadb-data-at-rest-encryption/key-management-and-encryption-plugins/encryption-key-management) is being used, and then it will load that plugin itself, which means that Mariabackup needs to be able to load the key management and encryption plugin's shared library.

Mariabackup will also query the server to determine which [encryption keys](/kb/en/encryption-key-management/#using-multiple-encryption-keys) it needs to use.

In other words, Mariabackup is able to figure out a lot of encryption-related information on its own, so normally one doesn't need to provide any extra options to backup or restore encrypted tables.

Mariabackup backs up encrypted and unencrypted tables as they are on the original server. If a table is encrypted, then the table will remain encrypted in the backup. Similarly, if a table is unencrypted, then the table will remain unencrypted in the backup.

The primary reason that Mariabackup needs to be able to encrypt and decrypt data is that it needs to apply [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) records to make the data consistent when the backup is prepared. As a consequence, Mariabackup does not perform many encryption or decryption operations when the backup is initially taken. MariaDB performs more encryption and decryption operations when the backup is prepared. This means that some encryption-related problems (such as using the wrong encryption keys) may not become apparent until the backup is prepared.

### Using Mariabackup for Galera SSTs

The `mariabackup` SST method uses the [Mariabackup](/kb/en/backup-restore-and-import-clients-mariadb-backup/) utility for performing SSTs. See [mariabackup SST method](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/mariabackup-sst-method) for more information.

## Files Backed up by Mariabackup

Mariabackup backs up many different files in order to perform its backup operation. See [Files Backed up by Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/files-backed-up-by-mariabackup) for a list of these files.

## Files Created by Mariabackup

Mariabackup creates several different types of files during the backup and prepare phases. See [Files Created by Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/files-created-by-mariabackup) for a list of these files.

## Known Issues

### Unsupported Server Option Groups

Mariabackup doesn't currently support the `[mariadbd]` and `[mariadbd-X.Y]` server option groups. See [MDEV-21298](https://jira.mariadb.org/browse/MDEV-21298) for more information.

Mariabackup doesn't currently support the `[mariadb-backup]` server option group. See [MDEV-21301](https://jira.mariadb.org/browse/MDEV-21301) for more information.

Prior to [MariaDB 10.1.38](/kb/en/mariadb-10138-release-notes/), [MariaDB 10.2.22](/kb/en/mariadb-10222-release-notes/), and [MariaDB 10.3.13](/kb/en/mariadb-10313-release-notes/), Mariabackup doesn't read server options from all [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) supported by the server. In those versions, it only looks for server options in the following server option groups:

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[xtrabackup]</code></td><td>Options read by Percona XtraBackup and Mariabackup.</td></tr>
<tr><td><code>[mariabackup]</code></td><td>Options read by Percona XtraBackup and Mariabackup. Available starting with <a href="/kb/en/mariadb-10131-release-notes/">MariaDB 10.1.31</a> and <a href="/kb/en/mariadb-10213-release-notes/">MariaDB 10.2.13</a>.</td></tr>
<tr><td><code>[mysqld]</code></td><td>&nbsp;Options read by <code>mysqld</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
</tbody></table>

Those versions do not read server options from the following option groups supported by the server:

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[server]</code></td><td>Options read by MariaDB Server. Available starting with <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-10222-release-notes/">MariaDB 10.2.22</a>, and <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a>.</td></tr>
<tr><td><code>[mysqld-X.Y]</code></td><td>&nbsp;Options read by a specific version of <code>mysqld</code>, which includes both MariaDB Server and MySQL Server. For example, <code>[mysqld-5.5]</code> Available starting with <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-10222-release-notes/">MariaDB 10.2.22</a>, and <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a>.</td></tr>
<tr><td><code>[mariadb]</code></td><td>Options read by MariaDB Server. Available starting with <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-10222-release-notes/">MariaDB 10.2.22</a>, and <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a>.</td></tr>
<tr><td><code>[mariadb-X.Y]</code></td><td>&nbsp;Options read by a specific version of MariaDB Server. For example, <code>[mariadb-10.3]</code> Available starting with <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-10222-release-notes/">MariaDB 10.2.22</a>, and <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a>.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients. Available starting with <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-10222-release-notes/">MariaDB 10.2.22</a>, and <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a>.</td></tr>
<tr><td><code>[galera]</code></td><td>&nbsp;Options read by MariaDB Server, but only if it is compiled with <a href="/kb/en/galera/">Galera Cluster</a> support. In <a href="/kb/en/what-is-mariadb-101/">MariaDB 10.1</a> and later, all builds on Linux are compiled with <a href="/kb/en/galera/">Galera Cluster</a> support. When using one of these builds, options from this option group are read even if the <a href="/kb/en/galera/">Galera Cluster</a> functionality is not enabled. Available starting with <a href="/kb/en/mariadb-10138-release-notes/">MariaDB 10.1.38</a>, <a href="/kb/en/mariadb-10222-release-notes/">MariaDB 10.2.22</a>, and <a href="/kb/en/mariadb-10313-release-notes/">MariaDB 10.3.13</a> on systems compiled with <a href="/kb/en/galera/">Galera Cluster</a> support.</td></tr>
</tbody></table>

See [MDEV-18347](https://jira.mariadb.org/browse/MDEV-18347) for more information.

### No Default Datadir

Prior to [MariaDB 10.1.36](/kb/en/mariadb-10136-release-notes/), [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/), and [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/), if you were performing a <a undefined>--copy-back</a> operation, and if you did not explicitly specify a value for the <a undefined>datadir</a> option either on the command line or one of the supported [server option groups](#server-option-group) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), then Mariabackup would not default to the server's default `datadir`. Instead, Mariabackup would fail with an error. For example:

```sql
Error: datadir must be specified.
```

The solution is to explicitly specify a value for the <a undefined>datadir</a> option either on the command line or in one of the supported [server option groups](#server-option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mysqld]
datadir=/var/lib/mysql
```

In [MariaDB 10.1.36](/kb/en/mariadb-10136-release-notes/), [MariaDB 10.2.18](/kb/en/mariadb-10218-release-notes/), and [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/) and later, Mariabackup will default to the server's default <a undefined>datadir</a> value.

See [MDEV-12956](https://jira.mariadb.org/browse/MDEV-12956) for more information.

### Concurrent DDL and Backup Issues

Prior to [MariaDB 10.2.19](/kb/en/mariadb-10219-release-notes/) and [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/), if concurrent DDL was executed while the backup was taken, then that could cause various kinds of problems to occur.

One example is that if DDL caused any tablespace IDs to change (such as [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table) or [RENAME TABLE](/sql-statements-structure/sql-statements/data-definition/rename-table)), then that could cause the effected tables to be inconsistent in the backup. In this scenario, you might see errors about mismatched tablespace IDs when the backup is prepared.

For example, the errors might look like this:

```sql
2018-12-07 07:49:32 7f51b3184820  InnoDB: Error: table 'DB1/TAB_TEMP'
InnoDB: in InnoDB data dictionary has tablespace id 1355633,
InnoDB: but a tablespace with that id does not exist. There is
InnoDB: a tablespace of name DB1/TAB_TEMP and id 1354713, though. Have
InnoDB: you deleted or moved .ibd files?
InnoDB: Please refer to
InnoDB: http://dev.mysql.com/doc/refman/5.6/en/innodb-troubleshooting-datadict.html
InnoDB: for how to resolve the issue.
```

Or they might look like this:

```sql
2018-07-12 21:24:14 139666981324672 [Note] InnoDB: Ignoring data file 'db1/tab1.ibd' with space ID 200485, since the redo log references db1/tab1.ibd with space ID 200484.
```

Some of the problems related to concurrent DDL are described below.

Problems solved by setting <a undefined>--lock-ddl-per-table</a> (Mariabackup command-line option added in [MariaDB 10.2.9](/kb/en/mariadb-1029-release-notes/)):

- If a table is dropped during the backup, then it might still exists after the backup is prepared.
- If a table exists when the backup starts, but it is dropped before the backup copies it, then the tablespace file can't be copied, so the backup would fail.

Problems solved by setting <a undefined>innodb_log_optimize_ddl=OFF</a> (MariaDB Server system variable added in [MariaDB 10.2.17](/kb/en/mariadb-10217-release-notes/):

- If the backup noticed concurrent DDL, then it might fail with "ALTER TABLE or OPTIMIZE TABLE was executed during backup".

Problems solved by <a undefined>innodb_safe_truncate=ON</a> (MariaDB Server system variable in [MariaDB 10.2.19](/kb/en/mariadb-10219-release-notes/)):

- If a table is created during the backup, then it might not exist in the backup after prepare.
- If a table is renamed during the backup after the tablespace file was copied, then the table may not exist after the backup is prepared.
- If a table is dropped and created under the same name during the backup after the tablespace file was copied, then the table will have the wrong tablespace ID when the backup is prepared.

Problems solved by other bug fixes:

- If <a undefined>--lock-ddl-per-table</a> is used and if a table is concurrently being dropped or renamed, then Mariabackup can fail to acquire the MDL lock.

These problems are only fixed in [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, so it is not recommended to execute concurrent DDL when using Mariabackup with [MariaDB 10.1](/kb/en/what-is-mariadb-101/).

See [MDEV-13563](https://jira.mariadb.org/browse/MDEV-13563), [MDEV-13564](https://jira.mariadb.org/browse/MDEV-13564), [MDEV-16809](https://jira.mariadb.org/browse/MDEV-16809), and [MDEV-16791](https://jira.mariadb.org/browse/MDEV-16791) for more information.

### Manual Restore with Pre-existing InnoDB Redo Log files

Prior to [MariaDB 10.2.10](/kb/en/mariadb-10210-release-notes/), Mariabackup users could run into issues if they restored a backup by manually copying the files from the backup into the <a undefined>datadir</a> while the directory still contained pre-existing [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) files. The backup itself did not contain [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) files with the traditional `ib_logfileN` file names, so the pre-existing log files would remain in the <a undefined>datadir</a>. If the server were started with these pre-existing log files, then it could perform crash recovery with them, which could cause the database to become inconsistent or corrupt.

In these MariaDB versions, this problem could be avoided by not restoring the backup by manually copying the files and instead restoring the backup by using Mariabackup and providing the <a undefined>--copy-back</a> option, since Mariabackup deletes pre-existing [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) files from the <a undefined>datadir</a> during the restore process.

In [MariaDB 10.2.10](/kb/en/mariadb-10210-release-notes/) and later, Mariabackup prevents this issue by creating an empty [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) file called <a undefined>ib_logfile0</a> as part of the <a undefined>--prepare</a> stage. That way, if the backup is manually restored, any pre-existing [InnoDB redo log](/kb/en/xtradbinnodb-redo-log/) files would get overwritten by the empty one.

See [MDEV-13311](https://jira.mariadb.org/browse/MDEV-13311) for more information.

### Too Many Open Files

If Mariabackup uses more file descriptors than the system is configured to allow, then users can see errors like the following:

```sql
2019-02-12 09:48:38 7ffff7fdb820  InnoDB: Operating system error number 23 in a file operation.
InnoDB: Error number 23 means 'Too many open files in system'.
InnoDB: Some operating system error numbers are described at
InnoDB: http://dev.mysql.com/doc/refman/5.6/en/operating-system-error-codes.html
InnoDB: Error: could not open single-table tablespace file ./db1/tab1.ibd
InnoDB: We do not continue the crash recovery, because the table may become
InnoDB: corrupt if we cannot apply the log records in the InnoDB log to it.
InnoDB: To fix the problem and start mysqld:
InnoDB: 1) If there is a permission problem in the file and mysqld cannot
InnoDB: open the file, you should modify the permissions.
InnoDB: 2) If the table is not needed, or you can restore it from a backup,
InnoDB: then you can remove the .ibd file, and InnoDB will do a normal
InnoDB: crash recovery and ignore that table.
InnoDB: 3) If the file system or the disk is broken, and you cannot remove
InnoDB: the .ibd file, you can set innodb_force_recovery > 0 in my.cnf
InnoDB: and force InnoDB to continue crash recovery here.
```

Prior to [MariaDB 10.1.39](/kb/en/mariadb-10139-release-notes/), [MariaDB 10.2.24](/kb/en/mariadb-10224-release-notes/), and [MariaDB 10.3.14](/kb/en/mariadb-10314-release-notes/), Mariabackup would actually ignore the error and continue the backup. In some of those cases, Mariabackup would even report a successful completion of the backup to the user. In later versions, Mariabackup will properly throw an error and abort when this error is encountered. See [MDEV-19060](https://jira.mariadb.org/browse/MDEV-19060) for more information.

When this error is encountered, one solution is to explicitly specify a value for the <a undefined>open-files-limit</a> option either on the command line or in one of the supported [server option groups](#server-option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariabackup]
open_files_limit=65535
```

An alternative solution is to set the soft and hard limits for the user account that runs Mariabackup by adding new limits to <a undefined>/etc/security/limits.conf</a>. For example, if Mariabackup is run by the `mysql` user, then you could add lines like the following:

```sql
mysql soft nofile 65535
mysql hard nofile 65535
```

After the system is rebooted, the above configuration should set new open file limits for the `mysql` user, and the user's `ulimit` output should look like the following:

```sql
$ ulimit -Sn
65535
$ ulimit -Hn
65535
```

## Versions

<table><tbody><tr><th>Mariabackup/Server Version</th><th>Maturity</th></tr>
<tr><td><a href="/kb/en/mariadb-10210-release-notes/">MariaDB 10.2.10</a>+, <a href="/kb/en/mariadb-10126-release-notes/">MariaDB 10.1.26</a>+</td><td>Stable</td></tr>
<tr><td><a href="/kb/en/mariadb-1027-release-notes/">MariaDB 10.2.7</a>+, <a href="/kb/en/mariadb-10125-release-notes/">MariaDB 10.1.25</a></td><td>Beta</td></tr>
<tr><td><a href="/kb/en/mariadb-10123-release-notes/">MariaDB 10.1.23</a></td><td>Alpha</td></tr>
</tbody></table>

## See Also

- [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump)
- [How to backup with MariaDB](https://www.youtube.com/watch?v=xB4ImmmzXqU) (video)
- [MariaDB Enterprise backup](https://mariadb.com/docs/usage/mariadb-enterprise-backup/). An updated version of mariabackup.
- [Percona Xtrabackup 2.3 documentation](https://www.percona.com/doc/percona-xtrabackup/2.3/index.html/)