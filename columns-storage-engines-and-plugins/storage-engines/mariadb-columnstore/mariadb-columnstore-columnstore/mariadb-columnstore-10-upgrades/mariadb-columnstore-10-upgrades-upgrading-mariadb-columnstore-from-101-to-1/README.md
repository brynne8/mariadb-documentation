# Upgrading MariaDB ColumnStore from 1.0.1 to 1.0.2

## MariaDB ColumnStore single server software upgrade from 1.0.1 to 1.0.2

Note: Calpont.xml modifications you manually made are not automatically carried
forward on an upgrade. These modifications will need to be incorporated back into
.XML once the upgrade has occurred.

The previous configuration file will be saved as
/usr/local/MariaDB/Columnstore/etc/Calpont.xml.rpmsave.

In 1.0.2, the configuration file name was changed to Columnstore.xml. Check the 1.0.2 Release Notes on how to perform the upgrade from the 1.0.1 to the 1.0.2. There is a work-around procedure to follow.

### Upgrade workaround

After you have completed the part of the package upgrade process (removing the old package and installing the 1.0.2 package), the following file will exist:
/usr/local/mariadb/columnstore/etc/Calpont.xml.rpmsave
Run the following commands, this is the work-around part that is only required when going from 1.0.0/1.0.1 to 1.0.2

1 cd /usr/local/mariadb/columnstore/etc/
2 cp Calpont.xml.rpmsave Columnstore.xml.rpmsave
3 sed -i 's/Calpont Version/Columnstore Version/g' Columnstore.xml.rpmsave
4 sed -i 's/Calpont.xml/Columnstore.xml/g' Columnstore.xml.rpmsave
5 sed -i 's/&lt;\/Calpont&gt;/&lt;\/Columnstore&gt;/g' Columnstore.xml.rpmsave
Then you can run the install script to upgrade the system:
6 /usr/local/mariadb/columnstore/bin/postConfigure -u

### Choosing the type of upgrade

#### Root User Installs

#### Upgrading MariaDB ColumnStore using RPMs

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

<strong> Download the package mariadb-columnstore-1.0.2-1-centos#.x86_64.rpm.tar.gz to the server where you are installing MariaDB ColumnStore.
</strong> Shutdown the MariaDB ColumnStore system:

```sql
# mcsadmin shutdownsystem y
```

- Unpack the tarball, which will generate a set of RPMs that will reside in the /root/ directory.

`tar -zxf mariadb-columnstore-1.0.2-1-centos#.x86_64.rpm.tar.gz`

- Upgrade the RPMs. The MariaDB ColumnStore software will be installed in /usr/local/.

Note: If another version of MariaDB is currently running, this service
will need to be stopped before running this command. After the
rpm command has been executed, you can restart the service.

```sql
rpm -Uvh mariadb-columnstore*.rpm
```

- Run postConfigure using the upgrade option, which will utilize the configuration from
the Columnstore.xml,rpmsave

```sql
# /usr/local/MariaDB/Columnstore/bin/postConfigure -u
```

For RPM Upgrade, the previous configuration file will be saved as:

/usr/local/MariaDB/Columnstore/etc/Columnstore.xml.rpmsave

### Initial download/install of MariaDB ColumnStore binary package

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

- Download the package into the /usr/local directory
-mariadb-columnstore-release#.x86_64.bin.tar.gz (Binary 64-BIT)to the
server where you are installing MariaDB ColumnStore.

- Shutdown the MariaDB ColumnStore system:

`cc shutdownsystem y`

- Run pre-uninstall script

`/usr/local/MariaDB/Columnstore/bin/pre-uninstall`

- Unpack the tarball, which will generate the /usr/local/ directory.

` tar -zxvf -mariadb-columnstore-release#.x86_64.bin.tar.gz`

- Run post-install scripts

` /usr/local/MariaDB/Columnstore/bin/post-install`

- Run postConfigure using the upgrade option, which will utilize the configuration from
the Columnstore.xml,rpmsave

`/usr/local/MariaDB/Columnstore/bin/postConfigure -u`

### Upgrading MariaDB ColumnStore using the DEB package

A DEB upgrade would be done on a system that supports DEBs like Debian or Ubuntu
systems.

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

- Download the package into the /root directory

```sql
mariadb-columnstore-release#.amd64.deb.tar.gz
```

(DEB 64-BIT) to the server where you are installing MariaDB ColumnStore.

- Shutdown the MariaDB ColumnStore system:

```sql
cc shutdownsystem y
```

- Unpack the tarball, which will generate DEBs.

```sql
tar -zxf mariadb-columnstore-release#.amd64.deb.tar.gz
```

- Remove, purge and install all MariaDB ColumnStore debs

```sql
cd /root/
dpkg -r mariadb-columnstore*deb
dpkg -P mariadb-columnstore*deb
```

- Run postConfigure using the upgrade option, which will utilize the
configuration from the Calpont.xml,rpmsave

```sql
/usr/local/MariaDB/Columnstore/bin/postConfigure -u
```

#### Non-Root User Installs

### Initial download/install of MariaDB ColumnStore binary package

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

- Download the package into the /home/'non-root-user" directory

mariadb-columnstore-release#.x86_64.bin.tar.gz (Binary 64-BIT)to the
server where you are installing MariaDB ColumnStore.

- Shutdown the MariaDB ColumnStore system:

`cc shutdownsystem y`

- Run pre-uninstall script

` $HOME/MariaDB/Columnstore/bin/pre-uninstall`

- Unpack the tarball, which will generate the $HOME/ directory.

` tar -zxvf -mariadb-columnstore-release#.x86_64.bin.tar.gz`

- Run post-install scripts 
<ol start="1"><li>$HOME/MariaDB/Columnstore/bin/post-install<code>
</code></li></ol>

- Run postConfigure using the upgrade option, which will utilize the configuration from
the Columnstore.xml,rpmsave

` $HOME<em>MariaDB/Columnstore/bin/postConfigure -n</em>`