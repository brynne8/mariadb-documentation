# Installing MariaDB with yum/dnf

On RHEL, CentOS, Fedora, and other similar Linux distributions, it is highly recommended to install the relevant [RPM packages](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/) from MariaDB's
repository using <a undefined>yum</a> or <a undefined>dnf</a>. Starting with RHEL 8 and Fedora 22, `yum` has been replaced by `dnf`, which is the next major version of `yum`. However, `yum` commands still work on many systems that use `dnf`.

This page walks you through the simple installation steps using `yum`.

## Adding the MariaDB YUM repository

We currently have YUM repositories for the following Linux distributions:

- Red Hat Enterprise Linux (RHEL) 6
- Red Hat Enterprise Linux (RHEL) 7
- CentOS 6
- CentOS 7
- Fedora 27
- Fedora 28
- Fedora 29

### Using the MariaDB Package Repository Setup Script

If you want to install MariaDB with `yum`, then you can configure `yum` to install from MariaDB Corporation's MariaDB Package Repository by using the [MariaDB Package Repository setup script](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage/).

MariaDB Corporation provides a MariaDB Package Repository for several Linux distributions that use `yum` to manage packages. This repository contains software packages related to MariaDB Server, including the server itself, [clients and utilities](/clients-utilities/), [client libraries](/kb/en/client-libraries/), [plugins](/columns-storage-engines-and-plugins/plugins/), and [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/). The MariaDB Package Repository setup script automatically configures your system to install packages from the MariaDB Package Repository.

To use the script, execute the following command:

```sql
curl -sS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash
```

Note that this script also configures a repository for [MariaDB MaxScale](/mariadb-platform-x3/sample-platform-x3-implementation-for-transactional-and-analytical-workloads/mariadb-enterprise/maxscale/) and a repository for MariaDB Tools, which currently only contains [Percona XtraBackup](/kb/en/percona-xtrabackup/) and its dependencies.

See [MariaDB Package Repository Setup and Usage](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage/) for more information.

### Using the MariaDB Repository Configuration Tool

If you want to install MariaDB with `yum`, then you can configure `yum` to install from MariaDB Foundation's MariaDB Repository by using the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/).

The MariaDB Foundation provides a MariaDB repository for several Linux distributions that use `yum` to manage packages. This repository contains software packages related to MariaDB Server, including the server itself, [clients and utilities](/clients-utilities/), [client libraries](/kb/en/client-libraries/), [plugins](/columns-storage-engines-and-plugins/plugins/), and [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/). The MariaDB Repository Configuration Tool can easily generate the appropriate configuration file to add the repository for your distribution.

Once you have the appropriate repository configuration section for your distribution, add it to a file named `MariaDB.repo` under `/etc/yum.repos.d/`.

For example, if you wanted to use the repository to install [MariaDB 10.3](/kb/en/what-is-mariadb-103/) on CentOS 7, then you could use the following `yum` repository configuration in `/etc/yum.repos.d/MariaDB.repo`:

```sql
[mariadb]
name = MariaDB
baseurl = http://yum.mariadb.org/10.3/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```

The example file above includes a `gpgkey` line to automatically fetch the
GPG public key that is used to verify the digital signatures of the packages in our repositories. This allows the the `yum`, `dnf`, and `rpm` utilities to verify the integrity of the packages that they install.

### Pinning the MariaDB Repository to a Specific Minor Release

If you wish to pin the `yum` repository to a specific minor release, or if you would like to do a `yum downgrade` to a specific minor release, then
you can create a `yum` repository configuration with a `baseurl` option set to that specific minor release.

The MariaDB Foundation archives repositories of old minor releases at the following URL:

