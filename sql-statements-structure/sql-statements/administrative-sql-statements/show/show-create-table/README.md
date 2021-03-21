# SHOW CREATE TABLE

## Syntax

```sql
SHOW CREATE TABLE tbl_name
```

## Description

Shows the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) statement that created the given
table. The statement requires the [SELECT privilege](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) for the table. This statement also works with [views](/programming-customizing-mariadb/views) and [SEQUENCE](/sql-statements-structure/sequences/create-sequence).

`SHOW CREATE TABLE` quotes table and
column names according to the value of the [sql_quote_show_create](/kb/en/server-system-variables/#sql_quote_show_create) server system variable.

Certain [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) values can result in parts of the original CREATE statement not being included in the output. MariaDB-specific table options, column options, and index options are not included in the output of this statement if the `NO_TABLE_OPTIONS`, `NO_FIELD_OPTIONS` and `NO_KEY_OPTIONS` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) flags are used. All MariaDB-specific table attributes are also not shown when a non-MariaDB/MySQL emulation mode is used, which includes `ANSI`, `DB2`, `POSTGRESQL`, `MSSQL`, `MAXDB` or `ORACLE`.

Invalid table options, column options and index options are normally commented out (note, that it is possible to create a table with invalid options, by altering a table of a different engine, where these options were valid). To have them uncommented, enable the `IGNORE_BAD_TABLE_OPTIONS` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode). Remember that replaying a [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table) statement with uncommented invalid options will fail with an error, unless the `IGNORE_BAD_TABLE_OPTIONS` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) is in effect.

Note that `SHOW CREATE TABLE` is not meant to provide metadata about a table. It provides information about how the table was declared, but the real table structure could differ a bit. For example, if an index has been declared as `HASH`, the `CREATE TABLE` statement returned by `SHOW CREATE TABLE` will declare that index as `HASH`; however, it is possible that the index is in fact a `BTREE`, because the storage engine does not support `HASH`.

##### MariaDB starting with [10.2.1](/kb/en/mariadb-1021-release-notes/)

[MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/) permits [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text) and [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob) data types to be assigned a [DEFAULT](/kb/en/create-table/#default) value. As a result, from [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/), `SHOW CREATE TABLE` will append a `DEFAULT NULL` to nullable TEXT or BLOB fields if no specific default is provided.

##### MariaDB starting with [10.2.2](/kb/en/mariadb-1022-release-notes/)

From [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/), numbers are no longer quoted in the `DEFAULT` clause in `SHOW CREATE` statement. Previously, MariaDB quoted numbers.

## Examples

```sql
SHOW CREATE TABLE t\G
*************************** 1. row ***************************
       Table: t
Create Table: CREATE TABLE `t` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `s` char(60) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1
```

With [sql_quote_show_create](/kb/en/server-system-variables/#sql_quote_show_create) off:

```sql
SHOW CREATE TABLE t\G
*************************** 1. row ***************************
       Table: t
Create Table: CREATE TABLE t (
  id int(11) NOT NULL AUTO_INCREMENT,
  s char(60) DEFAULT NULL,
  PRIMARY KEY (id)
) ENGINE=InnoDB DEFAULT CHARSET=latin1
```

Unquoted numeric DEFAULTs, from [MariaDB 10.2.2](/kb/en/mariadb-1022-release-notes/):

```sql
CREATE TABLE td (link TINYINT DEFAULT 1);

SHOW CREATE TABLE td\G
*************************** 1. row ***************************
       Table: td
Create Table: CREATE TABLE `td` (
  `link` tinyint(4) DEFAULT 1
) ENGINE=InnoDB DEFAULT CHARSET=latin1
```

Quoted numeric DEFAULTs, until [MariaDB 10.2.1](/kb/en/mariadb-1021-release-notes/):

```sql
CREATE TABLE td (link TINYINT DEFAULT 1);

SHOW CREATE TABLE td\G
*************************** 1. row ***************************
       Table: td
Create Table: CREATE TABLE `td` (
  `link` tinyint(4) DEFAULT '1'
) ENGINE=InnoDB DEFAULT CHARSET=latin1
```

[SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) impacting the output:

```sql
SELECT @@sql_mode;
+-------------------------------------------------------------------------------------------+
| @@sql_mode                                                                                |
+-------------------------------------------------------------------------------------------+
| STRICT_TRANS_TABLES,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION |
+-------------------------------------------------------------------------------------------+

CREATE TABLE `t1` (
       `id` int(11) NOT NULL AUTO_INCREMENT,
       `msg` varchar(100) DEFAULT NULL,
       PRIMARY KEY (`id`)
     ) ENGINE=InnoDB DEFAULT CHARSET=latin1
;

SHOW CREATE TABLE t1\G
*************************** 1. row ***************************
       Table: t1
Create Table: CREATE TABLE `t1` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `msg` varchar(100) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=latin1

SET SQL_MODE=ORACLE;

SHOW CREATE TABLE t1\G
*************************** 1. row ***************************
       Table: t1
Create Table: CREATE TABLE "t1" (
  "id" int(11) NOT NULL,
  "msg" varchar(100) DEFAULT NULL,
  PRIMARY KEY ("id")
```

## See Also

- [SHOW CREATE SEQUENCE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-sequence)
- [SHOW CREATE VIEW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-view)