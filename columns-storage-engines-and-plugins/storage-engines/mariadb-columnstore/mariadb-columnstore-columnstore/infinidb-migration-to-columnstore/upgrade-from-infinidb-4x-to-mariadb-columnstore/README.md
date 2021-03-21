# Upgrade from InfiniDB 4.x to MariaDB ColumnStore

## Upgrade from InfiniDB 4.x to MariaDB ColumnStore

## Overview

The columnar disk storage format is unchanged between InfiniDB 4.X and MariaDB ColumnStore allowing for a software upgrade on the same system. This document outlines an approach to perform the upgrade.

If you are wanted to migrate the data from an InfiniDB system to a separate MariaDB ColumnStore system, then please follow the Migration Guide:

[https://mariadb.com/kb/en/mariadb/migrating-from-infinidb-4x-to-mariadb-columnstore](https://mariadb.com/kb/en/mariadb/migrating-from-infinidb-4x-to-mariadb-columnstore)

<strong>NOTE: the MySQL engine type of 'infinidb' is still supported in the MariaDB ColumnStore, so tables created as 'infinidb' will continue to work. But it is recommend that all new tables use be created using the engine type of 'columnstore'. If you do not recreate the tables you will get warnings in the MariaDB server log file that the FRM file is inconsistent and the typecode is incorrect. These warnings can be ignored.</strong>

## Procedure

During the install of the MariaDB Columnstore packages, make sure you following the steps show in the prepare guide, like installing the dependency packages.

[https://mariadb.com/kb/en/mariadb/preparing-for-columnstore-installation/](https://mariadb.com/kb/en/mariadb/preparing-for-columnstore-installation/)

Note: Calpont.xml and my.cnf modifications you manually made are not automatically carried
forward on an upgrade. These modifications will need to be incorporated back into Columnstore.xml and my.cnf once the upgrade has occurred.

The previous configuration file will be saved as /usr/local/Calpont/etc/Calpont.xml.rpmsave.

If you have specified a root database password (which is good practice), then you must configure a .my.cnf file with user credentials for the upgrade process to use. Create a .my.cnf file in the user home directory with 600 file permissions with the following content (updating PASSWORD as appropriate):

```sql
[mysqladmin] 
user = root
password = PASSWORD
```

This file can be removed after the upgrade is complete.

### Choosing the type of upgrade

#### Root User Installs

#### Upgrading MariaDB ColumnStore using RPMs

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

- Download the package mariadb-columnstore-1.x.x-1-centos#.x86_64.rpm.tar.gz to the PM1 server where you are installing MariaDB ColumnStore.

- Get a copy of the InfiniDB system and storage configuration, this will be used to configure the MariaDB ColumnStore.

```sql
# cc getsystemnetwork
# cc getstorageconfig
```

- Shutdown the InfiniDB system:

```sql
# cc shutdownsystem y
```

- Unpack the tarball, which will generate a set of RPMs that will reside in the /root/ directory.

```sql
# tar -zxf mariadb-columnstore-1.x.x-1-centos#.x86_64.rpm.tar.gz
```

- Uninstall the InfiniDB RPMs on all servers in the system.

```sql
# rpm -e --nodeps $(rpm -qa | grep '^infinidb')
```

This step should remove the InfiniDB alias's from the /root/.bashrc file. But double check that file and if the alais still exist, edit the file manually to remove them.

- Rename the install directory on all servers

```sql
# mkdir -p /usr/local/mariadb/columnstore
# mv /usr/local/Calpont/* /usr/local/mariadb/columnstore/.
```

- Edit DBRM file on pm1

```sql
# sed -i 's/Calpont/mariadb\/columnstore/' /usr/local/mariadb/columnstore/data1/systemFiles/dbrm/BRM_saves_current
```

- Install the MariaDB ColumnStore RPMs. The MariaDB ColumnStore software will be installed in /usr/local/.

```sql
# rpm -ivh mariadb-columnstore-*1.x.x*rpm
```

- Run postConfigure following the either the Single Server Or Multi Server install guides, based on the type of system being upgraded. Provide the same module and storage configuration settings that were display from the InfiniDB system.

[https://mariadb.com/kb/en/mariadb/installing-and-configuring-mariadb-columnstore/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-getting-started/preparing-and-installing-mariadb-columnstore-10x/installing-and-configuring-mariadb-columnstore)

[https://mariadb.com/kb/en/mariadb/installing-and-configuring-a-multi-server-columnstore-system/](https://mariadb.com/kb/en/mariadb/installing-and-configuring-a-multi-server-columnstore-system/)

```sql
# /usr/local/mariadb/columnstore/bin/postConfigure
```

### Initial download/install of MariaDB ColumnStore binary package

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

- Download the package into the /usr/local directory
-mariadb-columnstore-1.x.x-1.x86_64.bin.tar.gz (Binary 64-BIT)to the
server where you are installing MariaDB ColumnStore.

- Get a copy of the InfiniDB system and storage configuration, this will be used to configure the MariaDB ColumnStore.

```sql
# cc getsystemnetwork
# cc getstorageconfig
```

- Shutdown the InfiniDB system:

```sql
# cc shutdownsystem y
```

- Run pre-uninstall script on all servers

```sql
# /usr/local/mariadb/columnstore/bin/pre-uninstall
```

This step should remove the InfiniDB alias's from the /root/.bashrc file. But double check that file and if the alais still exist, edit the file manually to remove them.

- Rename the install directory on all servers

```sql
# mkdir -p /usr/local/mariadb/columnstore
# mv /usr/local/Calpont/* /usr/local/mariadb/columnstore/.
```

- Edit DBRM file on pm1

```sql
# sed -i 's/Calpont/mariadb\/columnstore/' /usr/local/mariadb/columnstore/data1/systemFiles/dbrm/BRM_saves_current
```

- Unpack the tarball, in the /usr/local/ directory.

```sql
# tar -zxvf -mariadb-columnstore-1.x.x-1.x86_64.bin.tar.gz
```

- Run post-install scripts

```sql
# /usr/local/mariadb/columnstore/bin/post-install
```

- Run postConfigure following the either the Single Server Or Multi Server install guides, based on the type of system being upgraded. Provide the same module and storage configuration settings that were display from the InfiniDB system.

[https://mariadb.com/kb/en/mariadb/installing-and-configuring-mariadb-columnstore/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-getting-started/preparing-and-installing-mariadb-columnstore-10x/installing-and-configuring-mariadb-columnstore)

[https://mariadb.com/kb/en/mariadb/installing-and-configuring-a-multi-server-columnstore-system/](https://mariadb.com/kb/en/mariadb/installing-and-configuring-a-multi-server-columnstore-system/)

```sql
# /usr/local/mariadb/columnstore/bin/postConfigure
```

### Upgrading MariaDB ColumnStore using the DEB package

A DEB upgrade would be done on a system that supports DEBs like Debian or Ubuntu
systems.

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

- Download the package into the /root directory

mariadb-columnstore-1.x.x-1.amd64.deb.tar.gz (DEB 64-BIT) to the server where you are installing MariaDB ColumnStore.

- Get a copy of the InfiniDB system and storage configuration, this will be used to configure the MariaDB ColumnStore.

```sql
# cc getsystemnetwork
# cc getstorageconfig
```

- Shutdown the InfiniDB system:

```sql
# cc shutdownsystem y
```

- Unpack the tarball, which will generate DEBs.

```sql
# tar -zxf mariadb-columnstore-1.x.x-1.amd64.deb.tar.gz
```

- Remove, purge and install all InfiniDB debs

```sql
# cd /root/
# dpkg -r infinidb*deb
# dpkg -P infinidb*deb
```

This step should remove the InfiniDB alias's from the /root/.bashrc file. But double check that file and if the alais still exist, edit the file manually to remove them.

- Rename the install directory on all servers

```sql
# mkdir -p /usr/local/mariadb/columnstore
# mv /usr/local/Calpont/* /usr/local/mariadb/columnstore/.
```

- Edit DBRM file on pm1

```sql
# sed -i 's/Calpont/mariadb\/columnstore/' /usr/local/mariadb/columnstore/data1/systemFiles/dbrm/BRM_saves_current
```

- Install the MariaDB ColumnStore DEBs. The MariaDB ColumnStore software will be installed in /usr/local/.

```sql
# cd /root/
# dpkg -i mariadb-columnstore*.deb
```

- Run postConfigure following the either the Single Server Or Multi Server install guides, based on the type of system being upgraded. Provide the same module and storage configuration settings that were display from the InfiniDB system.

[https://mariadb.com/kb/en/mariadb/installing-and-configuring-mariadb-columnstore/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-getting-started/preparing-and-installing-mariadb-columnstore-10x/installing-and-configuring-mariadb-columnstore)

[https://mariadb.com/kb/en/mariadb/installing-and-configuring-a-multi-server-columnstore-system/](https://mariadb.com/kb/en/mariadb/installing-and-configuring-a-multi-server-columnstore-system/)

```sql
# /usr/local/mariadb/columnstore/bin/postConfigure
```

#### Non-Root User Installs

### Initial download/install of MariaDB ColumnStore binary package

Install MariaDB ColumnStore as user root on the server designated as PM1:

- Download the package into the /home/'non-root-user" directory

mariadb-columnstore-1.x.x-1.x86_64.bin.tar.gz (Binary 64-BIT)to the
server where you are installing MariaDB ColumnStore.

- Get a copy of the InfiniDB system and storage configuration, this will be used to configure the MariaDB ColumnStore.

```sql
# cc getsystemnetwork
# cc getstorageconfig
```

- Shutdown the InfiniDB system:

```sql
# cc shutdownsystem y
```

- Run pre-uninstall script on all servers

```sql
# $HOME/Calpont/bin/pre-uninstall -i /$HOME/Calpont
```

This step should remove the InfiniDB alias's from the /root/.bashrc file. But double check that file and if the alais still exist, edit the file manually to remove them.

- Rename the install directory on all servers

```sql
# mkdir -p $HOME/mariadb/columnstore
# mv $HOME/Calpont/* $HOME/mariadb/columnstore/.
```

- Edit DBRM file on pm1

```sql
# sed -i 's/Calpont/mariadb\/columnstore/' $HOME/mariadb/columnstore/data1/systemFiles/dbrm/BRM_saves_current
```

- Unpack the tarball, which will generate the $HOME/ directory.

```sql
# tar -zxvf -mariadb-columnstore-1.x.x-1.x86_64.bin.tar.gz
```

- Run post-install scripts

```sql
# $HOME/mariadb/columnstore/bin/post-install -i /home/guest/mariadb/columnstore
```

- Run postConfigure following the either the Single Server Or Multi Server install guides, based on the type of system being upgraded. Provide the same module and storage configuration settings that were display from the InfiniDB system.

[https://mariadb.com/kb/en/mariadb/installing-and-configuring-mariadb-columnstore/](/columns-storage-engines-and-plugins/storage-engines/mariadb-columnstore/columnstore-getting-started/preparing-and-installing-mariadb-columnstore-10x/installing-and-configuring-mariadb-columnstore)

[https://mariadb.com/kb/en/mariadb/installing-and-configuring-a-multi-server-columnstore-system/](https://mariadb.com/kb/en/mariadb/installing-and-configuring-a-multi-server-columnstore-system/)

```sql
# $HOME/mariadb/columnstore/bin/postConfigure -i /home/guest/mariadb/columnstore
```