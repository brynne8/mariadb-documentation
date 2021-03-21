# Installing System Tables (mysql_install_db)

`mysql_install_db` initializes the MariaDB data directory and creates the
[system tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables) in the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables) database, if they do not exist. MariaDB uses these tables to manage [privileges](/kb/en/grant/#privilege-levels), [roles](/mariadb-administration/user-server-security/user-account-management/roles), and [plugins](/columns-storage-engines-and-plugins/plugins). It also uses them to provide the data for the [help](/sql-statements-structure/sql-statements/administrative-sql-statements/help-command) command in the [mysql](/clients-utilities/mysql-client/mysql-command-line-client) client.

[mysql_install_db](/clients-utilities/mysql_install_db) works by starting MariaDB Server's `mysqld` process in <a undefined>--bootstrap</a> mode and sending commands to create the [system tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables) and their content.

There is a version specifically for Windows, [mysql_install_db.exe](/mariadb-administration/getting-installing-and-upgrading-mariadb/mysql_install_dbexe).

To invoke `mysql_install_db`, use the following syntax:

```sql
mysql_install_db --user=mysql
```

For the options supported by [mysql_install_db](/clients-utilities/mysql_install_db), see [mysql_install_db: Options](/kb/en/mysql_install_db/#options).

For the option groups read by [mysql_install_db](/clients-utilities/mysql_install_db), see [mysql_install_db: Option Groups](/kb/en/mysql_install_db/#option-groups).

See [mysql_install_db: Installing System Tables](/kb/en/mysql_install_db/#installing-system-tables) for information on the installation process.

See [mysql_install_db: Troubleshooting Issues](/kb/en/mysql_install_db/#troubleshooting-issues) for information on how to troubleshoot the installation process.

## See also:

- [mysql_install_db](/clients-utilities/mysql_install_db)
- The Windows version of `mysql_install_db`: [mysql_install_db.exe](/mariadb-administration/getting-installing-and-upgrading-mariadb/mysql_install_dbexe)