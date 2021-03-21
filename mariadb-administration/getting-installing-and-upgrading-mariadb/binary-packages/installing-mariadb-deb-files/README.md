# Installing MariaDB .deb Files

## Installing MariaDB with APT

On Debian, Ubuntu, and other similar Linux distributions, it is highly recommended to install the relevant `.deb` packages from MariaDB's
repository using <a undefined>apt</a>, <a undefined>aptitude</a>, [Ubuntu Software Center](https://help.ubuntu.com/community/UbuntuSoftwareCenter), [Synaptic Package Manager](https://help.ubuntu.com/community/SynapticHowto), or another package
manager.

This page walks you through the simple installation steps using `apt`.

### Adding the MariaDB APT repository

We currently have APT repositories for the following Linux distributions:

- Debian 8 (Stretch)
- Debian 9 (Jessie)
- Debian Unstable (Sid)
- Ubuntu 14.04 LTS (Trusty)
- Ubuntu 16.04 LTS (Xenial)
- Ubuntu 17.10 (Artful)
- Ubuntu 18.04 LTS (Bionic)
- Ubuntu 18.10 (Cosmic)
- Mint 17 (Qiana)
- Mint 17.1 (Rebecca)
- Mint 18 (Sarah)
- Mint 19 (Tara)

#### Using the MariaDB Package Repository Setup Script

If you want to install MariaDB with `apt`, then you can configure `apt` to install from MariaDB Corporation's MariaDB Package Repository by using the [MariaDB Package Repository setup script](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage/).

MariaDB Corporation provides a MariaDB Package Repository for several Linux distributions that use `apt` to manage packages. This repository contains software packages related to MariaDB Server, including the server itself, [clients and utilities](/clients-utilities/), [client libraries](/kb/en/client-libraries/), [plugins](/columns-storage-engines-and-plugins/plugins/), and [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/). The MariaDB Package Repository setup script automatically configures your system to install packages from the MariaDB Package Repository.

To use the script, execute the following command:

```sql
curl -sS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash
```

Note that this script also configures a repository for [MariaDB MaxScale](/mariadb-platform-x3/sample-platform-x3-implementation-for-transactional-and-analytical-workloads/mariadb-enterprise/maxscale/) and a repository for MariaDB Tools, which currently only contains [Percona XtraBackup](/kb/en/percona-xtrabackup/) and its dependencies.

See [MariaDB Package Repository Setup and Usage](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage/) for more information.

#### Using the MariaDB Repository Configuration Tool

If you want to install MariaDB with `apt`, then you can configure `apt` to install from MariaDB Foundation's MariaDB Repository by using the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/).

The MariaDB Foundation provides a MariaDB repository for several Linux distributions that use `apt-get` to manage packages. This repository contains software packages related to MariaDB Server, including the server itself, [clients and utilities](/clients-utilities/), [client libraries](/kb/en/client-libraries/), [plugins](/columns-storage-engines-and-plugins/plugins/), and [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/). The MariaDB Repository Configuration Tool can easily generate the appropriate commands to add the repository for your distribution.

There are several ways to add the repository.

##### Executing add-apt-repository

One way to add an `apt` repository is by using the <a undefined>add-apt-repository</a> command. This command will add the repository configuration to `/etc/apt/sources.list`.

For example, if you wanted to use the repository to install [MariaDB 10.3](/kb/en/what-is-mariadb-103/) on Ubuntu 18.04 LTS (Bionic), then you could use the following commands to add the MariaDB `apt` repository:

```sql
sudo apt-get install software-properties-common
sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://sfo1.mirrors.digitalocean.com/mariadb/repo/10.3/ubuntu bionic main'
```

And then you would have to update the package cache by executing the following command:

```sql
sudo apt update
```

##### Creating a Source List File

Another way to add an `apt` repository is by creating a [source list](http://manpages.ubuntu.com/manpages/bionic/man5/sources.list.5.html) file in `/etc/apt/sources.list.d/`.

For example, if you wanted to use the repository to install [MariaDB 10.3](/kb/en/what-is-mariadb-103/) on Ubuntu 18.04 LTS (Bionic), then you could create the `MariaDB.list` file in `/etc/apt/sources.list.d/`  with the following contents to add the MariaDB `apt` repository:

```sql
# MariaDB 10.3 repository list - created 2019-01-27 09:50 UTC
# http://downloads.mariadb.org/mariadb/repositories/
deb [arch=amd64,arm64,ppc64el] http://sfo1.mirrors.digitalocean.com/mariadb/repo/10.3/ubuntu bionic main
deb-src http://sfo1.mirrors.digitalocean.com/mariadb/repo/10.3/ubuntu bionic main
```

And then you would have to update the package cache by executing the following command:

```sql
sudo apt update
```

##### Using Ubuntu Software Center

Another way to add an `apt` repository is by using [Ubuntu Software Center](https://help.ubuntu.com/community/UbuntuSoftwareCenter).

You can do this by going to the <strong>Software Sources</strong> window. This window can be opened either by navigating to <strong>Edit</strong> &gt; <strong>Software Sources</strong> or by navigating to <strong>System</strong> &gt; <strong>Administration</strong> &gt; <strong>Software Sources</strong>.

Once the <strong>Software Sources</strong> window is open, go to the <strong>Other Software</strong> tab, and click the <strong>Add</strong> button. At that point, you can input the repository information provided by the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/).

See [here](https://help.ubuntu.com/community/UbuntuSoftwareCenter#Managing_software_sources) for more information.

##### Using Synaptic Package Manager

Another way to add an `apt` repository is by using [Synaptic Package Manager](https://help.ubuntu.com/community/SynapticHowto).

You can do this by going to the <strong>Software Sources</strong> window. This window can be opened either by navigating to <strong>System</strong> &gt; <strong>Administrator</strong> &gt; <strong>Software Sources</strong> or by navigating to <strong>Settings</strong> &gt; <strong>Repositories</strong>.

Once the <strong>Software Sources</strong> window is open, go to the <strong>Other Software</strong> tab, and click the <strong>Add</strong> button. At that point, you can input the repository information provided by the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/).

See [here](https://help.ubuntu.com/community/SynapticHowto#Managing_Repositories) for more information.

#### Pinning the MariaDB Repository to a Specific Minor Release

If you wish to pin the `apt` repository to a specific minor release, or if you would like to downgrade to a specific minor release, then
you can create a `apt` repository with the URL hard-coded to that specific minor release.

The MariaDB Foundation archives repositories of old minor releases at the following URL:

- [http://archive.mariadb.org/](http://archive.mariadb.org/)

For example, if you wanted to pin your repository to [MariaDB 10.3.14](/kb/en/mariadb-10314-release-notes/) on Ubuntu 18.04 LTS (Bionic), then you would have to first remove any existing MariaDB repository source list file from `/etc/apt/sources.list.d/`. And then you could use the following commands to add the MariaDB `apt-get` repository:

```sql
sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://archive.mariadb.org/mariadb-10.3.14/repo/ubuntu/ bionic main'
```

And then you would have to update the package cache by executing the following command:

```sql
sudo apt update
```

### Updating the MariaDB APT repository to a New Major Release

MariaDB's `apt` repository can be updated to a new major release. How this is done depends on how you originally configured the repository.

#### Updating the Major Release with the MariaDB Package Repository Setup Script

If you configured `apt` to install from MariaDB Corporation's MariaDB Package Repository by using the [MariaDB Package Repository setup script](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage/), then you can update the major release that the repository uses by running the script again.

#### Updating the Major Release with the MariaDB Repository Configuration Tool

If you configured `apt` to install from MariaDB Foundation's MariaDB Repository by using the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/), then you can update the major release in various ways, depending on how you originally added the repository.

##### Updating a Repository with add-apt-repository

If you added the `apt` repository by using the <a undefined>add-apt-repository</a> command, then you can update the major release that the repository uses by using the the <a undefined>add-apt-repository</a> command again.

First, look for the repository string for the old version in `/etc/apt/sources.list`.

And then, you can remove the repository for the old version by executing the <a undefined>add-apt-repository</a> command and providing the `--remove` option. For example, if you wanted to remove a [MariaDB 10.2](/kb/en/what-is-mariadb-102/) repository, then you could do so by executing something like the following:

```sql
sudo add-apt-repository --remove 'deb [arch=amd64,arm64,ppc64el] http://sfo1.mirrors.digitalocean.com/mariadb/repo/10.2/ubuntu bionic main'
```

After that, you can add the repository for the new version with the <a undefined>add-apt-repository</a> command. For example, if you wanted to use the repository to install [MariaDB 10.3](/kb/en/what-is-mariadb-103/) on Ubuntu 18.04 LTS (Bionic), then you could use the following commands to add the MariaDB `apt` repository:

```sql
sudo apt-get install software-properties-common
sudo add-apt-repository 'deb [arch=amd64,arm64,ppc64el] http://sfo1.mirrors.digitalocean.com/mariadb/repo/10.3/ubuntu bionic main'
```

And then you would have to update the package cache by executing the following command:

```sql
sudo apt update
```

After that, the repository should refer to [MariaDB 10.3](/kb/en/what-is-mariadb-103/).

##### Updating a Source List File

If you added the `apt` repository by creating a [source list](http://manpages.ubuntu.com/manpages/bionic/man5/sources.list.5.html) file in `/etc/apt/sources.list.d/`, then you can update the major release that the repository uses by updating the source list file in-place. For example, if you wanted to change the repository from [MariaDB 10.2](/kb/en/what-is-mariadb-102/) to [MariaDB 10.3](/kb/en/what-is-mariadb-103/), and if the source list file was at `/etc/apt/sources.list.d/MariaDB.list`, then you could execute the following:

```sql
sudo sed -i 's/10.2/10.3/' /etc/apt/sources.list.d/MariaDB.list
```

And then you would have to update the package cache by executing the following command:

```sql
sudo apt update
```

After that, the repository should refer to [MariaDB 10.3](/kb/en/what-is-mariadb-103/).

### Importing the MariaDB GPG Public Key

Before MariaDB can be installed, you also have to import the GPG public key that is used to verify the digital signatures of the packages in our repositories. This allows the `apt` utility to verify the integrity of the packages that it installs.

- Prior to Debian 9 (Stretch), and Debian Unstable (Sid), and Ubuntu 16.04 LTS (Xenial), the id of our GPG public key is `0xcbcb082a1bb943db`. The full key fingerprint is:

```sql
1993 69E5 404B D5FC 7D2F E43B CBCB 082A 1BB9 43DB
```

The <a undefined>apt-key</a> utility can be used to import this key. For example:

```sql
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xcbcb082a1bb943db
```

- Starting with Debian 9 (Stretch), and Debian Unstable (Sid), and Ubuntu 16.04 LTS (Xenial), the id of our GPG public key is `0xF1656F24C74CD1D8`. The full key fingerprint is:

```sql
177F 4010 FE56 CA33 3630  0305 F165 6F24 C74C D1D8
```

The <a undefined>apt-key</a> utility can be used to import this key. For example:

```sql
sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xF1656F24C74CD1D8
```

Starting with Debian 9 (Stretch), the <a undefined>dirmngr</a> package needs to be installed before the GPG public key can be imported. To install it, execute: `sudo apt install dirmngr`

If you are unsure which GPG public key you need, then it is perfectly safe to import both keys.

The command used to import the GPG public key is the same on both Debian and Ubuntu. For example:

```sql
$ sudo apt-key adv --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xcbcb082a1bb943db
Executing: gpg --ignore-time-conflict --no-options --no-default-keyring --secret-keyring /tmp/tmp.ASyOPV87XC --trustdb-name /etc/apt/trustdb.gpg --keyring /etc/apt/trusted.gpg --primary-keyring /etc/apt/trusted.gpg --recv-keys --keyserver hkp://keyserver.ubuntu.com:80 0xcbcb082a1bb943db
gpg: requesting key 1BB943DB from hkp server keyserver.ubuntu.com
gpg: key 1BB943DB: "MariaDB Package Signing Key <package-signing-key@mariadb.org>" imported
gpg: no ultimately trusted keys found
gpg: Total number processed: 1
gpg:               imported: 1
```

Once the GPG public key is imported, you are ready to install packages from the repository.

### Installing MariaDB Packages with APT

After the `apt` repository is configured, you can install MariaDB by executing the <a undefined>apt-get</a> command. The specific command that you would use would depend on which specific packages that you want to install.

#### Installing the Most Common Packages with APT

To Install the most common packages, first you would have to update the package cache by executing the following command:

```sql
sudo apt update
```

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, to Install the most common packages, execute the following command:

```sql
sudo apt-get install mariadb-server galera-4 mariadb-client libmariadb3 mariadb-backup mariadb-common
```

##### MariaDB [10.2](/kb/en/what-is-mariadb-102/) - [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and [MariaDB 10.3](/kb/en/what-is-mariadb-103/), to Install the most common packages, execute the following command:

```sql
sudo apt-get install mariadb-server galera mariadb-client libmariadb3 mariadb-backup mariadb-common
```

##### MariaDB until [10.1](/kb/en/what-is-mariadb-101/)

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, to Install the most common packages, execute the following command:

```sql
sudo apt-get install mariadb-server galera mariadb-client libmysqlclient18 mariadb-backup mariadb-common
```

#### Installing MariaDB Server with APT

To Install MariaDB Server, first you would have to update the package cache by executing the following command:

```sql
sudo apt update
```

Then, execute the following command:

```sql
sudo apt-get install mariadb-server
```

#### Installing MariaDB Galera Cluster with APT

The process to install MariaDB Galera Cluster with the MariaDB `apt-get` repository is practically the same as installing standard MariaDB Server.

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and later, Galera Cluster support has been included in the standard MariaDB Server packages, so you will need to install the `mariadb-server` package, as you normally would.

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, you also need to install the `galera-4` package to obtain the [Galera](/kb/en/galera/) 4 wsrep provider library.

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, you also need to install the `galera-3` package to obtain the [Galera](/kb/en/galera/) 3 wsrep provider library.

To install MariaDB Galera Cluster, first you would have to update the package cache by executing the following command:

```sql
sudo apt update
```

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, to install MariaDB Galera Cluster, you could execute the following command:

```sql
sudo apt-get install mariadb-server mariadb-client galera-4
```

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, to install MariaDB Galera Cluster, you could execute the following command:

```sql
sudo apt-get install mariadb-server mariadb-client galera-3
```

MariaDB Galera Cluster also has a separate package that can be installed on arbitrator nodes. In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, the package is called `galera-arbitrator-4` In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, the package is called `galera-arbitrator-3`. This package should be installed on whatever node you want to serve as the arbitrator. It can either run on a separate server that is not acting as a cluster node, which is the recommended configuration, or it can run on a server that is also acting as an existing cluster node.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, to install the arbitrator package, you could execute the following command:

```sql
sudo apt-get install galera-arbitrator-4
```

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, to install the arbitrator package, you could execute the following command:

```sql
sudo apt-get install galera-arbitrator-3
```

See [MariaDB Galera Cluster](/kb/en/galera/) for more information on MariaDB Galera Cluster.

#### Installing MariaDB Clients and Client Libraries with APT

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, [MariaDB Connector/C](/kb/en/about-mariadb-connector-c/) has been included as the client library.

To Install the clients and client libraries, first you would have to update the package cache by executing the following command:

```sql
sudo apt update
```

Then, in [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, execute the following command:

```sql
sudo apt-get install mariadb-client libmariadb3
```

Or in [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, execute the following command:

```sql
sudo apt-get install mariadb-client libmysqlclient18
```

#### Installing Mariabackup with APT

To install [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/), first you would have to update the package cache by executing the following command:

```sql
sudo apt update
```

Then, execute the following command:

```sql
sudo apt-get install mariadb-backup
```

#### Installing Plugins with APT

Some [plugins](/columns-storage-engines-and-plugins/plugins/) may also need to be installed.

For example, to install the [cracklib_password_check](/columns-storage-engines-and-plugins/plugins/password-validation-plugins/cracklib-password-check-plugin/) password validation plugin, first you would have to update the package cache by executing the following command:

```sql
sudo apt update
```

Then, execute the following command:

```sql
sudo apt-get install mariadb-cracklib-password-check
```

#### Installing Older Versions from the Repository

The MariaDB `apt` repository contains the last few versions of MariaDB. To show what versions are available, use the <a undefined>apt-cache</a> command:

```sql
sudo apt-cache showpkg mariadb-server
```

In the output you will see the available versions.

To install an older version of a package instead of the latest version we just
need to specify the package name, an equal sign, and then the version number.

However, when installing an older version of a package, if `apt-get` has to install dependencies, then it will automatically choose to install the latest versions of those packages. To ensure that all MariaDB packages are on the same version in this scenario, it is necessary to specify them all. Therefore, to install [MariaDB 10.3.14](/kb/en/mariadb-10314-release-notes/) from this `apt` repository, we would do the following:

```sql
sudo apt-get install mariadb-server=10.3.14-1 mariadb-client=10.3.14-1 libmariadb3=10.3.14-1 mariadb-backup=10.3.14-1 mariadb-common=10.3.14-1
```

The rest of the install and setup process is as normal.

## Installing MariaDB with dpkg

While it is not recommended, it is possible to download and install the
`.deb` packages manually. However, it is generally recommended to use a package manager like `apt-get`.

A tarball that contains the `.deb` packages can be downloaded from the following URL:

- [https://downloads.mariadb.com](https://downloads.mariadb.com)

For example, to install the [MariaDB 10.4.8](/kb/en/mariadb-1048-release-notes/) `.deb` packages on Ubuntu 18.04 LTS (Bionic), you could execute the following:

```sql
sudo apt-get update
sudo apt-get install libdbi-perl libdbd-mysql-perl psmisc libaio1 socat
wget https://downloads.mariadb.com/MariaDB/mariadb-10.4.8/repo/ubuntu/mariadb-10.4.8-ubuntu-bionic-amd64-debs.tar
tar -xvf mariadb-10.4.8-ubuntu-bionic-amd64-debs.tar
cd mariadb-10.4.8-ubuntu-bionic-amd64-debs/
sudo dpkg --install ./mariadb-common*.deb \
   ./mysql-common*.deb \
   ./mariadb-client*.deb \
   ./libmariadb3*.deb \
   ./libmysqlclient18*.deb 
sudo dpkg --install ./mariadb-server*.deb \
   ./mariadb-backup*.deb \
   ./galera-4*.deb 
```

## After Installation

After the installation is complete, you can [start MariaDB](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

If you are using [MariaDB Galera Cluster](/kb/en/galera/), then keep in mind that the first node will have to be [bootstrapped](/kb/en/getting-started-with-mariadb-galera-cluster/#bootstrapping-a-new-cluster).

## Installation Issues

##### MariaDB starting with [5.5](/kb/en/what-is-mariadb-55/)

### Upgrading `mariadb-server` and `mariadb-client` packages

As noted in [MDEV-4266](https://jira.mariadb.org/browse/MDEV-4266), the `mariadb-server` and `mariadb-client` packages
have a minor upgrade issue if you use '`apt-get install mariadb-server`' or
'`apt-get install mariadb-client`' to upgrade them instead of the more common
'`apt-get upgrade`'. This is because those two packages depend on
`mariadb-server-5.5` and `mariadb-client-5.5` with no specific version of
those packages. For example, if you have the `mariadb-server` package
installed, version 5.5.29 and you install version 5.5.30 of that package it
will not automatically upgrade the `mariadb-server-5.5` package to version
5.5.30 like you would expect because the 5.5.29 version of that package
satisfies the dependency.

The `mariadb-server` and `mariadb-client` packages are virtual packages,
they only exist to require the installation of the `mariadb-server-5.5` and
`mariadb-client-5.5` packages, respectively. MariaDB will function normally
with a, for example, version 5.5.30 version of the `mariadb-server` package
and a version 5.5.29 version of the `mariadb-server-5.5` package. No data is
at risk. However, expected behavior is for '`apt-get install mariadb-server`'
to upgrade everything to the latest version (if a new version is available), so
this is definitely a bug.

A fix is planned for this bug in a future version of MariaDB. In the mean time,
when upgrading MariaDB, use '`apt-get upgrade`' or
'`apt-get install mariadb-server-5.5`'.

##### MariaDB [5.1](/kb/en/what-is-mariadb-51/) - [5.5](/kb/en/what-is-mariadb-55/)

### Version Mismatch Between MariaDB and Ubuntu/Debian Repositories

As mentioned [here](https://lists.launchpad.net/maria-discuss/msg00698.html) (and in [MDEV-4080](https://jira.mariadb.org/browse/MDEV-4080) and [MDEV-3882](https://jira.mariadb.org/browse/MDEV-3882))
sometimes APT will refuse to install MariaDB. Or, if MariaDB is already installed, suggest the removal of MariaDB to apply an upgrade to the
`mysql-common` or `libmysqlclient` packages. This happens whenever the version
number of those two packages is higher in the distribution repositories than
the versions in the MariaDB
repositories. Most MariaDB packages have different names than their MySQL
counterparts, but in order for upgrades from MySQL to MariaDB to be successful
in APT, those two packages must be named the same. Because they have the same
names, APT just checks the version numbers and tries to install what it
considers to be the most recent.

It is rare for the version numbers of `mysql-common` or `libmysqlclient` to be
higher in the official Ubuntu or Debian repositories than they are in the
MariaDB repositories, but it has happened. Whenever it has it has been because of
critical bug fix releases for bugs that existed in the
version of MySQL in the distribution repositories but which had already been
fixed in the version of MariaDB in the MariaDB repositories.

If a situation as described above exists when you try to install MariaDB you
will get an error like this:

```sql
The following packages have unmet dependencies:
 mariadb-server : Depends: mariadb-server-5.5 but it is not going to be installed
E: Unable to correct problems, you have held broken packages.
```

There are three primary ways of fixing this issue:

1 [Pinning the MariaDB Repository](#pinning-the-mariadb-repository)
2 [Specifying Specific Versions to Install](#specifying-specific-versions-to-install)
3 [Holding Packages](#holding-packages)

Click on the links above to jump to directions for each (or simply scroll down).

#### Pinning the MariaDB Repository

It is possible to <em>pin</em> the MariaDB repository used so the packages it provides will always have an higher priority over the ones from the system repositories.

This is done by creating a file with the '`.pref`' extension under '`/etc/apt/preferences.d/`' with the
following contents:

```sql
Package: *
Pin: origin <mirror-domain>
Pin-Priority: 1000
```

Replace '`&lt;mirror-domain&gt;`' with the domain name of the MariaDB mirror you
use. For example '`ftp.osuosl.org`'.

#### Specifying Specific Versions to Install

A way to fix this is to specify the exact version of the two packages that you
want to install. To do this, first determine the full version numbers of the
affected packages. An easy way to do so is with `'apt-cache show'`:

```sql
apt-cache show mysql-common | grep Version
apt-cache show libmysqlclient18 | grep Version
```

For each of the above you will be given a list of versions. The ones in the
MariaDB repositories will have "mariadb" in the version strings and are
the ones you want. With the version numbers in hand you will be able to install
MariaDB by explicitly specifying the version numbers like so:

```sql
apt-get install mariadb-server-5.5 mariadb-client-5.5 \
  libmysqlclient18=<version-number> \
  mysql-common=<version-number>
```

Replace the two instances of `&lt;version-number&gt;` in the example above with the
actual version number of MariaDB that you want to install.

Even after having installed these specific packages versions, running '`apt-get dist-upgrade`' will still try to upgrade the packages to the highest version available.

#### Holding Packages

After MariaDB is installed, and as long as the version number issue exists, an
'`apt-get dist-upgrade`' will try to remove MariaDB in order to install the
"upgraded" `libmysqlclient` and `mysql-common` packages. To prevent this from
happening you can <em>hold</em> them so that apt doesn't try to upgrade them. To do
so, open a terminal, become root with '`sudo -s`', and then enter the
following:

```sql
echo libmysqlclient18 hold | dpkg --set-selections
echo mysql-common hold | dpkg --set-selections
```

The holds will prevent you from upgrading MariaDB, so when you want to remove
the holds, open a terminal, become root with '`sudo -s`', and then enter the
following:

```sql
echo libmysqlclient18 install | dpkg --set-selections
echo mysql-common install | dpkg --set-selections
```

You will then be able to upgrade MariaDB as normal
(e.g. with '`sudo apt-get update; sudo apt-get upgrade`').

## Available DEB Packages

The available DEB packages depend on the specific MariaDB release series.

### Available DEB Packages in [MariaDB 10.4](/kb/en/what-is-mariadb-104/)

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/), the following DEBs are available:

<table><tbody><tr><th>Package Name</th><th>Description</th></tr>
<tr><td><code>galera-4</code></td><td>The WSREP provider for <a href="/kb/en/galera/">Galera</a> 4.</td></tr>
<tr><td><code>libmariadb3</code></td><td>Dynamic client libraries.</td></tr>
<tr><td><code>libmariadb-dev</code></td><td>Development headers and static libraries.</td></tr>
<tr><td><code>libmariadbclient18</code></td><td>Virtual package to satisfy external depends</td></tr>
<tr><td><code>libmysqlclient18</code></td><td>Virtual package to satisfy external depends</td></tr>
<tr><td><code>mariadb-backup</code></td><td><a href="/kb/en/mariabackup/">Mariabackup</a></td></tr>
<tr><td><code>mariadb-client</code></td><td>Client tools like <code class="fixed" style="white-space:pre-wrap">mysql</code> CLI, <code class="fixed" style="white-space:pre-wrap">mysqldump</code>, and others.</td></tr>
<tr><td><code>mariadb-client-core</code></td><td>Core client tools</td></tr>
<tr><td><code>mariadb-common</code></td><td>Character set files and <code class="fixed" style="white-space:pre-wrap">/etc/my.cnf</code></td></tr>
<tr><td><code>mariadb-plugin-connect</code></td><td>The <code><a href="/kb/en/connect/">CONNECT</a></code> storage engine.</td></tr>
<tr><td><code>mariadb-plugin-cracklib-password-check</code></td><td>The <code><a href="/kb/en/cracklib-password-check-plugin/">cracklib_password_check</a></code> password validation plugin.</td></tr>
<tr><td><code>mariadb-plugin-gssapi-client</code></td><td>The client-side component of the <code><a href="/kb/en/authentication-plugin-gssapi/">gssapi</a></code> authentication plugin.</td></tr>
<tr><td><code>mariadb-plugin-gssapi-server</code></td><td>The server-side component of the <code><a href="/kb/en/authentication-plugin-gssapi/">gssapi</a></code> authentication plugin.</td></tr>
<tr><td><code>mariadb-plugin-rocksdb</code></td><td>The <a href="/kb/en/myrocks/">MyRocks</a> storage engine.</td></tr>
<tr><td><code>mariadb-plugin-spider</code></td><td>The <code><a href="/kb/en/spider/">SPIDER</a></code> storage engine.</td></tr>
<tr><td><code>mariadb-plugin-tokudb</code></td><td>The <a href="/kb/en/tokudb/">TokuDB</a> storage engine.</td></tr>
<tr><td><code>mariadb-server</code></td><td>The server and server tools, like <a href="/kb/en/myisamchk/">myisamchk</a> and <a href="/kb/en/mysqlhotcopy/">mysqlhotcopy</a> are here.</td></tr>
<tr><td><code>mariadb-server-core</code></td><td>The core server.</td></tr>
<tr><td><code>mariadb-test</code></td><td><code class="fixed" style="white-space:pre-wrap">mysql-client-test</code> executable, and <strong>mysql-test</strong> framework with the tests.</td></tr>
<tr><td><code>mariadb-test-data</code></td><td>MariaDB database regression test suite - data files</td></tr>
</tbody></table>

### Available DEB Packages in [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and [MariaDB 10.3](/kb/en/what-is-mariadb-103/)

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and [MariaDB 10.3](/kb/en/what-is-mariadb-103/), the following DEBs are available:

<table><tbody><tr><th>Package Name</th><th>Description</th></tr>
<tr><td><code>galera</code></td><td>The WSREP provider for <a href="/kb/en/galera/">Galera</a> 3.</td></tr>
<tr><td><code>libmariadb3</code></td><td>Dynamic client libraries.</td></tr>
<tr><td><code>libmariadb-dev</code></td><td>Development headers and static libraries.</td></tr>
<tr><td><code>libmariadbclient18</code></td><td>Virtual package to satisfy external depends</td></tr>
<tr><td><code>libmysqlclient18</code></td><td>Virtual package to satisfy external depends</td></tr>
<tr><td><code>mariadb-backup</code></td><td><a href="/kb/en/mariabackup/">Mariabackup</a></td></tr>
<tr><td><code>mariadb-client</code></td><td>Client tools like <code class="fixed" style="white-space:pre-wrap">mysql</code> CLI, <code class="fixed" style="white-space:pre-wrap">mysqldump</code>, and others.</td></tr>
<tr><td><code>mariadb-client-core</code></td><td>Core client tools</td></tr>
<tr><td><code>mariadb-common</code></td><td>Character set files and <code class="fixed" style="white-space:pre-wrap">/etc/my.cnf</code></td></tr>
<tr><td><code>mariadb-plugin-connect</code></td><td>The <code><a href="/kb/en/connect/">CONNECT</a></code> storage engine.</td></tr>
<tr><td><code>mariadb-plugin-cracklib-password-check</code></td><td>The <code><a href="/kb/en/cracklib-password-check-plugin/">cracklib_password_check</a></code> password validation plugin.</td></tr>
<tr><td><code>mariadb-plugin-gssapi-client</code></td><td>The client-side component of the <code><a href="/kb/en/authentication-plugin-gssapi/">gssapi</a></code> authentication plugin.</td></tr>
<tr><td><code>mariadb-plugin-gssapi-server</code></td><td>The server-side component of the <code><a href="/kb/en/authentication-plugin-gssapi/">gssapi</a></code> authentication plugin.</td></tr>
<tr><td><code>mariadb-plugin-rocksdb</code></td><td>The <a href="/kb/en/myrocks/">MyRocks</a> storage engine.</td></tr>
<tr><td><code>mariadb-plugin-spider</code></td><td>The <code><a href="/kb/en/spider/">SPIDER</a></code> storage engine.</td></tr>
<tr><td><code>mariadb-plugin-tokudb</code></td><td>The <a href="/kb/en/tokudb/">TokuDB</a> storage engine.</td></tr>
<tr><td><code>mariadb-server</code></td><td>The server and server tools, like <a href="/kb/en/myisamchk/">myisamchk</a> and <a href="/kb/en/mysqlhotcopy/">mysqlhotcopy</a> are here.</td></tr>
<tr><td><code>mariadb-server-core</code></td><td>The core server.</td></tr>
<tr><td><code>mariadb-test</code></td><td><code class="fixed" style="white-space:pre-wrap">mysql-client-test</code> executable, and <strong>mysql-test</strong> framework with the tests.</td></tr>
<tr><td><code>mariadb-test-data</code></td><td>MariaDB database regression test suite - data files</td></tr>
</tbody></table>

### Available DEB Packages in [MariaDB 10.1](/kb/en/what-is-mariadb-101/)

##### MariaDB starting with [10.1](/kb/en/what-is-mariadb-101/)

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/), the following DEBs are available:

<table><tbody><tr><th>Package Name</th><th>Description</th></tr>
<tr><td><code>galera</code></td><td>The WSREP provider for <a href="/kb/en/galera/">Galera</a> 3.</td></tr>
<tr><td><code>libmysqlclient18</code></td><td>Dynamic client libraries.</td></tr>
<tr><td><code>mariadb-backup</code></td><td><a href="/kb/en/mariabackup/">Mariabackup</a></td></tr>
<tr><td><code>mariadb-client</code></td><td>Client tools like <code class="fixed" style="white-space:pre-wrap">mysql</code> CLI, <code class="fixed" style="white-space:pre-wrap">mysqldump</code>, and others.</td></tr>
<tr><td><code>mariadb-client-core</code></td><td>Core client tools</td></tr>
<tr><td><code>mariadb-common</code></td><td>Character set files and <code class="fixed" style="white-space:pre-wrap">/etc/my.cnf</code></td></tr>
<tr><td><code>mariadb-plugin-connect</code></td><td>The <code><a href="/kb/en/connect/">CONNECT</a></code> storage engine.</td></tr>
<tr><td><code>mariadb-plugin-cracklib-password-check</code></td><td>The <code><a href="/kb/en/cracklib-password-check-plugin/">cracklib_password_check</a></code> password validation plugin.</td></tr>
<tr><td><code>mariadb-plugin-gssapi-client</code></td><td>The client-side component of the <code><a href="/kb/en/authentication-plugin-gssapi/">gssapi</a></code> authentication plugin.</td></tr>
<tr><td><code>mariadb-plugin-gssapi-server</code></td><td>The server-side component of the <code><a href="/kb/en/authentication-plugin-gssapi/">gssapi</a></code> authentication plugin.</td></tr>
<tr><td><code>mariadb-plugin-spider</code></td><td>The <code><a href="/kb/en/spider/">SPIDER</a></code> storage engine.</td></tr>
<tr><td><code>mariadb-plugin-tokudb</code></td><td>The <a href="/kb/en/tokudb/">TokuDB</a> storage engine.</td></tr>
<tr><td><code>mariadb-server</code></td><td>The server and server tools, like <a href="/kb/en/myisamchk/">myisamchk</a> and <a href="/kb/en/mysqlhotcopy/">mysqlhotcopy</a> are here.</td></tr>
<tr><td><code>mariadb-server-core</code></td><td>The core server.</td></tr>
<tr><td><code>mariadb-test</code></td><td><code class="fixed" style="white-space:pre-wrap">mysql-client-test</code> executable, and <strong>mysql-test</strong> framework with the tests.</td></tr>
<tr><td><code>mariadb-test-data</code></td><td>MariaDB database regression test suite - data files</td></tr>
</tbody></table>

### Available DEB Packages in [MariaDB 5.5](/kb/en/what-is-mariadb-55/)

##### MariaDB starting with [5.5](/kb/en/what-is-mariadb-55/)

In [MariaDB 5.5](/kb/en/what-is-mariadb-55/), the following DEBs are available:

<table><tbody><tr><th>Package Name</th><th>Description</th></tr>
<tr><td><code>libmysqlclient18</code></td><td>Dynamic client libraries.</td></tr>
<tr><td><code>mariadb-client</code></td><td>Client tools like <code class="fixed" style="white-space:pre-wrap">mysql</code> CLI, <code class="fixed" style="white-space:pre-wrap">mysqldump</code>, and others.</td></tr>
<tr><td><code>mariadb-client-core</code></td><td>Core client tools</td></tr>
<tr><td><code>mariadb-common</code></td><td>Character set files and <code class="fixed" style="white-space:pre-wrap">/etc/my.cnf</code></td></tr>
<tr><td><code>mariadb-server</code></td><td>The server and server tools, like <a href="/kb/en/myisamchk/">myisamchk</a> and <a href="/kb/en/mysqlhotcopy/">mysqlhotcopy</a> are here.</td></tr>
<tr><td><code>mariadb-server-core</code></td><td>The core server.</td></tr>
<tr><td><code>mariadb-test</code></td><td><code class="fixed" style="white-space:pre-wrap">mysql-client-test</code> executable, and <strong>mysql-test</strong> framework with the tests.</td></tr>
<tr><td><code>mariadb-test-data</code></td><td>MariaDB database regression test suite - data files</td></tr>
</tbody></table>

## Actions Performed by DEB Packages

### Users and Groups Created

When the `mariadb-server` DEB package is installed, it will create a user and group named `mysql`, if they do not already exist.

## See Also

- [Differences in MariaDB in Debian](/kb/en/differences-in-mariadb-in-debian/)