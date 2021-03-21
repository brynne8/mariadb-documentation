# MariaDB Package Repository Setup and Usage

MariaDB Corporation provides a convenient shell script to configure access to their MariaDB Package Repositories. It is available at:

- [https://downloads.mariadb.com/MariaDB/mariadb_repo_setup](https://downloads.mariadb.com/MariaDB/mariadb_repo_setup).

The script will will set up 2 different repositories in a single repository configuration file. The repositories are

- MariaDB Repository
- MariaDB MaxScale Repository

The script can be executed in the following way:

```sql
curl -LsS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash
```

For the script to work, the `curl` and `ca-certificates` packages need to be installed on your system. Additionally on Debian and Ubuntu the `apt-transport-https` package needs to be installed. The script will check if these are installed and let you know before it attempts to create the repository configuration on your system.

## Repositories

The script will will set up 2 different repositories in a single repository configuration file.

### MariaDB Repository

The <strong>MariaDB Repository</strong> contains software packages related to MariaDB Server, including the server itself, [clients and utilities](/clients-utilities), [client libraries](/kb/en/client-libraries/), [plugins](/columns-storage-engines-and-plugins/plugins), and [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup).

The binaries in MariaDB Corporation's <strong>MariaDB Repository</strong> are currently identical to the binaries in MariaDB Foundation's MariaDB Repository that is configured with the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/).

By default, the script will configure your system to install from the repository of the latest GA version of MariaDB. That is currently [MariaDB 10.5](/kb/en/what-is-mariadb-105/). If a new major GA release occurs and you would like to upgrade to it, then you will need to either manually edit the repository configuration file to point to the new version, or run the MariaDB Package Repository setup script again.

The script can also configure your system to install from the repository of a different version of MariaDB if you use the <a undefined>--mariadb-server-version</a> option.

If you would not like to configure the <strong>MariaDB Repository</strong> on your system, then you can use the `--skip-server` option to prevent the MariaDB Package Repository setup script from configuring it.

### MariaDB MaxScale Repository

The <strong>MariaDB MaxScale Repository</strong> contains software packages related to [MariaDB MaxScale](/mariadb-platform-x3/sample-platform-x3-implementation-for-transactional-and-analytical-workloads/mariadb-enterprise/maxscale).

By default, the script will configure your system to install from the repository of the <em>latest</em> GA version of MariaDB MaxScale. When a new major GA release occurs, the repository will automatically switch to the new version. If instead you would like to stay on a particular version you will need to manually edit the repository configuration file and change '`latest`' to the version you want (e.g. '`2.5`') or run the MariaDB Package Repository setup script again, specifying the particular version or series you want.

Older versions of the MariaDB Package Repository setup script would configure a specific MariaDB MaxScale series in the repository (i.e. '`2.4`'), so if you used the script in the past to set up your repository and want MariaDB MaxScale to automatically use the latest GA version then change '`2.4`' or '`2.3`' in the repository configuration to '`latest`'. Or download the current version of the script and re-run it to set up the repository again.

The script can configure your system to install from the repository of an older version of MariaDB MaxScale if you use the <a undefined>--mariadb-maxscale-version</a> option. For example, `--mariadb-maxscale-version=2.4` if you want the latest release in the MariaDB MaxScale 2.4.x series.

If you do not want to configure the <strong>MariaDB MaxScale Repository</strong> on your system, then you can use the `--skip-maxscale` option to prevent the MariaDB Package Repository setup script from configuring it.

