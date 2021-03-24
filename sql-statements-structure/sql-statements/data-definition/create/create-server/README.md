# CREATE SERVER

## Syntax

```sql
CREATE [OR REPLACE] SERVER [IF NOT EXISTS] server_name
    FOREIGN DATA WRAPPER wrapper_name
    OPTIONS (option [, option] ...)

option:
  { HOST character-literal
  | DATABASE character-literal
  | USER character-literal
  | PASSWORD character-literal
  | SOCKET character-literal
  | OWNER character-literal
  | PORT numeric-literal }
```

## Description

This statement creates the definition of a server for use with the [Spider](/columns-storage-engines-and-plugins/storage-engines/spider/),
[FEDERATED](/columns-storage-engines-and-plugins/storage-engines/legacy-storage-engines/federated-storage-engine/) or [FederatedX](/kb/en/federatedx/) storage
engine. The CREATE SERVER statement creates a new row within the
[servers](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlservers-table/) table within the mysql database. This statement
requires the [SUPER](/kb/en/grant/#super) privilege or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [FEDERATED ADMIN](/kb/en/grant/#federated-admin) privilege.

The server_name should be a unique reference to the server. Server definitions
are global within the scope of the server, it is not possible to qualify the
server definition to a specific database. server_name has a maximum length of
64 characters (names longer than 64 characters are silently truncated), and is
case insensitive. You may specify the name as a quoted string.

The wrapper_name may be quoted with single quotes. Supported values are:

- `mysql`
- `mariadb` (in [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and later)

For each option you must specify either a character literal or numeric literal.
Character literals are UTF-8, support a maximum length of 64 characters and
default to a blank (empty) string. String literals are silently truncated to 64
characters. Numeric literals must be a number between 0 and 9999, default value
is 0.

<strong>Note</strong>: The `OWNER` option is currently not applied, and has no effect on
the ownership or operation of the server connection that is created.

The CREATE SERVER statement creates an entry in the
[mysql.servers](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlservers-table/) table that can later be used with the
CREATE TABLE statement when creating a [Spider](/columns-storage-engines-and-plugins/storage-engines/spider/), [FederatedX](/kb/en/federatedx/) or
[FEDERATED](/columns-storage-engines-and-plugins/storage-engines/legacy-storage-engines/federated-storage-engine/) table. The options that you specify will
be used to populate the columns in the mysql.servers table. The table columns
are Server_name, Host, Db, Username, Password, Port and Socket.

[DROP SERVER](/sql-statements-structure/sql-statements/data-definition/drop/drop-server/) removes a previously created server definition.

CREATE SERVER is not written to the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/), irrespective of
the [binary log format](/mariadb-administration/server-monitoring-logs/binary-log/binary-log-formats/) being used.

For valid identifiers to use as server names, see [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names/).

#### OR REPLACE

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

If the optional `OR REPLACE` clause is used, it acts as a shortcut for:

```sql
DROP SERVER IF EXISTS name;
CREATE SERVER server_name ...;
```

#### IF NOT EXISTS

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

If the IF NOT EXISTS clause is used, MariaDB will return a warning instead of an error if the server already exists. Cannot be used together with OR REPLACE.

## Examples

```sql
CREATE SERVER s
FOREIGN DATA WRAPPER mysql
OPTIONS (USER 'Remote', HOST '192.168.1.106', DATABASE 'test');
```

OR REPLACE and IF NOT EXISTS:

```sql
CREATE SERVER s 
FOREIGN DATA WRAPPER mysql 
OPTIONS (USER 'Remote', HOST '192.168.1.106', DATABASE 'test');
ERROR 1476 (HY000): The foreign server, s, you are trying to create already exists

CREATE OR REPLACE SERVER s 
FOREIGN DATA WRAPPER mysql 
OPTIONS (USER 'Remote', HOST '192.168.1.106', DATABASE 'test');
Query OK, 0 rows affected (0.00 sec)

CREATE SERVER IF NOT EXISTS s 
FOREIGN DATA WRAPPER mysql 
OPTIONS (USER 'Remote', HOST '192.168.1.106', DATABASE 'test');
Query OK, 0 rows affected, 1 warning (0.00 sec)

SHOW WARNINGS;
+-------+------+----------------------------------------------------------------+
| Level | Code | Message                                                        |
+-------+------+----------------------------------------------------------------+
| Note  | 1476 | The foreign server, s, you are trying to create already exists |
+-------+------+----------------------------------------------------------------+
```

## See Also

- [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names/)
- [ALTER SERVER](/sql-statements-structure/sql-statements/data-definition/alter/alter-server/)
- [DROP SERVER](/sql-statements-structure/sql-statements/data-definition/drop/drop-server/)
- [Spider Storage Engine](/columns-storage-engines-and-plugins/storage-engines/spider/)