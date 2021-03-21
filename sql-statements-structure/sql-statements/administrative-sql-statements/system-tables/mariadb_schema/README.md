# mariadb_schema

`mariadb_schema` is a data type qualifier that allows one to create MariaDB native date types in an [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode/) that has conflicting data type translations.

`mariadb_schema` was introduced in [MariaDB 10.3.24](/kb/en/mariadb-10324-release-notes/), [MariaDB 10.4.14](/kb/en/mariadb-10414-release-notes/) and [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/).

For example, in [SQL_MODE=ORACLE](/kb/en/sql_modeoracle/), if one creates a table with the [DATE](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/date/) type, it will actually create a [DATETIME](/kb/en/datetime/#oracle-mode) column to match what an Oracle user is expecting. To be able to create a MariaDB DATE in Oracle mode one would have to use `mariadb_schema`:

```sql
CREATE TABLE t1 (d mariadb_schema.DATE);
```

`mariadb_schema` is also shown if one creates a table with `DATE` in MariaDB native mode and then does a [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) in `ORACLE` mode:

```sql
SET sql_mode=DEFAULT;
CREATE OR REPLACE TABLE t1 (
  d DATE
);
SET SQL_mode=ORACLE;
SHOW CREATE TABLE t1;
+-------+--------------------------------------------------------------+
| Table | Create Table                                                 |
+-------+--------------------------------------------------------------+
| t1    | CREATE TABLE "t1" (
  "d" mariadb_schema.date DEFAULT NULL
) |
+-------+--------------------------------------------------------------+
```

When the server sees the `mariadb_schema` qualifier, it disables sql_mode-specific data type translation and interprets the data type literally, so for example `mariadb_schema.DATE` is interpreted as the traditional MariaDB `DATE` data type, no matter what the current sql_mode is.

The `mariadb_schema` prefix is displayed only when the data type name would be ambiguous otherwise. The prefix is displayed together with MariaDB `DATE` when [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) is executed in [SQL_MODE=ORACLE](/kb/en/sql_modeoracle/). The prefix is not displayed when [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) is executed in SQL_MODE=DEFAULT, or when a non-ambiguous data type is displayed.

Note, the `mariadb_schema` prefix can be used with any data type, including non-ambiguous ones:

```sql
CREATE OR REPLACE TABLE t1 (a mariadb_schema.INT);
SHOW CREATE TABLE t1;
+-------+--------------------------------------------------+
| Table | Create Table                                     |
+-------+--------------------------------------------------+
| t1    | CREATE TABLE "t1" (
  "a" int(11) DEFAULT NULL
) |
+-------+--------------------------------------------------+
```

Currently the `mariadb_schema` prefix is only used in the following case:

- For a MariaDB native [DATE](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/date/) type when running [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) in [Oracle mode](/kb/en/sql_modeoracle/).

## History

When running with [SQL_MODE=ORACLE](/kb/en/sql_modeoracle/), MariaDB server translates the data type `DATE` to `DATETIME`, for better Oracle compatibility:

```sql
SET SQL_mode=ORACLE;
CREATE OR REPLACE TABLE t1 (
  d DATE
);
SHOW CREATE TABLE t1;
+-------+---------------------------------------------------+
| Table | Create Table                                      |
+-------+---------------------------------------------------+
| t1    | CREATE TABLE "t1" (
  "d" datetime DEFAULT NULL
) |
+-------+---------------------------------------------------+
```

Notice, `DATE` was translated to `DATETIME`.

This translation may cause some ambiguity. Suppose a user creates a table with a column of the traditional MariaDB `DATE` data type using the default sql_mode, but then switches to [SQL_MODE=ORACLE](/kb/en/sql_modeoracle/) and runs a [SHOW CREATE TABLE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table/) statement:

```sql
SET sql_mode=DEFAULT;
CREATE OR REPLACE TABLE t1 (
  d DATE
);
SET SQL_mode=ORACLE;
SHOW CREATE TABLE t1;
```

Before `mariadb_schema` was introduced, the above script displayed:

```sql
CREATE TABLE "t1" (
  "d" date DEFAULT NULL
);
```

which had two problems:

- It was confusing for the reader: its not clear if it is the traditional MariaDB `DATE`, or is it Oracle-alike date (which is actually `DATETIME`);
- It broke replication and caused data type mismatch on the master and on the slave (see [MDEV-19632](https://jira.mariadb.org/browse/MDEV-19632)).

To address this problem, starting from the mentioned versions, MariaDB uses the idea of qualified data types:

```sql
SET sql_mode=DEFAULT;
CREATE OR REPLACE TABLE t1 (
  d DATE
);
SET SQL_mode=ORACLE;
SHOW CREATE TABLE t1;
+-------+--------------------------------------------------------------+
| Table | Create Table                                                 |
+-------+--------------------------------------------------------------+
| t1    | CREATE TABLE "t1" (
  "d" mariadb_schema.date DEFAULT NULL
) |
+-------+--------------------------------------------------------------+
```