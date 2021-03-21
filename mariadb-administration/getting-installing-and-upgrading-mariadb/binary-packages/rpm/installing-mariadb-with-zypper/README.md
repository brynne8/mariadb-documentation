# Installing MariaDB with zypper

On SLES, OpenSUSE, and other similar Linux distributions, it is highly recommended to install the relevant [RPM packages](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/) from MariaDB's
repository using <a undefined>zypper</a>.

This page walks you through the simple installation steps using `zypper`.

## Adding the MariaDB ZYpp repository

We currently have ZYpp repositories for the following Linux distributions:

- SUSE Linux Enterprise Server (SLES) 12
- SUSE Linux Enterprise Server (SLES) 15
- OpenSUSE 15
- OpenSUSE 42

### Using the MariaDB Package Repository Setup Script

If you want to install MariaDB with `zypper`, then you can configure `zypper` to install from MariaDB Corporation's MariaDB Package Repository by using the [MariaDB Package Repository setup script](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage/).

MariaDB Corporation provides a MariaDB Package Repository for several Linux distributions that use `zypper` to manage packages. This repository contains software packages related to MariaDB Server, including the server itself, [clients and utilities](/clients-utilities/), [client libraries](/kb/en/client-libraries/), [plugins](/columns-storage-engines-and-plugins/plugins/), and [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/). The MariaDB Package Repository setup script automatically configures your system to install packages from the MariaDB Package Repository.

To use the script, execute the following command:

```sql
curl -sS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash
```

Note that this script also configures a repository for [MariaDB MaxScale](/mariadb-platform-x3/sample-platform-x3-implementation-for-transactional-and-analytical-workloads/mariadb-enterprise/maxscale/) and a repository for MariaDB Tools, which currently only contains [Percona XtraBackup](/kb/en/percona-xtrabackup/) and its dependencies.

See [MariaDB Package Repository Setup and Usage](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage/) for more information.

### Using the MariaDB Repository Configuration Tool

If you want to install MariaDB with `zypper`, then you can configure `zypper` to install from MariaDB Foundation's MariaDB Repository by using the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/).

The MariaDB Foundation provides a MariaDB repository for several Linux distributions that use `zypper` to manage packages. This repository contains software packages related to MariaDB Server, including the server itself, [clients and utilities](/clients-utilities/), [client libraries](/kb/en/client-libraries/), [plugins](/columns-storage-engines-and-plugins/plugins/), and [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/). The MariaDB Repository Configuration Tool can easily generate the appropriate commands to add the repository for your distribution.

For example, if you wanted to use the repository to install [MariaDB 10.3](/kb/en/what-is-mariadb-103/) on SLES 15, then you could use the following commands to add the MariaDB `zypper` repository:

```sql
sudo zypper addrepo --gpgcheck --refresh https://yum.mariadb.org/10.3/sles/15/x86_64 mariadb
sudo zypper --gpg-auto-import-keys refresh
```

### Pinning the MariaDB Repository to a Specific Minor Release

If you wish to pin the `zypper` repository to a specific minor release, or if you would like to downgrade to a specific minor release, then
you can create a `zypper` repository with the URL hard-coded to that specific minor release.

The MariaDB Foundation archives repositories of old minor releases at the following URL:

