# DROP DATABASE

## Syntax

```sql
DROP {DATABASE | SCHEMA} [IF EXISTS] db_name
```

## Description

`DROP DATABASE` drops all tables in the database and deletes the database. Be very careful with this statement! To use DROP DATABASE,
you need the [DROP privilege](/kb/en/grant/#table-privileges) on the database. `DROP SCHEMA` is a synonym for `DROP DATABASE`.

<strong>Important:</strong> When a database is dropped, user privileges on the database are not automatically dropped. See [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/).

#### IF EXISTS

Use `IF EXISTS` to prevent an error from occurring for databases that do not exist. A `NOTE` is generated for each non-existent database when using `IF EXISTS`. See [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/).

## Examples

```sql
DROP DATABASE bufg;
Query OK, 0 rows affected (0.39 sec)

DROP DATABASE bufg;
ERROR 1008 (HY000): Can't drop database 'bufg'; database doesn't exist

 \W
Show warnings enabled.

DROP DATABASE IF EXISTS bufg;
Query OK, 0 rows affected, 1 warning (0.00 sec)
Note (Code 1008): Can't drop database 'bufg'; database doesn't exist
```

## See Also

- [CREATE DATABASE](/sql-statements-structure/sql-statements/data-definition/create/create-database/)
- [ALTER DATABASE](/sql-statements-structure/sql-statements/data-definition/alter/alter-database/)
- [SHOW DATABASES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-databases/)
- [Information Schema SCHEMATA Table](/kb/en/information-schema-schemata-table/)
- [SHOW CREATE DATABASE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-database/)