- [http://archive.mariadb.org/](http://archive.mariadb.org/)

So if you can't find the repository of a specific minor release at `yum.mariadb.org`, then it would be a good idea to check the archive.

For example, if you wanted to pin your repository to [MariaDB 10.3.14](/kb/en/mariadb-10314-release-notes/) on CentOS 7, then you could use the following `yum` repository configuration in `/etc/yum.repos.d/MariaDB.repo`:

```sql
[mariadb]
name = MariaDB-10.3.14
baseurl=http://yum.mariadb.org/10.3.14/centos7-amd64
# alternative: baseurl=http://archive.mariadb.org/mariadb-10.3.14/yum/centos7-amd64
gpgkey=https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
gpgcheck=1
```

Note that if you change an existing repository configuration, then you need to execute the following:

```sql
sudo yum clean all
```

## Updating the MariaDB YUM repository to a New Major Release

MariaDB's `yum` repository can be updated to a new major release. How this is done depends on how you originally configured the repository.

### Updating the Major Release with the MariaDB Package Repository Setup Script

If you configured `yum` to install from MariaDB Corporation's MariaDB Package Repository by using the [MariaDB Package Repository setup script](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage/), then you can update the major release that the repository uses by running the script again.

### Updating the Major Release with the MariaDB Repository Configuration Tool

If you configured `yum` to install from MariaDB Foundation's MariaDB Repository by using the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/), then you can update the major release that the repository uses by updating the `yum` repository configuration file in-place. For example, if you wanted to change the repository from [MariaDB 10.2](/kb/en/what-is-mariadb-102/) to [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and if the repository configuration file was at `/etc/yum.repos.d/MariaDB.repo`, then you could execute the following:

```sql
sudo sed -i 's/10.2/10.3/' /etc/yum.repos.d/MariaDB.repo
```

After that, the repository should refer to [MariaDB 10.3](/kb/en/what-is-mariadb-103/).

If the `yum` repository is pinned to a specific minor release, then the above `sed` command can result in an invalid repository configuration. In that case, the recommended options are:

- Edit the `MariaDB.repo` repository file manually.
- Or delete the `MariaDB.repo` repository file, and then install the repository of the new version with the more robust [MariaDB Package Repository setup script](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage/).

## Importing the MariaDB GPG Public Key

Before MariaDB can be installed, you also have to import the GPG public key that is used to verify the digital signatures of the packages in our repositories. This allows the `yum`, `dnf` and `rpm` utilities to verify the integrity of the packages that they install.

The id of our GPG public key is `0xcbcb082a1bb943db`. The short form of the id
is `0x1BB943DB`. The full key fingerprint is:

```sql
1993 69E5 404B D5FC 7D2F E43B CBCB 082A 1BB9 43DB
```

`yum` should prompt you to import the GPG public key the first time that you install a package from MariaDB's repository. However, if you like, the <a undefined>rpm</a> utility can be used to manually import this key instead. For example:

```sql
sudo rpm --import https://yum.mariadb.org/RPM-GPG-KEY-MariaDB
```

Once the GPG public key is imported, you are ready to install packages from the repository.

## Installing MariaDB Packages with YUM

After the `yum` repository is configured, you can install MariaDB by executing the <a undefined>yum</a> command. The specific command that you would use would depend on which specific packages that you want to install.

### Installing the Most Common Packages with YUM

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, to Install the most common packages, execute the following command:

```sql
sudo yum install MariaDB-server galera-4 MariaDB-client MariaDB-shared MariaDB-backup MariaDB-common
```

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, to Install the most common packages, execute the following command:

```sql
sudo yum install MariaDB-server galera MariaDB-client MariaDB-shared MariaDB-backup MariaDB-common
```

### Installing MariaDB Server with YUM

To Install MariaDB Server, execute the following command:

```sql
sudo yum install MariaDB-server
```

### Installing MariaDB Galera Cluster with YUM

The process to install MariaDB Galera Cluster with the MariaDB `yum` repository is practically the same as installing standard MariaDB Server.

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, Galera Cluster support has been included in the standard MariaDB Server packages, so you will need to install the `MariaDB-server` package, as you normally would.

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, you also need to install the `galera-4` package to obtain the [Galera](/kb/en/galera/) 4 wsrep provider library.

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, you also need to install the `galera` package to obtain the [Galera](/kb/en/galera/) 3 wsrep provider library.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, to install MariaDB Galera Cluster, you could execute the following command:

```sql
sudo yum install MariaDB-server MariaDB-client galera-4
```

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, to install MariaDB Galera Cluster, you could execute the following command:

```sql
sudo yum install MariaDB-server MariaDB-client galera
```

If you haven't yet imported the MariaDB GPG public key, then `yum` will prompt you to
import it after it downloads the packages, but before it prompts you to install them.

See [MariaDB Galera Cluster](/kb/en/galera/) for more information on MariaDB Galera Cluster.

### Installing MariaDB Clients and Client Libraries with YUM

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, [MariaDB Connector/C](/kb/en/about-mariadb-connector-c/) has been included as the client library. However, the package name for the client library has not been changed.

To Install the clients and client libraries, execute the following command:

```sql
sudo yum install MariaDB-client MariaDB-shared
```

### Installing Mariabackup with YUM

To install [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/), execute the following command:

```sql
sudo yum install MariaDB-backup
```

### Installing Plugins with YUM

Some [plugins](/columns-storage-engines-and-plugins/plugins/) may also need to be installed.

For example, to install the [cracklib_password_check](/columns-storage-engines-and-plugins/plugins/password-validation-plugins/cracklib-password-check-plugin/) password validation plugin, execute the following command:

```sql
sudo yum install MariaDB-cracklib-password-check
```

### Installing Debug Info Packages with YUM

##### MariaDB starting with [5.5.64](/kb/en/mariadb-5564-release-notes/)

The MariaDB `yum` repository first added <a undefined>debuginfo</a> packages in [MariaDB 5.5.64](/kb/en/mariadb-5564-release-notes/), [MariaDB 10.1.39](/kb/en/mariadb-10139-release-notes/), [MariaDB 10.2.23](/kb/en/mariadb-10223-release-notes/), [MariaDB 10.3.14](/kb/en/mariadb-10314-release-notes/), and [MariaDB 10.4.4](/kb/en/mariadb-1044-release-notes/).

The MariaDB `yum` repository also contains <a undefined>debuginfo</a> packages. These package may be needed when [debugging a problem](/kb/en/how-to-produce-a-full-stack-trace-for-mysqld/).

#### Installing Debug Info for the Most Common Packages with YUM

To install <a undefined>debuginfo</a> for the most common packages, execute the following command:

```sql
sudo yum install MariaDB-server-debuginfo MariaDB-client-debuginfo MariaDB-shared-debuginfo MariaDB-backup-debuginfo MariaDB-common-debuginfo
```

#### Installing Debug Info for MariaDB Server with YUM

To install <a undefined>debuginfo</a> for MariaDB Server, execute the following command:

```sql
sudo yum install MariaDB-server-debuginfo
```

#### Installing Debug Info for MariaDB Clients and Client Libraries with YUM

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, [MariaDB Connector/C](/kb/en/about-mariadb-connector-c/) has been included as the client library. However, the package name for the client library has not been changed.

To install <a undefined>debuginfo</a> for the clients and client libraries, execute the following command:

```sql
sudo yum install MariaDB-client-debuginfo MariaDB-shared-debuginfo
```

#### Installing Debug Info for Mariabackup with YUM

To install <a undefined>debuginfo</a> for [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/), execute the following command:

```sql
sudo yum install MariaDB-backup-debuginfo
```

#### Installing Debug Info for Plugins with YUM

For some [plugins](/columns-storage-engines-and-plugins/plugins/),  <a undefined>debuginfo</a> may also need to be installed.

For example, to install <a undefined>debuginfo</a> for the [cracklib_password_check](/columns-storage-engines-and-plugins/plugins/password-validation-plugins/cracklib-password-check-plugin/) password validation plugin, execute the following command:

```sql
sudo yum install MariaDB-cracklib-password-check-debuginfo
```

### Installing Older Versions from the Repository

The MariaDB `yum` repository contains the last few versions of MariaDB. To show what versions are available, use the following command:

```sql
yum list --showduplicates MariaDB-server
```

In the output you will see the available versions. For example:

```sql
$ yum list --showduplicates MariaDB-server
Loaded plugins: fastestmirror
Loading mirror speeds from cached hostfile
 * base: centos.mirrors.ovh.net
 * extras: centos.mirrors.ovh.net
 * updates: centos.mirrors.ovh.net
Available Packages
MariaDB-server.x86_64   10.3.10-1.el7.centos    mariadb
MariaDB-server.x86_64   10.3.11-1.el7.centos    mariadb
MariaDB-server.x86_64   10.3.12-1.el7.centos    mariadb
mariadb-server.x86_64   1:5.5.60-1.el7_5         base   
```

The MariaDB `yum` repository in this example contains [MariaDB 10.3.10](/kb/en/mariadb-10310-release-notes/), [MariaDB 10.3.11](/kb/en/mariadb-10311-release-notes/), and [MariaDB 10.3.12](/kb/en/mariadb-10312-release-notes/). The CentOS base `yum` repository also contains [MariaDB 5.5.60](/kb/en/mariadb-5560-release-notes/).

To install an older version of a package instead of the latest version we just
need to specify the package name, a dash, and then the version number. And we
only need to specify enough of the version number for it to be unique from the
other available versions.

However, when installing an older version of a package, if `yum` has to install dependencies, then it will automatically choose to install the latest versions of those packages. To ensure that all MariaDB packages are on the same version in this scenario, it is necessary to specify them all.

The packages that the MariaDB-server package depend on are: MariaDB-client,
MariaDB-shared, and MariaDB-common. Therefore, to install [MariaDB 10.3.11](/kb/en/mariadb-10311-release-notes/) from this `yum`
repository, we would do the following:

```sql
sudo yum install MariaDB-server-10.3.11 MariaDB-client-10.3.11 MariaDB-shared-10.3.11 MariaDB-backup-10.3.11 MariaDB-common-10.3.11
```

The rest of the install and setup process is as normal.

## After Installation

After the installation is complete, you can [start MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/starting-and-stopping-mariadb-automatically/).

If you are using [MariaDB Galera Cluster](/kb/en/galera/), then keep in mind that the first node will have to be [bootstrapped](/kb/en/getting-started-with-mariadb-galera-cluster/#bootstrapping-a-new-cluster).