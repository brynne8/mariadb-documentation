# SHOW CREATE DATABASE

## Syntax

```sql
SHOW CREATE {DATABASE | SCHEMA} db_name
```

## Description

Shows the [CREATE DATABASE](/sql-statements-structure/sql-statements/data-definition/create/create-database) statement that
creates the given database. `SHOW CREATE SCHEMA` is a synonym
for `SHOW CREATE DATABASE`. `SHOW CREATE DATABASE` quotes database names according to the value of the [sql_quote_show_create](/kb/en/server-system-variables/#sql_quote_show_create) server system variable.

## Examples

```sql
SHOW CREATE DATABASE test;
+----------+-----------------------------------------------------------------+
| Database | Create Database                                                 |
+----------+-----------------------------------------------------------------+
| test     | CREATE DATABASE `test` /*!40100 DEFAULT CHARACTER SET latin1 */ |
+----------+-----------------------------------------------------------------+

SHOW CREATE SCHEMA test;
+----------+-----------------------------------------------------------------+
| Database | Create Database                                                 |
+----------+-----------------------------------------------------------------+
| test     | CREATE DATABASE `test` /*!40100 DEFAULT CHARACTER SET latin1 */ |
+----------+-----------------------------------------------------------------+
```

With <a undefined>sql_quote_show_create</a> off:

```sql
SHOW CREATE DATABASE test;
+----------+---------------------------------------------------------------+
| Database | Create Database                                               |
+----------+---------------------------------------------------------------+
| test     | CREATE DATABASE test /*!40100 DEFAULT CHARACTER SET latin1 */ |
+----------+---------------------------------------------------------------+
```

With a comment, from [MariaDB 10.5](/kb/en/what-is-mariadb-105/):

```sql
SHOW CREATE DATABASE p;
+----------+--------------------------------------------------------------------------------------+
| Database | Create Database                                                                      |
+----------+--------------------------------------------------------------------------------------+
| p        | CREATE DATABASE `p` /*!40100 DEFAULT CHARACTER SET latin1 */ COMMENT 'presentations' |
+----------+--------------------------------------------------------------------------------------+
```

## See Also

- [CREATE DATABASE](/sql-statements-structure/sql-statements/data-definition/create/create-database)
- [ALTER DATABASE](/sql-statements-structure/sql-statements/data-definition/alter/alter-database)
- [Character Sets and Collations](/kb/en/character-sets-and-collations/)