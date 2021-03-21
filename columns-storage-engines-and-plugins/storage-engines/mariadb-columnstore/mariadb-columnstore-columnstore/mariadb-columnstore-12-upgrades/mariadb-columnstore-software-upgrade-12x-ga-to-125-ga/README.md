# MariaDB ColumnStore software upgrade 1.2.x GA to 1.2.5 GA

## MariaDB ColumnStore software upgrade 1.2.x GA to 1.2.5 GA

### Changes in 1.2.1 and later

#### libjemalloc dependency

ColumnStore 1.2.3 onward requires libjemalloc to be installed. For Ubuntu &amp; Debian based distributions this is installed using the package "libjemalloc1" in the standard repositories.

For CentOS the package is in RedHat's EPEL repository:

This does require either root user access or SUDO setup to install:

```sql
sudo yum -y install epel-release
sudo yum install -y jemalloc
```

#### Non-distributed is the default distribution mode in postConfigure

The default distribution mode has changed from 'distributed' to 'non-distributed'.  During an upgrade, however, the default is to use the distribution mode used in the original installation.  The options '-d' and '-n' can always be used to override the default.

#### Non-root user sudo setup

Root-level permissions are no longer required to install or upgrade ColumnStore for most types of installations.  Installations requiring some level of sudo access, and the instructions, are listed here: [https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-121/#update-sudo-configuration-if-needed-by-root-user](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-121/#update-sudo-configuration-if-needed-by-root-user)

#### Running the mysql_upgrade script

As part of the upgrade process to 1.2.5, the user is might be required to run the mysql_upgrade script on all of the following nodes.

1. If the system was upgraded to 1.1.x to 1.2.x. And now it being upgrade to 1.2.5, the mysql_upgrade script will need to be run.
2. If the system was initially installed with 1.2.x and now its being upgraded to 1.2.5, the mysql_upgrade script doesnt need to be run. So this section can be skipped.

- All User Modules on a system configured with separate User and Performance Modules
- All Performance Modules on a system configured with separate User and Performance Modules and Local Query Feature is enabled
- All Performance Modules on a system configured with combined User and Performance Modules

mysql_upgrade should be run once the upgrade has been completed.

This is an example of how it run on a root user install:

```sql
/usr/local/mariadb/columnstore/mysql/bin/mysql_upgrade --defaults-file=/usr/local/mariadb/columnstore/mysql/my.cnf --force
```

This is an example of how it run on a non-root user install, assuming ColumnStore is installed under the user's home directory:

```sql
$HOME/mariadb/columnstore/mysql/bin/mysql_upgrade --defaults-file=$HOME/mariadb/columnstore/mysql/my.cnf --force
```

In addition you should run the upgrade stored procedure below for a major version upgrade.

#### Executing the upgrade stored procedure

1. If the system was upgraded to 1.1.x to 1.2.x. And now it being upgrade to 1.2.5, the upgrade stored procedure will need to be run.
2. If the system was initially installed with 1.2.x and now its being upgraded to 1.2.5, the upgrade stored procedure doesnt need to be run. So this section can be skipped.

This updates the MariaDB FRM files by altering every ColumnStore table with a blank table comment. This will not affect options set using table comments but will erase any table comment the user has manually set.

You only need to execute this as part of a major version upgrade. It is executed using the following query which should be executed by a user which has access to alter every ColumnStore table:

```sql
call columnstore_info.columnstore_upgrade();
```

### Upgrading from 1.2.4 to a later version

Version 1.2.4 had a bug which caused bad metadata to be stored with dictionary columns. After an upgrade from 1.2.4 tables with dictionary columns (CHAR &gt; 8 bytes, VARCHAR &gt; 7, TEXT &amp; BLOB) tables should be recreated and data cloned. This could be done using CREATE TABLE ... LIKE and using INSERT...SELECT.

### Setup

In this section, we will refer to the directory ColumnStore is installed in as &lt;CSROOT&gt;.  If you installed the RPM or DEB package, then your &lt;CSROOT&gt; will be /usr/local.  If you installed it from the tarball, &lt;CSROOT&gt; will be where you unpacked it.

#### Columnstore.xml / my.cnf

Configuration changes made manually are not automatically carried forward during the upgrade.  These modifications will need to be made again manually after the upgrade is complete.

After the upgrade process the configuration files will be saved at:

- &lt;CSROOT&gt;/mariadb/columnstore/etc/Columnstore.xml.rpmsave
- &lt;CSROOT&gt;/mariadb/columnstore/mysql/my.cnf.rpmsave

#### MariaDB root user database password

If you have specified a root user database password (which is good practice), then you must configure a .my.cnf file with user credentials for the upgrade process to use. Create a .my.cnf file in the user home directory, which is /root for root install and /home/'user' for non-root install. Set 600 as the file permissions with the following content (updating PASSWORD as appropriate):

```sql
[mysqladmin]
user = root
password = PASSWORD
```

### Choosing the type of upgrade

Note, softlinks may cause a problem during the upgrade if you use the RPM or DEB packages.  If you have linked a directory above /usr/local/mariadb/columnstore, the softlinks will be deleted and the upgrade will fail.  In that case you will need to upgrade using the binary tarball instead.  If you have only linked the data directories (ie /usr/local/MariaDB/columnstore/data*), the RPM/DEB package upgrade will work.

#### Root User Installs

##### Upgrading MariaDB ColumnStore using the tarball of RPMs (distributed mode)

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

<strong> Download the package mariadb-columnstore-1.2.5-1-centos#.x86_64.rpm.tar.gz to the PM1 server where you are installing MariaDB ColumnStore.</strong>

<strong> Shutdown the MariaDB ColumnStore system:</strong>

```sql
# mcsadmin shutdownsystem y
```

- Unpack the tarball, which will generate a set of RPMs that will reside in the /root/ directory.

```sql
# tar -zxf mariadb-columnstore-1.2.5-1-centos#.x86_64.rpm.tar.gz
```

- Uninstall the old packages, then install the new packages. The MariaDB ColumnStore software will be installed in /usr/local/.

```sql
# rpm -e --nodeps $(rpm -qa | grep '^mariadb-columnstore')
# rpm -ivh mariadb-columnstore-*1.2.5*rpm
```

- Run postConfigure using the upgrade option

```sql
# /usr/local/mariadb/columnstore/bin/postConfigure -u
```

- Run the mysql_upgrade script on the nodes documented above for a root user install

[https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-117-ga-to-124-ga/#running-the-mysql_upgrade-script](https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-117-ga-to-124-ga/#running-the-mysql_upgrade-script)

##### Upgrading MariaDB ColumnStore using RPM Package Repositories (non-distributed mode)

The system can be upgraded when it was previously installed from the Package Repositories. This will need to be run on each module in the system.

Additional information can be found in this document on how to setup and install using the 'yum' package repo command:

[https://mariadb.com/kb/en/library/installing-mariadb-ax-from-the-package-repositories](https://mariadb.com/kb/en/library/installing-mariadb-ax-from-the-package-repositories)

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

<strong> Shutdown the MariaDB ColumnStore system:</strong>

```sql
# mcsadmin shutdownsystem y
```

- Uninstall MariaDB ColumnStore Packages

```sql
# yum remove mariadb-columnstore*
```

- Install MariaDB ColumnStore Packages

```sql
# yum --enablerepo=mariadb-columnstore clean metadata
# yum install mariadb-columnstore*
```

NOTE: On all modules except for PM1, start the columnstore service

```sql
# /usr/local/mariadb/columnstore/bin/columnstore start
```

- Run postConfigure using the upgrade and non-distributed options

```sql
# /usr/local/mariadb/columnstore/bin/postConfigure -u -n
```

- Run the mysql_upgrade script on the nodes documented above for a root user install

[https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-117-ga-to-124-ga/#running-the-mysql_upgrade-script](https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-117-ga-to-124-ga/#running-the-mysql_upgrade-script)

##### Upgrading MariaDB ColumnStore using the binary tarball (distributed mode)

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

- Download the package into the /usr/local directory mariadb-columnstore-1.2.5-1.x86_64.bin.tar.gz

- Shutdown the MariaDB ColumnStore system:

```sql
# mcsadmin shutdownsystem y
```

- Run pre-uninstall script

```sql
# /usr/local/mariadb/columnstore/bin/pre-uninstall
```

- Unpack the tarball in the /usr/local/ directory.

```sql
# tar -zxvf mariadb-columnstore-1.2.5-1.x86_64.bin.tar.gz
```

- Run post-install scripts

```sql
# /usr/local/mariadb/columnstore/bin/post-install
```

- Run postConfigure using the upgrade option

```sql
# /usr/local/mariadb/columnstore/bin/postConfigure -u
```

- Run the mysql_upgrade script on the nodes documented above for a root user install

[https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-117-ga-to-124-ga/#running-the-mysql_upgrade-script](https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-117-ga-to-124-ga/#running-the-mysql_upgrade-script)

##### Upgrading MariaDB ColumnStore using the DEB tarball (distributed mode)

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

- Download the package into the /root directory mariadb-columnstore-1.2.5-1.amd64.deb.tar.gz

- Shutdown the MariaDB ColumnStore system:

```sql
# mcsadmin shutdownsystem y
```

- Unpack the tarball, which contains DEBs.

```sql
# tar -zxf mariadb-columnstore-1.2.5-1.amd64.deb.tar.gz
```

- Remove and install all MariaDB ColumnStore debs

```sql
# cd /root/
# dpkg -r  $(dpkg --list | grep 'mariadb-columnstore' | awk '{print $2}')
# dpkg --install mariadb-columnstore-*1.2.5-1*deb
```

- Run postConfigure using the upgrade option

```sql
# /usr/local/mariadb/columnstore/bin/postConfigure -u
```

- Run the mysql_upgrade script on the nodes documented above for a root user install

[https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-117-ga-to-124-ga/#running-the-mysql_upgrade-script](https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-117-ga-to-124-ga/#running-the-mysql_upgrade-script)

##### Upgrading MariaDB ColumnStore using DEB Package Repositories (non-distributed mode)

The system can be upgraded when it was previously installed from the Package Repositories. This will need to be run on each module in the system

Additional information can be found in this document on how to setup and install using the 'apt-get' package repo command:

[https://mariadb.com/kb/en/library/installing-mariadb-ax-from-the-package-repositories](https://mariadb.com/kb/en/library/installing-mariadb-ax-from-the-package-repositories)

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

<strong> Shutdown the MariaDB ColumnStore system:</strong>

```sql
# mcsadmin shutdownsystem y
```

- Uninstall MariaDB ColumnStore Packages

```sql
# apt-get remove mariadb-columnstore*
```

- Install MariaDB ColumnStore Packages

```sql
# apt-get update
# sudo apt-get install mariadb-columnstore*
```

NOTE: On all modules except for PM1, start the columnstore service

```sql
# /usr/local/mariadb/columnstore/bin/columnstore start
```

- Run postConfigure using the upgrade and non-distributed options

```sql
# /usr/local/mariadb/columnstore/bin/postConfigure -u -n
```

- Run the mysql_upgrade script on the nodes documented above for a root user install

[https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-117-ga-to-124-ga/#running-the-mysql_upgrade-script](https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-117-ga-to-124-ga/#running-the-mysql_upgrade-script)

#### Non-Root User Installs

##### Upgrade MariaDB ColumnStore from the binary tarball (non-distributed mode)

The upgrade instructions:

- Download the binary tarball to the current installation location on all nodes.  See [https://downloads.mariadb.com/ColumnStore/](https://downloads.mariadb.com/ColumnStore/)

- Shutdown the MariaDB ColumnStore system from PM1:

```sql
$ mcsadmin shutdownsystem y
```

- On all nodes, run pre-install, specifying where ColumnStore is installed

```sql
$ <CSROOT>/mariadb/columnstore/bin/pre-install --installdir=<CSROOT>/mariadb/columnstore
```

- On all nodes, untar the new files in the same location as the old ones

```sql
$ tar zxf columnstore-1.2.5-1.x86_64.bin.tar.gz
```

- On all nodes, run post-install, specifying where ColumnStore is installed

```sql
$ <CSROOT>/mariadb/columnstore/bin/post-install --installdir=<CSROOT>/mariadb/columnstore
```

- On all nodes except for PM1, start the columnstore service

```sql
$ <CSROOT>/mariadb/columnstore/bin/columnstore start
```

- On PM1 only, run postConfigure, specifying the upgrade, non-distributed installation mode, and the location of the installation

```sql
$ <CSROOT>/mariadb/columnstore/bin/postConfigure -u -n -i <CSROOT>/mariadb/columnstore
```

- Run the mysql_upgrade script on the nodes documented above for a non-root user install

[https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-117-ga-to-125-ga/#running-the-mysql_upgrade-script](https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-117-ga-to-125-ga/#running-the-mysql_upgrade-script)

##### Upgrade MariaDB ColumnStore from the binary tarball (distributed mode)

Upgrade MariaDB ColumnStore as user USER on the server designated as PM1:

- Download the package into the user's home directory mariadb-columnstore-1.2.5-1.x86_64.bin.tar.gz

- Shutdown the MariaDB ColumnStore system:

```sql
$ mcsadmin shutdownsystem y
```

- Run the pre-uninstall script

```sql
$ <CSROOT>/mariadb/columnstore/bin/pre-uninstall --installdir=<CSROOT>/mariadb/columnstore
```

- Unpack the tarball in the same place as the original installation

```sql
$ tar -zxvf mariadb-columnstore-1.2.5-1.x86_64.bin.tar.gz
```

- Run post-install scripts

```sql
$ <CSROOT>/mariadb/columnstore/bin/post-install --installdir=<CSROOT>/mariadb/columnstore
```

- Run postConfigure using the upgrade option

```sql
$ <CSROOT>/mariadb/columnstore/bin/postConfigure -u -i <CSROOT>/mariadb/columnstore
```

- Run the mysql_upgrade script on the nodes documented above for a non-root user install

[https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-117-ga-to-125-ga/#running-the-mysql_upgrade-script](https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-117-ga-to-125-ga/#running-the-mysql_upgrade-script)