# ALTER DATABASE

Modifies a database, changing its overall characteristics.

## Syntax

```sql
ALTER {DATABASE | SCHEMA} [db_name]
    alter_specification ...
ALTER {DATABASE | SCHEMA} db_name
    UPGRADE DATA DIRECTORY NAME

alter_specification:
    [DEFAULT] CHARACTER SET [=] charset_name
  | [DEFAULT] COLLATE [=] collation_name
  | COMMENT [=] 'comment'
```

## Description

`ALTER DATABASE` enables you to change the overall characteristics of a
database. These characteristics are stored in the `db.opt` file in the
database directory. To use `ALTER DATABASE`, you need the `ALTER`
privilege on the database. `ALTER SCHEMA` is a synonym for ALTER
DATABASE.

The `CHARACTER SET` clause changes the default database character set.
The `COLLATE` clause changes the default database collation. See [Character Sets and Collations](/kb/en/data-types-character-sets-and-collations/) for more.

You can see what character sets and collations are available using,
respectively, the [SHOW CHARACTER SET](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-character-set) and [SHOW COLLATION](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-collation) statements.

Changing the default character set/collation of a database does not change the character set/collation of any [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures) or [stored functions](/programming-customizing-mariadb/stored-routines/stored-functions) that were previously created, and relied on the defaults. These need to be dropped and recreated in order to apply the character set/collation changes.

The database name can be omitted from the first syntax, in which case
the statement applies to the default database.

The syntax that includes the `UPGRADE DATA DIRECTORY NAME` clause was
added in MySQL 5.1.23. It updates the name of the directory associated
with the database to use the encoding implemented in MySQL 5.1 for
mapping database names to database directory names (see
[Identifier to File Name Mapping](/sql-statements-structure/sql-language-structure/identifier-to-file-name-mapping)). This
clause is for use under these conditions:

- It is intended when upgrading MySQL to 5.1 or later from older versions.
- It is intended to update a database directory name to the current encoding format if the name contains special characters that need encoding.
- The statement is used by `mysqlcheck` (as invoked by `mysql_upgrade`).

For example,if a database in MySQL 5.0 has a name of a-b-c, the name
contains instance of the `-' character. In 5.0, the database directory
is also named a-b-c, which is not necessarily safe for all file
systems. In MySQL 5.1 and up, the same database name is encoded as
a@002db@002dc to produce a file system-neutral directory name.

When a MySQL installation is upgraded to MySQL 5.1 or later from an
older version,the server displays a name such as a-b-c (which is in
the old format) as #mysql50#a-b-c, and you must refer to the name
using the #mysql50# prefix. Use `UPGRADE DATA DIRECTORY NAME` in this
case to explicitly tell the server to re-encode the database directory
name to the current encoding format:

```sql
ALTER DATABASE `#mysql50#a-b-c` UPGRADE DATA DIRECTORY NAME;
```

After executing this statement, you can refer to the database as a-b-c
without the special #mysql50# prefix.

#### COMMENT

##### MariaDB starting with [10.5.0](/kb/en/mariadb-1050-release-notes/)

From [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/), it is possible to add a comment of a maximum of 1024 bytes. If the comment length exceeds this length, a error/warning code 4144 is thrown. The database comment is also added to the db.opt file, as well as to the [information_schema.schemata table](/kb/en/information-schema-schemata-table/).

## Examples

```sql
ALTER DATABASE test CHARACTER SET='utf8'  COLLATE='utf8_bin';
```

From [MariaDB 10.5.0](/kb/en/mariadb-1050-release-notes/):

```sql
ALTER DATABASE p COMMENT='Presentations';
```

## See Also

- [CREATE DATABASE](/sql-statements-structure/sql-statements/data-definition/create/create-database)
- [DROP DATABASE](/sql-statements-structure/sql-statements/data-definition/drop/drop-database)
- [SHOW CREATE DATABASE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-database)
- [SHOW DATABASES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-databases)
- [Character Sets and Collations](/kb/en/character-sets-and-collations/)
- [Information Schema SCHEMATA Table](/kb/en/information-schema-schemata-table/)