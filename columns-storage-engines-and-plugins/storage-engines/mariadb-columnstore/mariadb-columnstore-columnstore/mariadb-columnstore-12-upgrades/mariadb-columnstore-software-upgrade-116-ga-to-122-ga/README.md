# MariaDB ColumnStore software upgrade 1.1.6 GA to 1.2.2 GA

## MariaDB ColumnStore software upgrade 1.1.6 GA to 1.2.2 GA

This upgrade also applies to 1.2.0 Alpha to 1.2.2 GA upgrades

### Changes in 1.2.1 and later

#### Non-distributed is the default distribution mode in postConfigure

The default distribution mode has changed from 'distributed' to 'non-distributed'.  During an upgrade, however, the default is to use the distribution mode used in the original installation.  The options '-d' and '-n' can always be used to override the default.

#### Non-root user sudo setup

Root-level permissions are no longer required to install or upgrade ColumnStore for some types of installations.  Installations requiring some level of sudo access, and the instructions, are listed here: [https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-121/#update-sudo-configuration-if-needed-by-root-user](https://mariadb.com/kb/en/library/preparing-for-columnstore-installation-121/#update-sudo-configuration-if-needed-by-root-user)

#### Running the mysql_upgrade script

As part of the upgrade process to 1.2.2, the user is required to run the mysql_upgrade script on all of the following nodes.

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

### Setup

In this section, we will refer to the directory ColumnStore is installed in as &lt;CSROOT&gt;.  If you installed the RPM or DEB package, then your &lt;CSROOT&gt; will be /usr/local.  If you installed it from the tarball, &lt;CSROOT&gt; will be where you unpacked it.

#### Columnstore.xml / my.cnf

Configuration changes made manually are not automatically carried forward during the upgrade.  These modifications will need to be made again manually after the upgrade is complete.

After the upgrade process the configuration files will be saved at:

- &lt;CSROOT&gt;/mariadb/columnstore/etc/Columnstore.xml.rpmsave
- &lt;CSROOT&gt;/mariadb/columnstore/mysql/my.cnf.rpmsave

#### MariaDB root user database password

If you have specified a root user database password (which is good practice), then you must configure a .my.cnf file with user credentials for the upgrade process to use. Create a .my.cnf file in the user home directory with 600 file permissions with the following content (updating PASSWORD as appropriate):

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

<strong> Download the package mariadb-columnstore-1.2.2-1-centos#.x86_64.rpm.tar.gz to the PM1 server where you are installing MariaDB ColumnStore.</strong>

<strong> Shutdown the MariaDB ColumnStore system:</strong>

```sql
# mcsadmin shutdownsystem y
```

- Unpack the tarball, which will generate a set of RPMs that will reside in the /root/ directory.

```sql
# tar -zxf mariadb-columnstore-1.2.2-1-centos#.x86_64.rpm.tar.gz
```

- Uninstall the old packages, then install the new packages. The MariaDB ColumnStore software will be installed in /usr/local/.

```sql
# rpm -e --nodeps $(rpm -qa | grep '^mariadb-columnstore')
# rpm -ivh mariadb-columnstore-*1.2.2*rpm
```

- Run postConfigure using the upgrade option

```sql
# /usr/local/mariadb/columnstore/bin/postConfigure -u
```

- Run the mysql_upgrade script on the nodes documented above for a root user install

[https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-116-ga-to-122-ga/#running-the-mysql_upgrade-script](https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-116-ga-to-122-ga/#running-the-mysql_upgrade-script)

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

[https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-116-ga-to-122-ga/#running-the-mysql_upgrade-script](https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-116-ga-to-122-ga/#running-the-mysql_upgrade-script)

##### Upgrading MariaDB ColumnStore using the binary tarball (distributed mode)

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

- Download the package into the /usr/local directory mariadb-columnstore-1.2.2-1.x86_64.bin.tar.gz

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
# tar -zxvf mariadb-columnstore-1.2.2-1.x86_64.bin.tar.gz
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

[https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-116-ga-to-122-ga/#running-the-mysql_upgrade-script](https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-116-ga-to-122-ga/#running-the-mysql_upgrade-script)

##### Upgrading MariaDB ColumnStore using the DEB tarball (distributed mode)

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

- Download the package into the /root directory mariadb-columnstore-1.2.2-1.amd64.deb.tar.gz

- Shutdown the MariaDB ColumnStore system:

```sql
# mcsadmin shutdownsystem y
```

- Unpack the tarball, which contains DEBs.

```sql
# tar -zxf mariadb-columnstore-1.2.2-1.amd64.deb.tar.gz
```

- Remove and install all MariaDB ColumnStore debs

```sql
# cd /root/
# dpkg -r  $(dpkg --list | grep 'mariadb-columnstore' | awk '{print $2}')
# dpkg --install mariadb-columnstore-*1.2.2-1*deb
```

- Run postConfigure using the upgrade option

```sql
# /usr/local/mariadb/columnstore/bin/postConfigure -u
```

- Run the mysql_upgrade script on the nodes documented above for a root user install

[https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-116-ga-to-122-ga/#running-the-mysql_upgrade-script](https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-116-ga-to-122-ga/#running-the-mysql_upgrade-script)

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

[https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-116-ga-to-122-ga/#running-the-mysql_upgrade-script](https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-116-ga-to-122-ga/#running-the-mysql_upgrade-script)

#### Non-Root User Installs

##### Upgrade MariaDB ColumnStore from the binary tarball without sudo access (non-distributed mode)

This upgrade method applies when root/sudo access is not an option.

The uninstall script for 1.1.6 requires root access to perform some operations.  These operations are the following:

- removing /etc/profile.d/columnstore{Alias,Env}.sh to remove aliases and environment variables from all users.
- running '&lt;CSROOT&gt;/mysql/columnstore/bin/syslogSetup.sh uninstall' to remove ColumnStore from the logging system
- removing the columnstore startup script
- remove /etc/ld.so.conf.d/columnstore.conf to ColumnStore directories from the ld library search path

Because you are upgrading ColumnStore and not uninstalling it, they are not necessary.  If at some point you wish to uninstall it, you (or your sysadmin) will have to perform those operations by hand.

The upgrade instructions:

- Download the binary tarball to the current installation location on all nodes.  See [https://downloads.mariadb.com/ColumnStore/](https://downloads.mariadb.com/ColumnStore/)

- Shutdown the MariaDB ColumnStore system:

```sql
$ mcsadmin shutdownsystem y
```

- Copy Columnstore.xml to Columnstore.xml.rpmsave, and my.cnf to my.cnf.rpmsave

```sql
$ cp <CSROOT>/mariadb/columnstore/etc/Columnstore{.xml,.xml.rpmsave} 
$ cp <CSROOT>/mariadb/columnstore/mysql/my{.cnf,.cnf.rpmsave}
```

- On all nodes, untar the new files in the same location as the old ones

```sql
$ tar zxf columnstore-1.2.2-1.x86_64.bin.tar.gz
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

[https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-116-ga-to-122-ga/#running-the-mysql_upgrade-script](https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-116-ga-to-122-ga/#running-the-mysql_upgrade-script)

##### Upgrade MariaDB ColumnStore from the binary tarball (distributed mode)

Upgrade MariaDB ColumnStore as user USER on the server designated as PM1:

- Download the package into the user's home directory mariadb-columnstore-1.2.2-1.x86_64.bin.tar.gz

- Shutdown the MariaDB ColumnStore system:

```sql
$ mcsadmin shutdownsystem y
```

- Run the pre-uninstall script; this will require sudo access as you are running a script from 1.1.6.

```sql
$ <CSROOT>/mariadb/columnstore/bin/pre-uninstall --installdir=<CSROOT>/mariadb/columnstore
```

- Make the sudo changes as noted at the beginning of this document

- Unpack the tarball in the same place as the original installation

```sql
$ tar -zxvf mariadb-columnstore-1.2.2-1.x86_64.bin.tar.gz
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

[https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-116-ga-to-122-ga/#running-the-mysql_upgrade-script](https://mariadb.com/kb/en/library/mariadb-columnstore-software-upgrade-116-ga-to-122-ga/#running-the-mysql_upgrade-script)