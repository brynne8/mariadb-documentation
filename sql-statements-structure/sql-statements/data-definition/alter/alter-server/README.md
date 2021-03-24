# ALTER SERVER

## Syntax

```sql
ALTER SERVER server_name
    OPTIONS (option [, option] ...)
```

## Description

Alters the server information for <em>server_name</em>, adjusting the specified
options as per the [CREATE SERVER](/sql-statements-structure/sql-statements/data-definition/create/create-server/) command. The corresponding fields in the [mysql.servers table](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqlservers-table/) are updated accordingly. This statement requires the [SUPER](/kb/en/grant/#super) privilege or, from [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), the [FEDERATED ADMIN](/kb/en/grant/#federated-admin) privilege.

## Examples

```sql
ALTER SERVER s OPTIONS (USER 'sally');
```

## See Also

- [CREATE SERVER](/sql-statements-structure/sql-statements/data-definition/create/create-server/)
- [DROP SERVER](/sql-statements-structure/sql-statements/data-definition/drop/drop-server/)
- [Spider Storage Engine](/columns-storage-engines-and-plugins/storage-engines/spider/)