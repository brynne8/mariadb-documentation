# USE

## Syntax

```sql
USE db_name
```

## Description

The <code class="highlight fixed" style="white-space:pre-wrap">'USE db_name'</code> statement tells MariaDB to use the
<code class="highlight fixed" style="white-space:pre-wrap">db_name</code> database as the default (current) database for
subsequent statements. The database remains the default until the end of the
session or another <code class="highlight fixed" style="white-space:pre-wrap">USE</code> statement is issued:

```sql
USE db1;
SELECT COUNT(*) FROM mytable;   # selects from db1.mytable
USE db2;
SELECT COUNT(*) FROM mytable;   # selects from db2.mytable
```

The [DATABASE()](/built-in-functions/secondary-functions/information-functions/database) function ([SCHEMA()](/built-in-functions/secondary-functions/information-functions/schema) is a synonym) returns the default database.

Another way to set the default database is specifying its name at [mysql](/clients-utilities/mysql-client/mysql-command-line-client) command line client startup.

## See Also

- [Identifier Qualifiers](/sql-statements-structure/sql-language-structure/identifier-qualifiers)