# SecuRich

SecuRich has not been updated since 2011.

SecuRich is a library of [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures/) that adds security features to MariaDB and MySQL. It is maintained by Darren Cassar, and can be downloaded from [securich.com](http://www.securich.com/).

Its main purpose is to emulate [roles](/mariadb-administration/user-server-security/user-account-management/roles/), but this feature has been supported in the server since [MariaDB 10.0](/kb/en/what-is-mariadb-100/). However other interesting features are available via SecuRich, such as the ability to block a user, clone a user or set a password that will expire (also supported since [MariaDB 10.4](/kb/en/what-is-mariadb-104/) - see [User Password Expiry](/mariadb-administration/user-server-security/user-account-management/user-password-expiry/)).

To use SecuRich features, its stored procedures should be used instead of the usual security-related SQL statements. For example, drop_user() should be used instead of [DROP USER](/sql-statements-structure/sql-statements/account-management-sql-commands/drop-user/). However, if a combination of SecuRich procedure and normal SQL statements is used, SecuRich can reconcile its tables with the system tables.