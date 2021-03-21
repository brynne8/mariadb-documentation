# SQL_MODE

MariaDB supports several different modes which allow you to tune it to suit your needs.

The most important ways for doing this are using `SQL_MODE` (controlled by the [sql_mode](/kb/en/server-system-variables/#sql_mode) system variable) and [OLD_MODE](/kb/en/old_mode/) (the [old_mode](/kb/en/server-system-variables/#old_mode) system variable). `SQL_MODE` is used for getting MariaDB to emulate behavior from other SQL servers, while [OLD_MODE](/kb/en/old_mode/) is used for emulating behavior from older MariaDB or MySQL versions.

<code class="fixed" style="white-space:pre-wrap">SQL_MODE</code>is a string with different options separated by commas ('`,`') without spaces. The options are case insensitive.

You can check the local and global value of it with:

```sql
SELECT @@SQL_MODE, @@GLOBAL.SQL_MODE;
```

## Setting SQL_MODE

### Defaults

<table><tbody><tr><th>From version</th><th>Default sql_mode setting</th></tr>
<tr><td><a href="/kb/en/mariadb-1024-release-notes/">MariaDB 10.2.4</a></td><td><span class="cstm-style" style="color: green;">STRICT_TRANS_TABLES, ERROR_FOR_DIVISION_BY_ZERO </span>, NO_AUTO_CREATE_USER, NO_ENGINE_SUBSTITUTION</td></tr>
<tr><td><a href="/kb/en/mariadb-1017-release-notes/">MariaDB 10.1.7</a></td><td><span class="cstm-style" style="color: green;">NO_ENGINE_SUBSTITUTION, NO_AUTO_CREATE_USER</span></td></tr>
<tr><td>&lt;= <a href="/kb/en/mariadb-1016-release-notes/">MariaDB 10.1.6</a></td><td>No value</td></tr>
</tbody></table>

You can set the <code class="fixed" style="white-space:pre-wrap"><span class="n">SQL_MODE</span>
</code> either from the
[command line](/kb/en/mysqld-options-full-list/) (the <code class="fixed" style="white-space:pre-wrap">--sql-mode</code> option) or by setting the [sql_mode](/kb/en/server-system-variables/#sql_mode) system variable.

```sql
SET sql_mode = 'modes';
SET GLOBAL sql_mode = 'modes';
```

The session value only affects the current client, and can be changed by the client when required. To set the global value, the SUPER privilege is required, and the change affects any clients that connect from that point on.

## SQL_MODE Values

The different `SQL_MODE` values are:

#### ALLOW_INVALID_DATES

Allow any day between 1-31 in the day part. This is convenient when you want to read in all (including wrong data) into the database and then manipulate it there.

#### ANSI

Changes the SQL syntax to be closer to ANSI SQL.

Sets: [REAL_AS_FLOAT](#real_as_float),  [PIPES_AS_CONCAT](#pipes_as_concat), [ANSI_QUOTES](#ansi_quotes), [IGNORE_SPACE](#ignore_space).

It also adds a restriction: an error will be returned if a subquery uses an [aggregating function](/kb/en/functions-and-modifiers-for-use-with-group-by/) with a reference to a column from an outer query in a way that cannot be resolved.

If set, [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) output will not display MariaDB-specific table attributes.

#### ANSI_QUOTES

Changes `"` to be treated as ```, the identifier quote character. This may break old MariaDB applications which assume that `"` is used as a string quote character.

#### DB2

Same as: [PIPES_AS_CONCAT](#pipes_as_concat), [ANSI_QUOTES](#ansi_quotes) , [IGNORE_SPACE](#ignore_space), [DB2](#db2), [NO_KEY_OPTIONS](#no_key_options), [NO_TABLE_OPTIONS](#no_table_options), [NO_FIELD_OPTIONS](#no_field_options)

If set, [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) output will not display MariaDB-specific table attributes.

#### EMPTY_STRING_IS_NULL

Oracle-compatibility option that translates Item_string created in the parser to Item_null, and translates binding an empty string as prepared statement parameters to binding NULL. For example, `SELECT '' IS NULL` returns TRUE, `INSERT INTO t1 VALUES ('')` inserts NULL. Since [MariaDB 10.3.3](/kb/en/mariadb-1033-release-notes/)

#### ERROR_FOR_DIVISION_BY_ZERO

If not set, division by zero returns NULL. If set returns an error if one tries to update a column with 1/0 and returns a warning as well. Also see [MDEV-8319](https://jira.mariadb.org/browse/MDEV-8319). Default since [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/).

#### HIGH_NOT_PRECEDENCE

Compatibility option for MySQL 5.0.1 and before; This changes `NOT a BETWEEN b AND c` to be parsed as `(NOT a) BETWEEN b AND c`

#### IGNORE_BAD_TABLE_OPTIONS

If this is set generate a warning (not an error) for wrong table option in CREATE TABLE. Also, since 10.0.13, do not comment out these wrong table options in [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/).

#### IGNORE_SPACE

Allow one to have spaces (including tab characters and new line characters) between function name and '('. The drawback is that this causes built in functions to become [reserved words](/sql-statements-structure/sql-language-structure/reserved-words/).

#### MAXDB

Same as: [PIPES_AS_CONCAT](#pipes_as_concat), [ANSI_QUOTES](#ansi_quotes), [IGNORE_SPACE](#ignore_space), [MAXDB](#maxdb), [NO_KEY_OPTIONS](#no_key_options), [NO_TABLE_OPTIONS](#no_table_options), [NO_FIELD_OPTIONS](#no_field_options), [NO_AUTO_CREATE_USER](#no_auto_create_user).

Also has the effect of silently converting [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp/) fields into [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime/) fields when created or modified.

If set, [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) output will not display MariaDB-specific table attributes.

#### MSSQL

Additionally implies the following: [PIPES_AS_CONCAT](#pipes_as_concat), [ANSI_QUOTES](#ansi_quotes), [IGNORE_SPACE](#ignore_space), [NO_KEY_OPTIONS](#no_key_options), [NO_TABLE_OPTIONS](#no_table_options), [NO_FIELD_OPTIONS](#no_field_options).

Additionally from [MariaDB 10.4.5](/kb/en/mariadb-1045-release-notes/), implements a limited subset of Microsoft SQL Server's language. See [SQL_MODE=MSSQL](/kb/en/sql_modemssql/) for more.

If set, [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) output will not display MariaDB-specific table attributes.

#### MYSQL323

Same as: [NO_FIELD_OPTIONS](#no_field_options), [HIGH_NOT_PRECEDENCE](#high_not_precedence).

#### MYSQL40

Same as: [NO_FIELD_OPTIONS](#no_field_options), [HIGH_NOT_PRECEDENCE](#high_not_precedence).

#### NO_AUTO_CREATE_USER

Don't automatically create users with `GRANT` unless authentication information is specified. If none is provided, will produce a 1133 error: "Can't find any matching row in the user table". Default since [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/).

#### NO_AUTO_VALUE_ON_ZERO

If set don't generate an [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/) on [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) of zero in an `AUTO_INCREMENT` column.  Normally both `zero` and `NULL` generate new `AUTO_INCREMENT` values.

#### NO_BACKSLASH_ESCAPES

Disables using the backslash character `\` as an escape character within strings, making it equivalent to an ordinary character.

#### NO_DIR_IN_CREATE

Ignore all INDEX DIRECTORY and DATA DIRECTORY directives when creating a table. Can be useful on slave [replication](/replication/) servers.

#### NO_ENGINE_SUBSTITUTION

If not set, if the available storage engine specified by a CREATE TABLE is not available, a warning is given and the default storage engine is used instead. If set, generate a 1286 error when creating a table if the specified [storage engine](/columns-storage-engines-and-plugins/storage-engines/) is not available. See also [enforce_storage_engine](/kb/en/server-system-variables/#enforce_storage_engine). Default since [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/).

#### NO_FIELD_OPTIONS

Remove MariaDB-specific column options from the output of [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/). This is also used by the portability mode of [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/).

#### NO_KEY_OPTIONS

Remove MariaDB-specific index options from the output of [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/). This is also used by the portability mode of [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/).

#### NO_TABLE_OPTIONS

Remove MariaDB-specific table options from the output of [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/). This is also used by the portability mode of [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/).

#### NO_UNSIGNED_SUBTRACTION

When enabled, subtraction results are signed even if the operands are unsigned.

#### NO_ZERO_DATE

Don't allow '0000-00-00' as a valid date in strict mode (produce a 1525 error). Zero dates can be inserted with [IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/ignore/). If not in strict mode, a warning is generated.

#### NO_ZERO_IN_DATE

Don't allow dates where the year is not zero but the month or day parts of the date <em>are</em> zero (produce a 1525 error). For example, with this set, '0000-00-00' is allowed, but '1970-00-10' or '1929-01-00' are not. If the ignore option is used, MariaDB will insert '0000-00-00' for those types of dates. If not in strict mode, a warning is generated instead.

#### ONLY_FULL_GROUP_BY

For [SELECT ... GROUP BY](/kb/en/select/#group-by) queries, disallow [SELECTing](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) columns which are not referred to in the GROUP BY clause, unless they are passed to an aggregate function like [COUNT()](/built-in-functions/aggregate-functions/count/) or [MAX()](/built-in-functions/aggregate-functions/max/). Produce a 1055 error.

#### ORACLE

In all versions of MariaDB, this sets `sql_mode` that is equivalent to: [PIPES_AS_CONCAT](#pipes_as_concat), [ANSI_QUOTES](#ansi_quotes), [IGNORE_SPACE](#ignore_space), [NO_KEY_OPTIONS](#no_key_options), [NO_TABLE_OPTIONS](#no_table_options), [NO_FIELD_OPTIONS](#no_field_options), [NO_AUTO_CREATE_USER](#no_auto_create_user), [SIMULTANEOUS_ASSIGNMENT](#simultaneous_assignment)

From [MariaDB 10.3](/kb/en/what-is-mariadb-103/), this mode also configures the server to understand a large subset of Oracle's PL/SQL language instead of MariaDB's traditional syntax for stored routines. See [SQL_MODE=ORACLE From MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/).

If set, [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) output will not display MariaDB-specific table attributes.

#### PAD_CHAR_TO_FULL_LENGTH

Trailing spaces in [CHAR](/columns-storage-engines-and-plugins/data-types/string-data-types/char/) columns are by default trimmed upon retrieval. With PAD_CHAR_TO_FULL_LENGTH enabled, no trimming occurs. Does not apply to [VARCHARs](/columns-storage-engines-and-plugins/data-types/string-data-types/varchar/).

#### PIPES_AS_CONCAT

Allows using the pipe character (ASCII 124) as string concatenation operator. This means that `"A" || "B"` can be used in place of `CONCAT("A", "B")`.

#### POSTGRESQL

Same as: [PIPES_AS_CONCAT](#pipes_as_concat), [ANSI_QUOTES](#ansi_quotes), [IGNORE_SPACE](#ignore_space), [POSTGRESQL](#postgresql), [NO_KEY_OPTIONS](#no_key_options), [NO_TABLE_OPTIONS](#no_table_options), [NO_FIELD_OPTIONS](#no_field_options).

If set, [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) output will not display MariaDB-specific table attributes.

#### REAL_AS_FLOAT

`REAL` is a synonym for [FLOAT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/float/) rather than [DOUBLE](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/double/).

#### SIMULTANEOUS_ASSIGNMENT

Setting this makes the SET part of the [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) statement evaluate all assignments simultaneously, not left-to-right. From [MariaDB 10.3.5](/kb/en/mariadb-1035-release-notes/).

#### STRICT_ALL_TABLES

Strict mode. Statements with invalid or missing data are aborted and rolled back. For a non-transactional storage engine with a statement affecting multiple rows, this may mean a partial insert or update if the error is found in a row beyond the first.

#### STRICT_TRANS_TABLES

Strict mode. Statements with invalid or missing data are aborted and rolled back, except that for non-transactional storage engines and statements affecting multiple rows where the invalid or missing data is not the first row, MariaDB will convert the invalid value to the closest valid value, or, if a value is missing, insert the column default value. Default since [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/).

#### TIME_ROUND_FRACTIONAL

With this mode unset, MariaDB truncates fractional seconds when changing precision to smaller. When set, MariaDB will round when converting to TIME, DATETIME and TIMESTAMP, and truncate when converting to DATE. Since [MariaDB 10.4.1](/kb/en/mariadb-1041-release-notes/)

#### TRADITIONAL

Makes MariaDB work like a traditional SQL server. Same as: [STRICT_TRANS_TABLES](#strict_trans_tables), [STRICT_ALL_TABLES](#strict_all_tables), [NO_ZERO_IN_DATE](#no_zero_in_date), [NO_ZERO_DATE](#no_zero_date), [ERROR_FOR_DIVISION_BY_ZERO](#error_for_division_by_zero), [TRADITIONAL](#traditional), [NO_AUTO_CREATE_USER](#no_auto_create_user).

## Strict Mode

A mode where at least one of `STRICT_TRANS_TABLES` or `STRICT_ALL_TABLES` is enabled is called <em>strict mode</em>.

With strict mode set (default from [MariaDB 10.2.4](/kb/en/mariadb-1024-release-notes/)), statements that modify tables (either transactional for `STRICT_TRANS_TABLES` or all for `STRICT_ALL_TABLES`) will fail, and an error will be returned instead. The IGNORE keyword can be used when strict mode is set to convert the error to a warning.

With strict mode not set (default in version &lt;= [MariaDB 10.2.3](/kb/en/mariadb-1023-release-notes/)), MariaDB will automatically adjust invalid values, for example, truncating strings that are too long, or adjusting numeric values that are out of range, and produce a warning.

Statements that don't modify data will return a warning when adjusted regardless of mode.

## SQL_MODE and Stored Programs

[Stored programs and views](/kb/en/stored-programs-and-views/) always use the SQL_MODE that was active when they were created. This means that users can safely change session or global SQL_MODE; the stored programs they use will still work as usual.

It is possible to change session SQL_MODE within a stored program. In this case, the new SQL_MODE will be in effect only in the body of the current stored program. If it calls some stored procedures, they will not be affected by the change.

Some Information Schema tables (such as [ROUTINES](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-routines-table/)) and SHOW CREATE statements such as [SHOW CREATE PROCEDURE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-procedure/) show the SQL_MODE used by the stored programs.

## Examples

This example shows how to get a readable list of enabled SQL_MODE flags:

```sql
SELECT REPLACE(@@SQL_MODE, ',', '\n');
+-------------------------------------------------------------------------+
| REPLACE(@@SQL_MODE, ',', '\n')                                          |
+-------------------------------------------------------------------------+
| STRICT_TRANS_TABLES
NO_ZERO_IN_DATE
NO_ZERO_DATE
NO_ENGINE_SUBSTITUTION |
+-------------------------------------------------------------------------+
```

Adding a new flag:

```sql
SET @@SQL_MODE = CONCAT(@@SQL_MODE, ',NO_ENGINE_SUBSTITUTION');
```

If the specified flag is already ON, the above example has no effect but does not produce an error.

How to unset a flag:

```sql
SET @@SQL_MODE = REPLACE(@@SQL_MODE, 'NO_ENGINE_SUBSTITUTION', '');
```

How to check if a flag is set:

```sql
SELECT @@SQL_MODE LIKE '%NO_ZERO_DATE%';
+----------------------------------+
| @@SQL_MODE LIKE '%NO_ZERO_DATE%' |
+----------------------------------+
|                                1 |
+----------------------------------+
```

Without and with strict mode:

```sql
CREATE TABLE strict (s CHAR(5), n TINYINT);

INSERT INTO strict VALUES ('MariaDB', '128');
Query OK, 1 row affected, 2 warnings (0.14 sec)

SHOW WARNINGS;
+---------+------+--------------------------------------------+
| Level   | Code | Message                                    |
+---------+------+--------------------------------------------+
| Warning | 1265 | Data truncated for column 's' at row 1     |
| Warning | 1264 | Out of range value for column 'n' at row 1 |
+---------+------+--------------------------------------------+
2 rows in set (0.00 sec)

SELECT * FROM strict;
+-------+------+
| s     | n    |
+-------+------+
| Maria |  127 |
+-------+------+

SET sql_mode='STRICT_TRANS_TABLES';

INSERT INTO strict VALUES ('MariaDB', '128');
ERROR 1406 (22001): Data too long for column 's' at row 1
```

Overriding strict mode with the IGNORE keyword:

```sql
INSERT IGNORE INTO strict VALUES ('MariaDB', '128');
Query OK, 1 row affected, 2 warnings (0.15 sec)
```

## See Also

- [SQL_MODE=MSSQL](/kb/en/sql_modemssql/)
- [SQL_MODE=ORACLE](/kb/en/sql_modeoracle-from-mariadb-103/)