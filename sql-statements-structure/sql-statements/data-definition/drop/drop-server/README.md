# DROP SERVER

## Syntax

```sql
DROP SERVER [ IF EXISTS ] server_name
```

## Description

Drops the server definition for the server named <em>server_name</em>. The
corresponding row within the [mysql.servers table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlservers-table/) will be deleted. This statement requires the [SUPER](/kb/en/grant/#super) privilege or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [FEDERATED ADMIN](/kb/en/grant/#federated-admin) privilege.

Dropping a server for a table does not affect any [FederatedX](/kb/en/federatedx/), [FEDERATED](/columns-storage-engines-and-plugins/storage-engines/legacy-storage-engines/federated-storage-engine/) or [Spider](/columns-storage-engines-and-plugins/storage-engines/spider/) tables that used this connection information when they were created.

#### IF EXISTS

If the IF EXISTS clause is used, MariaDB will not return an error if the server does not exist. Unlike all other statements, DROP SERVER IF EXISTS does not issue a note if the server does not exist. See [MDEV-9400](https://jira.mariadb.org/browse/MDEV-9400).

## Examples

```sql
DROP SERVER s;
```

IF EXISTS:

```sql
DROP SERVER s;
ERROR 1477 (HY000): The foreign server name you are trying to reference 
  does not exist. Data source error:  s

DROP SERVER IF EXISTS s;
Query OK, 0 rows affected (0.00 sec)
```

## See Also

- [CREATE SERVER](/sql-statements-structure/sql-statements/data-definition/create/create-server/)
- [ALTER SERVER](/sql-statements-structure/sql-statements/data-definition/alter/alter-server/)
- [Spider Storage Engine](/columns-storage-engines-and-plugins/storage-engines/spider/)