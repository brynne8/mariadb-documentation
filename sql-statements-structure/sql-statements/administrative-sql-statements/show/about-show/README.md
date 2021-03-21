# About SHOW

<code class="highlight fixed" style="white-space:pre-wrap">SHOW</code> has many forms that provide information about
databases, tables, columns, or status information about the server. These include:

- [SHOW AUTHORS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-authors)
- [SHOW CHARACTER SET [like_or_where]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-character-set)
- [SHOW COLLATION [like_or_where]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-collation)
- [SHOW [FULL] COLUMNS FROM tbl_name [FROM db_name] [like_or_where]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-columns)
- [SHOW CONTRIBUTORS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-contributors)
- [SHOW CREATE DATABASE db_name](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-database)
- [SHOW CREATE EVENT event_name](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-event)
- [SHOW CREATE PACKAGE package_name](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-package)
- [SHOW CREATE PACKAGE BODY package_name](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-package-body)
- [SHOW CREATE PROCEDURE proc_name](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-procedure)
- [SHOW CREATE TABLE tbl_name](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-table)
- [SHOW CREATE TRIGGER trigger_name](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-trigger)
- [SHOW CREATE VIEW view_name](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-create-view)
- [SHOW DATABASES [like_or_where]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-databases)
- [SHOW ENGINE engine_name {STATUS | MUTEX}](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engine)
- [SHOW [STORAGE] ENGINES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-engines)
- [SHOW ERRORS [LIMIT [offset,] row_count]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-errors)
- [SHOW [FULL] EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-events)
- [SHOW FUNCTION CODE func_name](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-function-code)
- [SHOW FUNCTION STATUS [like_or_where]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-function-status)
- [SHOW GRANTS FOR user](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-grants)
- [SHOW INDEX FROM tbl_name [FROM db_name]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-index)
- [SHOW INNODB STATUS](/kb/en/show-innodb-status/)
- [SHOW OPEN TABLES [FROM db_name] [like_or_where]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-open-tables)
- [SHOW PLUGINS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins)
- [SHOW PROCEDURE CODE proc_name](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-procedure-code)
- [SHOW PROCEDURE STATUS [like_or_where]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-procedure-status)
- [SHOW PRIVILEGES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-privileges)
- [SHOW [FULL] PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist)
- [SHOW PROFILE [types] [FOR QUERY n] [OFFSET n] [LIMIT n]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-profile)
- [SHOW PROFILES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-profiles)
- [SHOW [GLOBAL | SESSION] STATUS [like_or_where]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status)
- [SHOW TABLE STATUS [FROM db_name] [like_or_where]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status)
- [SHOW TABLES [FROM db_name] [like_or_where]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-tables)
- [SHOW TRIGGERS [FROM db_name] [like_or_where]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-triggers)
- [SHOW [GLOBAL | SESSION] VARIABLES [like_or_where]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables)
- [SHOW WARNINGS [LIMIT [offset,] row_count]](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings)

```sql
like_or_where:
    LIKE 'pattern'
  | WHERE expr
```

If the syntax for a given <code class="highlight fixed" style="white-space:pre-wrap">SHOW</code> statement includes a
<code class="highlight fixed" style="white-space:pre-wrap">LIKE 'pattern'</code> part, <code class="highlight fixed" style="white-space:pre-wrap">'pattern'</code> is a
string that can contain the SQL "<code class="highlight fixed" style="white-space:pre-wrap">%</code>" and
"<code class="highlight fixed" style="white-space:pre-wrap">_</code>" wildcard characters. The pattern is useful for
restricting statement output to matching values.

Several <code class="highlight fixed" style="white-space:pre-wrap">SHOW</code> statements also accept a
<code class="highlight fixed" style="white-space:pre-wrap">WHERE</code> clause that provides more flexibility in specifying
which rows to display. See [Extended Show](/sql-statements-structure/sql-statements/administrative-sql-statements/show/extended-show).