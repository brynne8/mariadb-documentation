# Installing MariaDB Windows ZIP Packages

Getting started with ZIP packages on Windows is very straightforward - this distribution includes pre-built database files that can be used right after unpacking the ZIP.

You can run mysqld.exe from the command prompt like this:

```sql
  cd <directory-where-zip-was-unpacked>
  bin\mysqld --console
```

However, running MariaDB from the command line in a user session like this covers only the simplest testing scenarios.

For any serious purpose, use  [mysql_install_db.exe](/mariadb-administration/getting-installing-and-upgrading-mariadb/mysql_install_dbexe) to create database instances - it is more secure (disables anonymous user by default, allows setting root password during database creation), and more convenient (can create Windows service, supports user-specified data directory).

Note : Starting with [MariaDB 10.4.3](/kb/en/mariadb-1043-release-notes/), a pre-built data directory is no longer provided, and users need to run   [mysql_install_db.exe](/mariadb-administration/getting-installing-and-upgrading-mariadb/mysql_install_dbexe) to create a data directory.

For older MariaDB releases, you can follow the instructions in [MySQL documentation on ZIP distributions](http://dev.mysql.com/doc/refman/5.5/en/windows-install-archive.html)  - this describes the manual process for registering a service and securing the database.