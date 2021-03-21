# Upgrading MariaDB on Windows

## Minor upgrades

To install a minor upgrade, e.g 10.1.27 on top of existing 10.1.26, with MSI,  just download the 10.1.27 MSI and start it. It will do everything that needs to be done for  minor upgrade automatically - shutdown MariaDB service(s), replace executables and DLLs, and start service(s) again.

The rest of the article is dedicated to *major* upgrades, e.g 10.1.x to 10.2.y.

## General information on upgrade and version coexistence

This section assumes MSI installations.

First, check everything listed in the Incompatibilities section of the article relating to the version you are upgrading, for example, [Upgrading from MariaDB 10.1 to MariaDB 10.2](/mariadb-administration/getting-installing-and-upgrading-mariadb/upgrading/upgrading-from-mariadb-101-to-mariadb-102/), to make sure you are prepared for the upgrade.

MariaDB (and also MySQL) allows different versions of the product to co-exist
on the same machine, as long as these versions are different either in major or
minor version numbers. For example, it is possible to have say [MariaDB 5.1.51](/kb/en/mariadb-5151-release-notes/)
and 5.2.6 to be installed on the same machine.

However only a single instance of 5.2 can exist. If for example 5.2.7 is
installed on a machine where 5.2.6 is already installed, the installer will
just replace 5.2.6 executables with 5.2.7 ones.

Now imagine, that both 5.1 and 5.2 are installed on the same machine and we
want to upgrade the database instance running on 5.1 to the new version. In
this case special tools are requied. Traditionally, [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade/) is used
to accomplish this. On Windows, the
[MySQL
upgrade](http://dev.mysql.com/doc/refman/5.5/en/windows-upgrading.html) is a complicated multiple-step manual process.

Since [MariaDB 5.2.6](/kb/en/mariadb-526-release-notes/), the Windows distribution includes tools that simplify
migration between different versions and also allow migration between MySQL and
MariaDB.

<strong>Note</strong>. Automatic upgrades are only possible for DB instances that run as a
Windows service.

## General recommendations

<strong> Important:</strong> Ignore any statement that tells you to <em>"just uninstall MySQL and install MariaDB"</em>. This does not work on Windows, never has, and never will. Keep your MySQL installed until after the database had been converted.

The following install/upgrade sequence is recommended in case of "major" upgrades, like going from 5.3 to 5.5

- Install new version, while still retaining the old one
- Upgrade services one by one, like described later in the document (e.g with mysql_upgrade_service). It is recommeded to have services cleanly shut down before the upgrade.
- Uninstall old version when previous step is done.

<strong>Note</strong>. This recommendation differs from the procedure on Unixes, where the upgrade sequence is "uninstall old version, install new version"

## Upgrade Wizard

This is a GUI tool that is typically invoked at the end of a MariaDB
installation if upgradable services are found. The UI allows you to select
instances you want to upgrade.

<img src="/kb/en/upgrading-mariadb-on-windows/+image/UpgradeWizard" alt="UpgradeWizard" title="UpgradeWizard">

## mysql_upgrade_service

This is a command line tool that performs upgrades. The tool requires full
administrative privileges (it has to start and stop services).

Example usage:

```sql
  mysql_upgrade_service --service=MySQL
```

`mysql_upgrade_service` accepts a single parameter <span>—</span>
the name of the MySQL or MariaDB service. It performs all the steps to convert
a MariaDB/MySQL instance running as the service to the current version.

## Migration to 64 bit MariaDB from 32 bit

Earlier we said that only single instance of "MariaDB &lt;major&gt;.&lt;minor&gt;" version
can be installed on the same machine. This was almost correct, because MariaDB
MSI installations allow 32 and 64-bit versions to be installed on the same
machine, and in this case it is possible to have two instances of say 5.2
installed at the same time, an x86 one and an x64 one. One can use the x64
Upgrade wizard to upgrade an instance running as a 32-bit process to run as
64-bit.

## Upgrading ZIP-based installations.

Both UpgradeWizard and mysql_upgrade_service can also be used to upgrade
database instances that were installed with the
[ZIP installation](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-windows-zip-packages/).