- [http://archive.mariadb.org/](http://archive.mariadb.org/)

So if you can't find the repository of a specific minor release at `yum.mariadb.org`, then it would be a good idea to check the archive.

For example, if you wanted to pin your repository to [MariaDB 10.3.14](/kb/en/mariadb-10314-release-notes/) on SLES 15, then you could use the following commands to add the MariaDB `zypper` repository:

```sql
sudo zypper removerepo mariadb
sudo zypper addrepo --gpgcheck --refresh https://yum.mariadb.org/10.3.14/sles/15/x86_64 mariadb
```

## Updating the MariaDB ZYpp repository to a New Major Release

MariaDB's `zypper` repository can be updated to a new major release. How this is done depends on how you originally configured the repository.

### Updating the Major Release with the MariaDB Package Repository Setup Script

If you configured `zypper` to install from MariaDB Corporation's MariaDB Package Repository by using the [MariaDB Package Repository setup script](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage/), then you can update the major release that the repository uses by running the script again.

### Updating the Major Release with the MariaDB Repository Configuration Tool

If you configured `zypper` to install from MariaDB Foundation's MariaDB Repository by using the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/), then you can update the major release that the repository uses by removing the repository for the old version and adding the repository for the new version.

First, you can remove the repository for the old version by executing the following command:

```sql
sudo zypper removerepo mariadb
```

After that, you can add the repository for the new version. For example, if you wanted to use the repository to install [MariaDB 10.3](/kb/en/what-is-mariadb-103/) on SLES 15, then you could use the following commands to add the MariaDB `zypper` repository:

```sql
sudo zypper addrepo --gpgcheck --refresh https://yum.mariadb.org/10.3/sles/15/x86_64 mariadb
sudo zypper --gpg-auto-import-keys refresh
```

After that, the repository should refer to [MariaDB 10.3](/kb/en/what-is-mariadb-103/).

## Importing the MariaDB GPG Public Key

Before MariaDB can be installed, you also have to import the GPG public key that is used to verify the digital signatures of the packages in our repositories. This allows the the `zypper` and `rpm` utilities to verify the integrity of the packages that they install.

The id of our GPG public key is `0xcbcb082a1bb943db`. The short form of the id
is `0x1BB943DB`. The full key fingerprint is:

```sql
1993 69E5 404B D5FC 7D2F E43B CBCB 082A 1BB9 43DB
```

The <a undefined>rpm</a> utility can be used to import this key. For example:

```sql
sudo rpm --import https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
```

Once the GPG public key is imported, you are ready to install packages from the repository.

## Installing MariaDB Packages with ZYpp

After the `zypper` repository is configured, you can install MariaDB by executing the <a undefined>zypper</a> command. The specific command that you would use would depend on which specific packages that you want to install.

### Installing the Most Common Packages with ZYpp

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, to Install the most common packages, execute the following command:

```sql
sudo zypper install MariaDB-server galera-4 MariaDB-client MariaDB-shared MariaDB-backup MariaDB-common
```

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, to Install the most common packages, execute the following command:

```sql
sudo zypper install MariaDB-server galera MariaDB-client MariaDB-shared MariaDB-backup MariaDB-common
```

### Installing MariaDB Server with ZYpp

To Install MariaDB Server, execute the following command:

```sql
sudo zypper install MariaDB-server
```

### Installing MariaDB Galera Cluster with ZYpp

The process to install MariaDB Galera Cluster with the MariaDB `zypper` repository is practically the same as installing standard MariaDB Server.

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, Galera Cluster support has been included in the standard MariaDB Server packages, so you will need to install the `MariaDB-server` package, as you normally would.

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, you also need to install the `galera-4` package to obtain the [Galera](/kb/en/galera/) 4 wsrep provider library.

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, you also need to install the `galera` package to obtain the [Galera](/kb/en/galera/) 3 wsrep provider library.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, to install MariaDB Galera Cluster, you could execute the following command:

```sql
sudo zypper install MariaDB-server MariaDB-client galera-4
```

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, to install MariaDB Galera Cluster, you could execute the following command:

```sql
sudo zypper install MariaDB-server MariaDB-client galera
```

If you haven't yet imported the MariaDB GPG public key, then `zypper` will prompt you to
import it after it downloads the packages, but before it prompts you to install them.

See [MariaDB Galera Cluster](/kb/en/galera/) for more information on MariaDB Galera Cluster.

### Installing MariaDB Clients and Client Libraries with ZYpp

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, [MariaDB Connector/C](/kb/en/about-mariadb-connector-c/) has been included as the client library. However, the package name for the client library has not been changed.

To Install the clients and client libraries, execute the following command:

```sql
sudo zypper install MariaDB-client MariaDB-shared
```

### Installing Mariabackup with ZYpp

To install [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/), execute the following command:

```sql
sudo zypper install MariaDB-backup
```

### Installing Plugins with ZYpp

Some [plugins](/columns-storage-engines-and-plugins/plugins/) may also need to be installed.

For example, to install the [cracklib_password_check](/columns-storage-engines-and-plugins/plugins/password-validation-plugins/cracklib-password-check-plugin/) password validation plugin, execute the following command:

```sql
sudo zypper install MariaDB-cracklib-password-check
```

### Installing Debug Info Packages with ZYpp

##### MariaDB starting with [5.5.64](/kb/en/mariadb-5564-release-notes/)

The MariaDB `zypper` repository first added <a undefined>debuginfo</a> packages in [MariaDB 5.5.64](/kb/en/mariadb-5564-release-notes/), [MariaDB 10.1.39](/kb/en/mariadb-10139-release-notes/), [MariaDB 10.2.23](/kb/en/mariadb-10223-release-notes/), [MariaDB 10.3.14](/kb/en/mariadb-10314-release-notes/), and [MariaDB 10.4.4](/kb/en/mariadb-1044-release-notes/).

The MariaDB `zypper` repository also contains <a undefined>debuginfo</a> packages. These package may be needed when [debugging a problem](/kb/en/how-to-produce-a-full-stack-trace-for-mysqld/).

#### Installing Debug Info for the Most Common Packages with ZYpp

To install <a undefined>debuginfo</a> for the most common packages, execute the following command:

```sql
sudo zypper install MariaDB-server-debuginfo MariaDB-client-debuginfo MariaDB-shared-debuginfo MariaDB-backup-debuginfo MariaDB-common-debuginfo
```

#### Installing Debug Info for MariaDB Server with ZYpp

To install <a undefined>debuginfo</a> for MariaDB Server, execute the following command:

```sql
sudo zypper install MariaDB-server-debuginfo
```

#### Installing Debug Info for MariaDB Clients and Client Libraries with ZYpp

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, [MariaDB Connector/C](/kb/en/about-mariadb-connector-c/) has been included as the client library. However, the package name for the client library has not been changed.

To install <a undefined>debuginfo</a> for the clients and client libraries, execute the following command:

```sql
sudo zypper install MariaDB-client-debuginfo MariaDB-shared-debuginfo
```

#### Installing Debug Info for Mariabackup with ZYpp

To install <a undefined>debuginfo</a> for [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/), execute the following command:

```sql
sudo zypper install MariaDB-backup-debuginfo
```

#### Installing Debug Info for Plugins with ZYpp

For some [plugins](/columns-storage-engines-and-plugins/plugins/),  <a undefined>debuginfo</a> may also need to be installed.

For example, to install <a undefined>debuginfo</a> for the [cracklib_password_check](/columns-storage-engines-and-plugins/plugins/password-validation-plugins/cracklib-password-check-plugin/) password validation plugin, execute the following command:

```sql
sudo zypper install MariaDB-cracklib-password-check-debuginfo
```

### Installing Older Versions from the Repository

The MariaDB `zypper` repository contains the last few versions of MariaDB. To show what versions are available, use the following command:

```sql
zypper search --details MariaDB-server
```

In the output you will see the available versions.

To install an older version of a package instead of the latest version we just
need to specify the package name, a dash, and then the version number. And we
only need to specify enough of the version number for it to be unique from the
other available versions.

However, when installing an older version of a package, if `zypper` has to install dependencies, then it will automatically choose to install the latest versions of those packages. To ensure that all MariaDB packages are on the same version in this scenario, it is necessary to specify them all.

The packages that the MariaDB-server package depend on are: MariaDB-client,
MariaDB-shared, and MariaDB-common. Therefore, to install [MariaDB 10.3.14](/kb/en/mariadb-10314-release-notes/) from this `zypper`
repository, we would do the following:

```sql
sudo zypper install MariaDB-server-10.3.14 MariaDB-client-10.3.14 MariaDB-shared-10.3.14 MariaDB-backup-10.3.14 MariaDB-common-10.3.14
```

The rest of the install and setup process is as normal.

## After Installation

After the installation is complete, you can [start MariaDB](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

If you are using [MariaDB Galera Cluster](/kb/en/galera/), then keep in mind that the first node will have to be [bootstrapped](/kb/en/getting-started-with-mariadb-galera-cluster/#bootstrapping-a-new-cluster).