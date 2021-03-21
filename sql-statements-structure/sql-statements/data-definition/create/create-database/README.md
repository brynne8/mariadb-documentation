# CREATE DATABASE

## Syntax

```sql
CREATE [OR REPLACE] {DATABASE | SCHEMA} [IF NOT EXISTS] db_name
    [create_specification] ...

create_specification:
    [DEFAULT] CHARACTER SET [=] charset_name
  | [DEFAULT] COLLATE [=] collation_name
  | COMMENT [=] 'comment'
```

## Description

`CREATE DATABASE` creates a database with the given name. To use this statement, you need the [CREATE privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) for the database. `CREATE SCHEMA` is a synonym for `CREATE DATABASE`.

For valid identifiers to use as database names, see [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names).

#### OR REPLACE

##### MariaDB starting with [10.1.3](/kb/en/mariadb-1013-release-notes/)

The `OR REPLACE` clause was added in [MariaDB 10.1.3](/kb/en/mariadb-1013-release-notes/)

If the optional `OR REPLACE` clause is used, it acts as a shortcut for:

```sql
DROP DATABASE IF EXISTS db_name;
CREATE DATABASE db_name ...;
```

#### IF NOT EXISTS

When the `IF NOT EXISTS` clause is used, MariaDB will return a warning instead of an error if the specified database already exists.

#### COMMENT

##### MariaDB starting with [10.5.0](/kb/en/mariadb-1050-release-notes/)

From [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/), it is possible to add a comment of a maximum of 1024 bytes. If the comment length exceeds this length, a error/warning code 4144 is thrown. The database comment is also added to the db.opt file, as well as to the [information_schema.schemata table](/kb/en/information-schema-schemata-table/).

## Examples

```sql
CREATE DATABASE db1;
Query OK, 1 row affected (0.18 sec)

CREATE DATABASE db1;
ERROR 1007 (HY000): Can't create database 'db1'; database exists

CREATE OR REPLACE DATABASE db1;
Query OK, 2 rows affected (0.00 sec)

CREATE DATABASE IF NOT EXISTS db1;
Query OK, 1 row affected, 1 warning (0.01 sec)

SHOW WARNINGS;
+-------+------+----------------------------------------------+
| Level | Code | Message                                      |
+-------+------+----------------------------------------------+
| Note  | 1007 | Can't create database 'db1'; database exists |
+-------+------+----------------------------------------------+
```

Setting the [character sets and collation](/kb/en/data-types-character-sets-and-collations/). See [Setting Character Sets and Collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/setting-character-sets-and-collations) for more details.

```sql
CREATE DATABASE czech_slovak_names 
  CHARACTER SET = 'keybcs2'
  COLLATE = 'keybcs2_bin';
```

Comments, from [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/):

```sql
CREATE DATABASE presentations COMMENT 'Presentations for conferences';
```

## See Also

- [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names)
- [DROP DATABASE](/sql-statements-structure/sql-statements/data-definition/drop/drop-database)
- [SHOW CREATE DATABASE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-database)
- [ALTER DATABASE](/sql-statements-structure/sql-statements/data-definition/alter/alter-database)
- [SHOW DATABASES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-databases)
- [Character Sets and Collations](/kb/en/character-sets-and-collations/)
- [Information Schema SCHEMATA Table](/kb/en/information-schema-schemata-table/)