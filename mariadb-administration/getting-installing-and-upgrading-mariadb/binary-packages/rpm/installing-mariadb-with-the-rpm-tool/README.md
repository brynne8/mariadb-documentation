# Installing MariaDB With the rpm Tool

This article describes how to download the RPM files and install them using the
`rpm` command.

It is highly recommended to [Install MariaDB with yum](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/yum/) where possible.

Navigate to [http://downloads.mariadb.org](http://downloads.mariadb.org) and choose
the desired database version and then select the RPMs that match your Linux distribution and architecture.

Clicking those links takes you to a local mirror. Choose the rpms
link and download the desired packages. The packages will be similar to the following:

```sql
MariaDB-client-5.2.5-99.el5.x86_64.rpm
MariaDB-debuginfo-5.2.5-99.el5.x86_64.rpm
MariaDB-devel-5.2.5-99.el5.x86_64.rpm
MariaDB-server-5.2.5-99.el5.x86_64.rpm
MariaDB-shared-5.2.5-99.el5.x86_64.rpm
MariaDB-test-5.2.5-99.el5.x86_64.rpm
```

For a standard server installation you will need to download at least
the <em>client</em>, <em>shared</em>, and <em>server</em> RPM files. See [About the MariaDB RPM Files](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/about-the-mariadb-rpm-files/) for more information about what is included in each RPM package.

After downloading the MariaDB RPM files, you might want to check their signatures.  See  [Checking MariaDB RPM Package Signatures](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/checking-mariadb-rpm-package-signatures/) for more information about checking signatures.

```sql
rpm --checksig $(find . -name '*.rpm')
```

Prior to installing MariaDB, be aware that it will conflict with an existing
installation of MySQL. To check whether MySQL is already installed, issue the
command:

```sql
rpm -qa 'mysql*'
```

If necessary, you can remove found MySQL packages before installing MariaDB.

To install MariaDB, use the command:

```sql
rpm -ivh MariaDB-*
```

You should see output such as the following:

```sql
Preparing...                ########################################### [100%]
   1:MariaDB-shared         ########################################### [ 14%]
   2:MariaDB-client         ########################################### [ 29%]
   3:MariaDB-client         ########################################### [ 43%]
   4:MariaDB-debuginfo      ########################################### [ 57%]
   5:MariaDB-devel          ########################################### [ 71%]
   6:MariaDB-server         ########################################### [ 86%]

PLEASE REMEMBER TO SET A PASSWORD FOR THE MariaDB root USER !
To do so, start the server, then issue the following commands:

/usr/bin/mysqladmin -u root password 'new-password'
/usr/bin/mysqladmin -u root -h hostname password 'new-password'

Alternatively you can run:
/usr/bin/mysql_secure_installation

which will also give you the option of removing the test
databases and anonymous user created by default.  This is
strongly recommended for production servers.

See the MySQL manual for more instructions.

Please report any problems with the /usr/bin/mysqlbug script!

The latest information about MariaDB is available at http://www.askmonty.org/.
You can find additional information about the MySQL part at:
http://dev.mysql.com
Support MariaDB development by buying support/new features from
Monty Program Ab. You can contact us about this at sales@askmonty.org.
Alternatively consider joining our community based development effort:
http://askmonty.org/wiki/index.php/MariaDB#How_can_I_participate_in_the_development_of_MariaDB

Starting MySQL....[  OK  ]
Giving mysqld 2 seconds to start
   7:MariaDB-test           ########################################### [100%]
```

Be sure to follow the instructions given in the preceding output and create a
password for the root user either by using mysqladmin or by running the
/usr/bin/mysql_secure_installation script.

Installing the MariaDB RPM files installs the MySQL tools in the `/usr/bin`
directory. You can confirm that MariaDB has been installed by using the MySQL
client program. Issuing the command `mysql` should give you the MariaDB
cursor.

## See Also

- [Installing MariaDB with yum](/kb/en/installing-mariadb-with-yum/)
- [Troubleshooting MariaDB Installs on RedHat/CentOS](/kb/en/troubleshooting-mariadb-installs-on-redhatcentos/)
- [Checking MariaDB RPM Package Signatures](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/checking-mariadb-rpm-package-signatures/)