MariaDB MaxScale is licensed under the [Business Source License 1.1](https://mariadb.com/bsl11/), so it is not entirely free to use for organizations who do not have a subscription with MariaDB Corporation. If you would like more information, see the information at [MariaDB Business Source License (BSL): Frequently Asked Questions](https://mariadb.com/bsl-faq-mariadb/). If you would like to know how much a subscription to use MariaDB MaxScale would cost, see [MariaDB Corporation's subscription pricing](https://mariadb.com/pricing/).

## Supported Distributions

The script supports Linux distributions that are officially supported by MariaDB Corporation's [MariaDB TX subscription](https://mariadb.com/products/mariadb-platform-transactional/). However, a MariaDB TX subscription with MariaDB Corporation is not required to use the MariaDB Package Repository.

The distributions currently supported by the script include:

- Red Hat Enterprise Linux (RHEL) 7 and 8
- CentOS 7 and 8
- Debian 9 (Stretch), and 10 (Buster)
- Ubuntu 16.04 LTS (Xenial), 18.04 LTS (Bionic), and 20.04 (Focal)
- SUSE Linux Enterprise Server (SLES) 12 and 15

To install MariaDB on distributions not supported by the MariaDB Package Repository setup script, please consider using MariaDB Foundation's [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/). Some Linux distributions also include MariaDB [in their own repositories](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/distributions-which-include-mariadb).

## Using the MariaDB Package Repository Setup Script

The script can be executed in the following way:

```sql
curl -LsS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash
```

The script will have to set up package repository configuration files, so it will need to be executed as root.

The script will also install the GPG public keys used to verify the signature of MariaDB software packages. If you want to avoid that, then you can use the `--skip-key-import` option.

If the script tries to create the repository configuration file and one with that name already exists, then the script will rename the existing file with an extension in the format ".old_[0-9]+", which would make the OS's package manager ignore the file. You can safely remove those files after you have confirmed that the updated repository configuration file works..

If you want to see the repository configuration file that would be created without actually doing so, then you can use the <a undefined>--write-to-stdout</a> option. This also prevents the need to run the script as root,

If you want to download the script, rather than executing it, then you can do so in the following way:

```sql
curl -LO https://downloads.mariadb.com/MariaDB/mariadb_repo_setup
```

### Options

To provide options to the script, you must tell bash to expect them by executing bash with the options `-s --`, like this:

```sql
curl -LsS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash -s -- --help
```

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--help</code></td><td>Display a usage message and exit.</td></tr>
<tr><td><code>--mariadb-server-version=</code>&lt;version&gt;</td><td>Override the default MariaDB Server version. By default, the script will use 'mariadb-10.5'.</td></tr>
<tr><td><code>--mariadb-maxscale-version=</code>&lt;version&gt;</td><td>Override the default MariaDB MaxScale version. By default, the script will use '2.4'.</td></tr>
<tr><td><code>--os-type=</code>&lt;type&gt;</td><td>Override detection of OS type. Acceptable values include <code>debian</code>, <code>ubuntu</code>, <code>rhel</code>, and <code>sles</code>.</td></tr>
<tr><td><code>--os-version=</code>&lt;version&gt;</td><td>Override detection of OS version. Acceptable values depend on the OS type you specify.</td></tr>
<tr><td><code>--skip-key-import</code></td><td>Skip importing GPG signing keys.</td></tr>
<tr><td><code>--skip-server</code></td><td>Skip the 'MariaDB Server' repository.</td></tr>
<tr><td><code>--skip-maxscale</code></td><td>Skip the 'MaxScale' repository.</td></tr>
<tr><td><code>--write-to-stdout</code></td><td>Write output to stdout instead of to the OS's repository configuration file. This will also skip importing GPG public keys and updating the package cache on platforms where that behavior exists.</td></tr>
</tbody></table>

#### `--mariadb-server-version`

By default, the script will configure your system to install from the repository of the latest GA version of MariaDB. That is currently [MariaDB 10.5](/kb/en/what-is-mariadb-105/). If a new major GA release occurs and you would like to upgrade to it, then you will need to either manually edit the repository configuration file to point to the new version, or run the MariaDB Package Repository setup script again.

The script can also configure your system to install from the repository of a different version of MariaDB if you use the `--mariadb-server-version` option.

The string `mariadb-` has to be prepended to the version number. For example, to configure your system to install from the repository of [MariaDB 10.3](/kb/en/what-is-mariadb-103/), that would be:

```sql
curl -LsS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash -s -- --mariadb-server-version="mariadb-10.3"
```

The following MariaDB versions are currently supported:

- `mariadb-10.2`
- `mariadb-10.3`
- `mariadb-10.4`
- `mariadb-10.5`

If you want to pin the repository of a specific minor release, such as [MariaDB 10.3.9](/kb/en/mariadb-1039-release-notes/), then you can also specify the minor release. For example, `mariadb-10.3.9`. This may be helpful if you want to avoid upgrades. However, avoiding upgrades is not recommended, since minor releases can contain important bug fixes and fixes for security vulnerabilities.

#### `--mariadb-maxscale-version`

By default, the script will configure your system to install from the repository of the latest GA version of MariaDB MaxScale. That is currently MariaDB MaxScale 2.4. If a new major GA release occurs and you would like to upgrade to it, then you will need to either manually edit the repository configuration file to point to the new version, or run the MariaDB Package Repository setup script again.

The script can also configure your system to install from the repository of a different version of MariaDB MaxScale if you use the `--mariadb-maxscale-version` option.

For example, to configure your system to install from the repository of MariaDB MaxScale 2.3, that would be:

```sql
curl -LsS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash -s -- --mariadb-maxscale-version="2.3"
```

The following MariaDB MaxScale versions are currently supported:

- MaxScale 1.4
- MaxScale 2.0
- MaxScale 2.1
- MaxScale 2.2
- MaxScale 2.3
- MaxScale 2.4
- MaxScale 2.5

The special identifiers `latest` (for the latest GA release) and `beta`
(for the latest beta release) are also supported. By default the
`mariadb_repo_setup` script uses `latest` as the version.

#### `--os-type` and `--os-version`

If you want to run this script on an unsupported OS that you believe to be
package-compatible with an OS that is supported, then you can use the
`--os-type` and `--os-version` options to override the script's OS
detection. If you use either option, then you must use both options.

The supported values for `--os-type` are:

- `rhel`
- `debian`
- `ubuntu`
- `sles`

If you use a non-supported value, then the script will fail, just as it would
fail if you ran the script on an unsupported OS.

The supported values for `--os-version` are entirely dependent on the OS
type.

For Red Hat Enterprise Linux (RHEL) and CentOS, `7` and `8` are valid
options.

For Debian and Ubuntu, the version must be specified as the codename of the
specific release. For example, Debian 9 must be specified as `stretch`, and
Ubuntu 18.04 must be specified as `bionic`.

These options can be useful if your distribution is a fork of another
distribution. As an example, Linux Mint 8.1 is based on and is fully compatible
with Ubuntu 16.04 LTS (Xenial). Therefore, If you are using Linux Mint 8.1,
then you  can configure your system to install from the repository of Ubuntu
16.04 LTS (Xenial). If you would like to do that, then you can do so by
specifying `--os-type=ubuntu` and `--os-version=xenial` to the MariaDB
Package Repository setup script.

For example, to manually set the `--os-type` and `--os-version` to RHEL 8
you could do:

```sql
curl -LsS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash -s -- --os-type=rhel --os-version=8
```

#### `--write-to-stdout`

The `--write-to-stdout` option will prevent the script from modifying
anything on the system. The repository configuration will not be written to the
repository configuration file. Instead, it will be printed to standard output.
That allows the configuration to be reviewed, redirected elsewhere, consumed by
another script, or used in some other way.

The `--write-to-stdout` option automatically enables `--skip-key-import`.

For example:

```sql
curl -LsS https://downloads.mariadb.com/MariaDB/mariadb_repo_setup | sudo bash -s -- --write-to-stdout
```

### Platform-Specific Behavior

#### Platform-Specific Behavior on RHEL and CentOS

On Red Hat Enterprise Linux (RHEL) and CentOS, the MariaDB Package Repository setup script performs the following tasks:

- Creates a repository configuration file at `/etc/yum.repos.d/mariadb.repo`.
- Imports the GPG public key used to verify the signature of MariaDB software packages with `rpm --import` from `downloads.mariadb.com`.

#### Platform-Specific Behavior on Debian and Ubuntu

On Debian and Ubuntu, the MariaDB Package Repository setup script performs the following tasks:

- Creates a repository configuration file at `/etc/apt/sources.list.d/mariadb.list`.
- Creates a package preferences file at `/etc/apt/preferences.d/mariadb-enterprise.pref`, which gives packages from MariaDB repositories a higher priority than packages from OS and other repositories, which can help avoid conflicts. It looks like the following:

```sql
Package: *
Pin: origin downloads.mariadb.com
Pin-Priority: 1000
```

- Imports the GPG public key used to verify the signature of MariaDB software package with `apt-key` from the `keyserver.ubuntu.com` key server.
- Updates the package cache with package definitions from the MariaDB Package Repository with `apt-get update`.

#### Platform-Specific Behavior on SLES

On SUSE Linux Enterprise Server (SLES), the MariaDB Package Repository setup script performs the following tasks:

- Creates a repository configuration file at `/etc/zypp/repos.d/mariadb.repo`.
- Imports the GPG public key used to verify the signature of MariaDB software packages with `rpm --import` from `downloads.mariadb.com`.

## Installing Packages with the MariaDB Package Repository

After setting up the MariaDB Package Repository, you can install the software packages in the supported repositories.

### Installing Packages on RHEL and CentOS

To install MariaDB on Red Hat Enterprise Linux (RHEL) and CentOS, see the instructions at [Installing MariaDB Packages with YUM](/kb/en/yum/#installing-mariadb-packages-with-yum). For example:

```sql
sudo yum install MariaDB-server MariaDB-client MariaDB-backup
```

To install MariaDB MaxScale on Red Hat Enterprise Linux (RHEL) and CentOS, see the instructions at [MariaDB MaxScale Installation Guide](/mariadb-platform-x3/sample-platform-x3-implementation-for-transactional-and-analytical-workloads/mariadb-enterprise/mariadb-maxscale-23-mariadb-maxscale-installation-guide). For example:

```sql
sudo yum install maxscale
```

### Installing Packages on Debian and Ubuntu

To install MariaDB on Debian and Ubuntu, see the instructions at [Installing MariaDB Packages with APT](/kb/en/installing-mariadb-deb-files/#installing-mariadb-packages-with-apt). For example:

```sql
sudo apt-get install mariadb-server mariadb-client mariadb-backup
```

To install MariaDB MaxScale on Debian and Ubuntu, see the instructions at [MariaDB MaxScale Installation Guide](/mariadb-platform-x3/sample-platform-x3-implementation-for-transactional-and-analytical-workloads/mariadb-enterprise/mariadb-maxscale-23-mariadb-maxscale-installation-guide). For example:

```sql
sudo apt-get install maxscale
```

### Installing Packages on SLES

To install MariaDB on SUSE Linux Enterprise Server (SLES), see the instructions at [Installing MariaDB Packages with ZYpp](/kb/en/installing-mariadb-with-zypper/#installing-mariadb-packages-with-zypp). For example:

```sql
sudo zypper install MariaDB-server MariaDB-client MariaDB-backup
```

To install MariaDB MaxScale on SUSE Linux Enterprise Server (SLES), see the instructions at [MariaDB MaxScale Installation Guide](/mariadb-platform-x3/sample-platform-x3-implementation-for-transactional-and-analytical-workloads/mariadb-enterprise/mariadb-maxscale-23-mariadb-maxscale-installation-guide). For example:

```sql
sudo zypper install maxscale
```