# MariaDB ColumnStore software upgrade 1.0.8 to 1.0.9

## MariaDB ColumnStore software upgrade 1.0.8 to 1.0.9

Note: Columnstore.xml modifications you manually made are not automatically carried
forward on an upgrade. These modifications will need to be incorporated back into
.XML once the upgrade has occurred.

The previous configuration file will be saved as /usr/local/mariadb/columnstore/etc/Columnstore.xml.rpmsave.

If you have specified a root database password (which is good practice), then you must configure a .my.cnf file with user credentials for the upgrade process to use. Create a .my.cnf file in the user home directory with 600 file permissions with the following content (updating PASSWORD as appropriate):

```sql
[mysqladmin] 
user = root
password = PASSWORD
```

### Choosing the type of upgrade

As noted on the Preparing guide, you can installing MariaDB ColumnStore with the use of soft-links. If you have the softlinks be setup at the Data Directory Levels, like mariadb/columnstore/data and mariadb/columnstore/dataX, then your upgrade will happen without any issues.
In the case where you have a softlink at the top directory, like /usr/local/mariadb, you will need to upgrade using the binary package. If you updating using the rpm package and tool, this softlink will be deleted when you perform the upgrade process and the upgrade will fail.

#### Root User Installs

#### Upgrading MariaDB ColumnStore using RPMs

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

<strong> Download the package mariadb-columnstore-1.0.9-1-centos#.x86_64.rpm.tar.gz to the PM1 server where you are installing MariaDB ColumnStore.
</strong> Shutdown the MariaDB ColumnStore system:

```sql
# mcsadmin shutdownsystem y
```

- Unpack the tarball, which will generate a set of RPMs that will reside in the /root/ directory.

`tar -zxf mariadb-columnstore-1.0.9-1-centos#.x86_64.rpm.tar.gz`

- Upgrade the RPMs. The MariaDB ColumnStore software will be installed in /usr/local/.

```sql
rpm -e --nodeps $(rpm -qa | grep '^mariadb-columnstore')
rpm -ivh mariadb-columnstore-*1.0.9*rpm
```

- Run postConfigure using the upgrade option, which will utilize the configuration from
the Columnstore.xml.rpmsave

```sql
# /usr/local/mariadb/columnstore/bin/postConfigure -u
```

For RPM Upgrade, the previous configuration file will be saved as:

/usr/local/mariadb/columnstore/etc/Columnstore.xml.rpmsave

### Initial download/install of MariaDB ColumnStore binary package

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

- Download the package into the /usr/local directory
-mariadb-columnstore-1.0.9-1.x86_64.bin.tar.gz (Binary 64-BIT)to the
server where you are installing MariaDB ColumnStore.

- Shutdown the MariaDB ColumnStore system:

`mcsadmin shutdownsystem y`

- Run pre-uninstall script

`/usr/local/mariadb/columnstore/bin/pre-uninstall`

- Unpack the tarball, in the /usr/local/ directory.

` tar -zxvf -mariadb-columnstore-1.0.9-1.x86_64.bin.tar.gz`

- Run post-install scripts

` /usr/local/mariadb/columnstore/bin/post-install`

- Run postConfigure using the upgrade option, which will utilize the configuration from
the Columnstore.xml,rpmsave

`/usr/local/mariadb/columnstore/bin/postConfigure -u`

### Upgrading MariaDB ColumnStore using the DEB package

A DEB upgrade would be done on a system that supports DEBs like Debian or Ubuntu
systems.

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

- Download the package into the /root directory

```sql
mariadb-columnstore-1.0.9-1.amd64.deb.tar.gz
```

(DEB 64-BIT) to the server where you are installing MariaDB ColumnStore.

- Shutdown the MariaDB ColumnStore system:

```sql
mcsadmin shutdownsystem y
```

- Unpack the tarball, which will generate DEBs.

```sql
tar -zxf mariadb-columnstore-1.0.9-1.amd64.deb.tar.gz
```

- Remove, purge and install all MariaDB ColumnStore debs

```sql
cd /root/
dpkg -r mariadb-columnstore*deb
dpkg -P mariadb-columnstore*deb
```

- Run postConfigure using the upgrade option, which will utilize the
configuration from the Columnstore.xml,rpmsave

```sql
/usr/local/mariadb/columnstore/bin/postConfigure -u
```

#### Non-Root User Installs

### Initial download/install of MariaDB ColumnStore binary package

Upgrade MariaDB ColumnStore as user root on the server designated as PM1:

- Download the package into the /home/'non-root-user" directory

mariadb-columnstore-1.0.9-1.x86_64.bin.tar.gz (Binary 64-BIT)to the
server where you are installing MariaDB ColumnStore.

- Shutdown the MariaDB ColumnStore system:

`mcsadmin shutdownsystem y`

- Run pre-uninstall script

` $HOME/mariadb/columnstore/bin/pre-uninstall -i /home/guest/mariadb/columnstore`

- Unpack the tarball, which will generate the $HOME/ directory.

` tar -zxvf -mariadb-columnstore-1.0.9-1.x86_64.bin.tar.gz`

- Run post-install scripts 
<ol start="1"><li>$HOME/mariadb/columnstore/bin/post-install -i /home/guest/mariadb/columnstore<code>
</code></li></ol>

- Run postConfigure using the upgrade option, which will utilize the configuration from
the Columnstore.xml,rpmsave

` $HOME/mariadb/columnstore/bin/postConfigure -u -i /home/guest/mariadb/columnstore`