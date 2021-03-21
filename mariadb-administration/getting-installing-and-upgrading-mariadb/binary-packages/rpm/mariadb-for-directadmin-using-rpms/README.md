# MariaDB for DirectAdmin Using RPMs

If you are using DirectAdmin and you encounter any issues with [Installing MariaDB with YUM](/kb/en/installing-mariadb-with-yum/), then the directions below may help. The process is very straightforward.

<strong>Note:</strong> Installing with YUM is preferable to installing the MariaDB RPM packages manually, so only do this if you are having issues such as:

```sql
Starting httpd:
  httpd:
    Syntax error on line 18 of /etc/httpd/conf/httpd.conf:
    Syntax error on line 1 of /etc/httpd/conf/extra/httpd-phpmodules.conf:
      Cannot load /usr/lib/apache/libphp5.so into server:
        libmysqlclient.so.18: cannot open shared object file: No such file or directory
```

Or:

```sql
Starting httpd:
  httpd:
    Syntax error on line 18 of /etc/httpd/conf/httpd.conf:
    Syntax error on line 1 of /etc/httpd/conf/extra/httpd-phpmodules.conf:
      Cannot load /usr/lib/apache/libphp5.so into server:
        /usr/lib/apache/libphp5.so: undefined symbol: client_errors
```

## RPM Installation

To install the RPMs, there is a quick and easy guide to [Installing MariaDB with the RPM Tool](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/installing-mariadb-with-the-rpm-tool). Follow the instructions there.

## Necessary Edits

We do not want DirectAdmin's custombuild to remove/overwrite our MariaDB
installation whenever an update is performed. To rectify this, disable automatic MySQL installation.

Edit `/usr/local/directadmin/custombuild/options.conf`

Change:

```sql
mysql_inst=yes
```

To:

```sql
mysql_inst=no
```

<strong>Note:</strong>
When MariaDB is installed manually (i.e. not using YUM), updates are not
automatic. You will need to update the RPMs yourself.