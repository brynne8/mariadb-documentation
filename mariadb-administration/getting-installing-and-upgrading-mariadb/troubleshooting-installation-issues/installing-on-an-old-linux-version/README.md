# Installing on an Old Linux Version

This article lists some typical errors that may happen when you try to use an
incompatible MariaDB binary on a linux system:

The following example errors are from trying to install MariaDB built for SuSE
11.x on a SuSE 9.0 server:

```sql
> scripts/mysql_install_db
./bin/my_print_defaults: /lib/i686/libc.so.6: 
  version `GLIBC_2.4' not found (required by ./bin/my_print_defaults)
```

and

```sql
> ./bin/mysqld --skip-grant &
./bin/mysqld: error while loading shared libraries: libwrap.so.0:
cannot open shared object file: No such file or directory
```

If you see either of the above errors, the binary MariaDB package you installed
is not compatible with your system.

The options you have are:

- Find another MariaDB package or tar from the
  [download page](https://downloads.mariadb.org/) that matches your
  system.

- or

- [Download the source](/kb/en/source-getting-the-mariadb-source-code/) and [build it](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/generic-build-instructions/).