# Percona XtraBackup Overview

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup) is the recommended backup method to use instead of Percona XtraBackup.

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/), Percona XtraBackup is <strong>not supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and [MariaDB 10.1](/kb/en/what-is-mariadb-101/), Percona XtraBackup is only <strong>partially supported</strong>. See [Percona XtraBackup Overview: Compatibility with MariaDB](/kb/en/percona-xtrabackup-overview/#compatibility-with-mariadb) for more information.

Percona XtraBackup is an open source tool for performing hot backups of MariaDB, MySQL and Percona Server databases. Percona XtraBackup can perform compressed, incremental and streaming backups. It was designed to back up [XtraDB/InnoDB](/columns-storage-engines-and-plugins/storage-engines/innodb) tables but can also back up other [storage engines](/columns-storage-engines-and-plugins/storage-engines).

[Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup) is a fork of Percona XtraBackup designed to work with encrypted and compressed tables and other MariaDB enhancements. There are many bug fixes, such as [MDEV-13807](https://jira.mariadb.org/browse/MDEV-13807), and some unsafe or redundant options have been removed. [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup) is the recommended backup method for MariaDB servers.

## Installing Percona XtraBackup

### Installing with a Package Manager

Percona XtraBackup can also be installed via a package manager on Linux. In order to do so, your system needs to be configured to install from a repository that has it.

You can also configure your package manager to install it from Percona's repository by following the instructions in their documentation:

- [Installing Percona XtraBackup 2.3](https://www.percona.com/doc/percona-xtrabackup/2.3/installation.html)
- [Installing Percona XtraBackup 2.4](https://www.percona.com/doc/percona-xtrabackup/2.4/installation.html)

#### Installing with yum/dnf

On RHEL, CentOS, Fedora, and other similar Linux distributions, it is highly recommended to install the relevant [RPM package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm) from MariaDB's
repository using [yum](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/yum) or <a undefined>dnf</a>. Starting with RHEL 8 and Fedora 22, `yum` has been replaced by `dnf`, which is the next major version of `yum`. However, `yum` commands still work on many systems that use `dnf`. For example, to install Percona XtraBackup 2.3:

```sql
sudo yum install percona-xtrabackup
```

And to install Percona XtraBackup 2.4:

```sql
sudo yum install percona-xtrabackup-24
```

#### Installing with apt-get

On Debian, Ubuntu, and other similar Linux distributions, it is highly recommended to install the relevant [DEB package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) from MariaDB's
repository using <a undefined>apt-get</a>. For example, to install Percona XtraBackup 2.3:

```sql
sudo apt-get install percona-xtrabackup
```

And to install Percona XtraBackup 2.4:

```sql
sudo apt-get install percona-xtrabackup-24
```

#### Installing with zypper

On SLES, OpenSUSE, and other similar Linux distributions, it is highly recommended to install the relevant [RPM package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm) from MariaDB's repository using [zypper](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/installing-mariadb-with-zypper). For example, to install Percona XtraBackup 2.3:

```sql
sudo zypper install percona-xtrabackup
```

And to install Percona XtraBackup 2.4:

```sql
sudo zypper install percona-xtrabackup-24
```

## Using Percona XtraBackup

The command to use `xtrabackup` and the general syntax is:

```sql
xtrabackup <options>
```

or:

```sql
innobackupex <options>
```

### Options

Options supported by Percona XtraBackup can be found on Percona's documentation.

`xtrabackup` options:

- [`xtrabackup` options - Percona XtraBackup 2.3](https://www.percona.com/doc/percona-xtrabackup/2.3/xtrabackup_bin/xbk_option_reference.html)
- [`xtrabackup` options - Percona XtraBackup 2.4](https://www.percona.com/doc/percona-xtrabackup/2.4/xtrabackup_bin/xbk_option_reference.html)

`innobackupex` options:

- [`innobackupex` options - Percona XtraBackup 2.3](https://www.percona.com/doc/percona-xtrabackup/2.3/innobackupex/innobackupex_option_reference.html)
- [`innobackupex` options - Percona XtraBackup 2.4](https://www.percona.com/doc/percona-xtrabackup/2.4/innobackupex/innobackupex_option_reference.html)

### Option Files

In addition to reading options from the command-line, Percona XtraBackup can also read options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).

The following options relate to how MariaDB/MySQL command-line tools handles option files. They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file.</td></tr>
<tr><td><code>--defaults-file=# </code></td><td>Only read default options from the given file #.</td></tr>
<tr><td><code>--defaults-extra-file=# </code></td><td>Read this file after the global files are read.</td></tr>
</tbody></table>

#### Server Option Groups

Percona XtraBackup reads server options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[xtrabackup]</code></td><td>Options read by <a href="/kb/en/mariabackup/">Mariabackup</a> and Percona XtraBackup.</td></tr>
<tr><td><code>[mysqld]</code></td><td>&nbsp;Options read by <code>mysqld</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
</tbody></table>

#### Client Option Groups

Percona XtraBackup reads client options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[xtrabackup]</code></td><td>Options read by <a href="/kb/en/mariabackup/">Mariabackup</a> and Percona XtraBackup.</td></tr>
<tr><td><code>[client]</code></td><td>&nbsp;Options read by all MariaDB and MySQL <a href="/kb/en/clients-utilities/">client programs</a>, which includes both MariaDB and MySQL clients. For example, <code>mysqldump</code>.</td></tr>
</tbody></table>

### Authentication and Privileges

Percona XtraBackup needs to authenticate with the database server when it performs a backup operation (i.e. when the `--backup` option is specified). The user account that performs the backup needs to have the `RELOAD` , `PROCESS`, `LOCK TABLES` and `REPLICATION CLIENT` [global privileges](/kb/en/grant/#global-privileges) on the database server. For example:

```sql
CREATE USER 'xtrabackup'@'localhost' IDENTIFIED BY 'mypassword';
GRANT RELOAD, PROCESS, LOCK TABLES, REPLICATION CLIENT ON *.* TO 'xtrabackup'@'localhost';
```

The user account information can be specified with the `-user` and `--password` command-line options. For example:

```sql
$ xtrabackup --backup \
   --target-dir=/var/mariadb/backup/ \
   --user=xtrabackup --password=mypassword
```

The user account information can also be specified in a supported [client option group](#client-option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[xtrabackup]
user=xtrabackup
password=mypassword
```

Percona XtraBackup does not need to authenticate with the database server when preparing or restoring a backup.

### File System Permissions

Percona XtraBackup has to read MariaDB's files from the file system. Therefore, when you run Percona XtraBackup as a specific operating system user, you should ensure that user account has sufficient permissions to read those files.

If you are using Linux and if you installed MariaDB with a package manager, then MariaDB's files will probably be owned by the `mysql` user and the `mysql` group.

## Compatibility with MariaDB

### Compatibility with [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and Later

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and later, [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup) is the recommended backup method to use instead of Percona XtraBackup.

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and later, Percona XtraBackup is not supported.

This limitation is being tracked by Percona XtraBackup bug [PXB-1550](https://jira.percona.com/browse/PXB-1550). However, it does not appear that there are plans to fix it.

### Compatibility with [MariaDB 10.2](/kb/en/what-is-mariadb-102/)

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup) is the recommended backup method to use instead of Percona XtraBackup.

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/), Percona XtraBackup 2.4 is supported in some cases if [InnoDB page compression](/kb/en/compression/) is not used, and if [data at rest encryption](/kb/en/data-at-rest-encryption/) is not used, and if [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size) is set to `16k`.

However, users should be aware that problems are likely due to the MySQL 5.7 undo log format incompatibility bug that was fixed in [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/) in [MDEV-12289](https://jira.mariadb.org/browse/MDEV-12289). Due to this bug, backups prepared with Percona XtraBackup 2.4 may fail to recover some transactions. Only if you ran the server with the setting [innodb_undo_logs](/kb/en/xtradbinnodb-server-system-variables/#innodb_undo_logs)=1 this would not be a problem. Percona XtraBackup 2.4 may also fail to work entirely with [MariaDB 10.2.19](/kb/en/mariadb-10219-release-notes/) and later if [innodb_safe_truncate=ON](/kb/en/xtradbinnodb-server-system-variables/#innodb_safe_truncate) is set due to changes in the redo log format introduced by [MDEV-14717](https://jira.mariadb.org/browse/MDEV-14717). In that case, you may see the following error:

```sql
InnoDB: Unsupported redo log format. The redo log was created with MariaDB 10.2.19. Please follow the instructions at http://dev.mysql.com/doc/refman/5.7/en/upgrading-downgrading.html
```

### Compatibility with [MariaDB 10.1](/kb/en/what-is-mariadb-101/)

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/), [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup) is the recommended backup method to use instead of Percona XtraBackup.

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/), Percona XtraBackup 2.3 is supported if [InnoDB page compression](/kb/en/compression/) is not used, and if [data at rest encryption](/kb/en/data-at-rest-encryption/) is not used, and if [innodb_page_size](/kb/en/innodb-system-variables/#innodb_page_size) is set to `16k`.

### Compatibility with [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and Before

In [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and before, Percona XtraBackup 2.3 is supported.

## Using Percona XtraBackup for Galera SSTs

The `xtrabackup-v2` SST method uses the [Percona XtraBackup](/kb/en/backup-restore-and-import-clients-percona-xtrabackup/) utility for performing SSTs. See [xtrabackup-v2 SST method](/replication/galera-cluster/state-snapshot-transfers-ssts-in-galera-cluster/xtrabackup-v2-sst-method) for more information.

## See Also

- [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup)
- [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump)
- [Percona XtraBackup documentation](http://www.percona.com/doc/percona-xtrabackup/)
- [Percona JIRA](https://jira.percona.com/secure/Dashboard.jspa)