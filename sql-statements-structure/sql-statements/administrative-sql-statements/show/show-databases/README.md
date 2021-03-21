# SHOW DATABASES

## Syntax

```sql
SHOW {DATABASES | SCHEMAS}
    [LIKE 'pattern' | WHERE expr]
```

## Description

<code class="highlight fixed" style="white-space:pre-wrap">SHOW DATABASES</code> lists the databases on the MariaDB server host.
<code class="highlight fixed" style="white-space:pre-wrap">SHOW SCHEMAS</code> is a synonym for 
<code class="highlight fixed" style="white-space:pre-wrap">SHOW DATABASES</code>. The <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clause, if
present on its own, indicates which database names to match. The <code class="highlight fixed" style="white-space:pre-wrap">WHERE</code> and <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clauses can be given to select rows using more general conditions, as discussed in [Extended SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/extended-show/).

You see only those databases for which you have some kind of
privilege, unless you have the global 
[SHOW DATABASES privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/). You
can also get this list using the [mysqlshow](/clients-utilities/mysqlshow/) command.

If the server was started with the <code class="highlight fixed" style="white-space:pre-wrap">--skip-show-database</code>
option, you cannot use this statement at all unless you have the
[SHOW DATABASES privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/).

The list of results returned by `SHOW DATABASES` is based on directories in the data directory, which is how MariaDB implements databases. It's possible that output includes directories that do not correspond to actual databases.

The [Information Schema SCHEMATA table](/kb/en/information-schema-schemata-table/) also contains database information.

## Examples

```sql
SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| test               |
+--------------------+
```

```sql
SHOW DATABASES LIKE 'm%';
+---------------+
| Database (m%) |
+---------------+
| mysql         |
+---------------+
```

## See Also

- [CREATE DATABASE](/sql-statements-structure/sql-statements/data-definition/create/create-database/)
- [ALTER DATABASE](/sql-statements-structure/sql-statements/data-definition/alter/alter-database/)
- [DROP DATABASE](/sql-statements-structure/sql-statements/data-definition/drop/drop-database/)
- [SHOW CREATE DATABASE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-database/)
- [Character Sets and Collations](/kb/en/character-sets-and-collations/)
- [Information Schema SCHEMATA Table](/kb/en/information-schema-schemata-table/)