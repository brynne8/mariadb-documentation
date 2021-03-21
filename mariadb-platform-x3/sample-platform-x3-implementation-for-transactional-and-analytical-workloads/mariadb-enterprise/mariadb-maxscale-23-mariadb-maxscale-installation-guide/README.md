# MariaDB MaxScale Installation Guide

# MariaDB MaxScale Installation Guide

## Normal Installation

Download the MaxScale package from the MariaDB Downloads page:

- [](https://mariadb.com/downloads/mariadb-tx/maxscale)[https://mariadb.com/downloads/mariadb-tx/maxscale](https://mariadb.com/downloads/mariadb-tx/maxscale)

Select your operating system and download either the RPM or the DEB package.

- <p>For RHEL/CentOS variants, use `yum` to install the downloaded RPM</p>
- <p>For SLES, use `zypper`</p>
- <p>For Debian/Ubuntu systems, install the package with `dpkg -i` and run `apt-get install`
  after it to install the dependencies</p>

You can also use
[the MariaDB package repository](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage/)
to install MaxScale by first configuring the repository and then
installing the `maxscale` package via your package manager.

## Install MariaDB MaxScale Using a Tarball

MaxScale can also be installed using a tarball.
That may be required if you are using a Linux distribution for which there
exist no installation package or if you want to install many different
MaxScale versions side by side. For instructions on how to do that, please refer to
[Install MariaDB MaxScale using a Tarball](/kb/en/mariadb-maxscale-23-installing-mariadb-maxscale-using-a-tarball/).

## Building MariaDB MaxScale From Source Code

Alternatively you may download the MariaDB MaxScale source and build your own binaries.
To do this, refer to the separate document
[Building MariaDB MaxScale from Source Code](/kb/en/mariadb-maxscale-23-building-mariadb-maxscale-from-source-code/)

## Configuring MariaDB MaxScale

[The MaxScale Tutorial](/kb/en/maxscale-23-tutorials-setting-up-mariadb-maxscale/) covers the first
steps in configuring your MariaDB MaxScale installation. Follow this tutorial
to learn how to configure and start using MaxScale.

For a detailed list of all configuration parameters, refer to the
[Configuration Guide](/kb/en/maxscale-23-getting-started-mariadb-maxscale-configuration-usage-scenarios/) and the module specific documents
listed in the [Documentation Contents](/kb/en/mariadb-maxscale-23-maxscale-23-contents/#routers).

## Encrypting Passwords

Read the [Encrypting Passwords](/kb/en/maxscale-23-getting-started-mariadb-maxscale-configuration-usage-scenarios/#encrypting-passwords)
section of the configuration guide to set up password encryption for the
configuration file.

## Administration Of MariaDB MaxScale

There are various administration tasks that may be done with MariaDB MaxScale.
Two command line tools are available, `maxctrl` and `maxadmin`, that will interact with a running
MariaDB MaxScale and allow the status of MariaDB MaxScale to be monitored and
give some control of the MariaDB MaxScale functionality.
There are a separate reference guides for the [maxctrl](/kb/en/mariadb-maxscale-23-maxctrl/) and [maxadmin](/kb/en/mariadb-maxscale-23-maxadmin-admin-interface/) utilities.

[The administration tutorial](/kb/en/mariadb-maxscale-23-mariadb-maxscale-administration-tutorial/)
covers the common administration tasks that need to be done with MariaDB MaxScale.