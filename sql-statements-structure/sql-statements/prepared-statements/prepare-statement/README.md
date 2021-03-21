# PREPARE Statement

## Syntax

```sql
PREPARE stmt_name FROM preparable_stmt
```

## Description

The <code class="fixed" style="white-space:pre-wrap">PREPARE</code> statement prepares a statement and assigns it a name,
<code class="fixed" style="white-space:pre-wrap">stmt_name</code>, by which to refer to the statement later. Statement names
are not case sensitive. <code class="fixed" style="white-space:pre-wrap">preparable_stmt</code> is either a string literal or a [user variable](/sql-statements-structure/sql-language-structure/user-defined-variables/) (not a [local variable](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable/), an SQL expression or a subquery) that contains the text of the statement. The text must   
represent a single SQL statement, not multiple statements. Within the
statement, "?" characters can be used as parameter markers to indicate
where data values are to be bound to the query later when you execute
it. The "?" characters should not be enclosed within quotes, even if
you intend to bind them to string values. Parameter markers can be used
only where expressions should appear, not for SQL keywords,
identifiers, and so forth.

The scope of a prepared statement is the session within which it is
created. Other sessions cannot see it.

If a prepared statement with the given name already exists, it is
deallocated implicitly before the new statement is prepared. This means
that if the new statement contains an error and cannot be prepared, an
error is returned and no statement with the given name exists.

Prepared statements can be PREPAREd and EXECUTEd in a stored procedure, but not in a stored function or trigger. Also, even if the statement is PREPAREd in a procedure, it will not be deallocated when the procedure execution ends.

A prepared statement can access [user-defined variables](/sql-statements-structure/sql-language-structure/user-defined-variables/), but not [local variables](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable/) or procedure's parameters.

If the prepared statement contains a syntax error, PREPARE will fail. As a side effect, stored procedures can use it to check if a statement is valid. For example:

```sql
CREATE PROCEDURE `test_stmt`(IN sql_text TEXT)
BEGIN
        DECLARE EXIT HANDLER FOR SQLEXCEPTION
        BEGIN
                SELECT CONCAT(sql_text, ' is not valid');
        END;
        SET @SQL := sql_text;
        PREPARE stmt FROM @SQL;
        DEALLOCATE PREPARE stmt;
END;
```

The [FOUND_ROWS()](/built-in-functions/secondary-functions/information-functions/found_rows/) and [ROW_COUNT()](/kb/en/information-functions-row_count/) functions, if called immediatly after EXECUTE, return the number of rows read or affected by the prepared statements; however, if they are called after DEALLOCATE PREPARE, they provide information about this statement. If the prepared statement produces errors or warnings, [GET DIAGNOSTICS](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/get-diagnostics/) return information about them. DEALLOCATE PREPARE shouldn't clear the [diagnostics area](/programming-customizing-mariadb/programmatic-compound-statements/programmatic-compound-statements-diagnostics/diagnostics-area/), unless it produces an error.

A prepared statement is executed with [EXECUTE](/sql-statements-structure/sql-statements/prepared-statements/execute-statement/) and released 
with <a undefined>DEALLOCATE PREPARE</a>.

The [max_prepared_stmt_count](/kb/en/server-system-variables/#max_prepared_stmt_count) server system variable determines the number of allowed prepared statements that can be prepared on the server. If it is set to 0, prepared statements are not allowed. If the limit is reached, an error similar to the following will be produced:

```sql
ERROR 1461 (42000): Can't create more than max_prepared_stmt_count statements 
  (current value: 0)
```

### Oracle Mode

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

In [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#prepared-statements), `PREPARE stmt FROM 'SELECT :1, :2'` is used, instead of `?`.

## Permitted Statements

Not all statements can be prepared. Only the following SQL commands are permitted:

- [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/)
- [ANALYZE TABLE](/sql-statements-structure/sql-statements/table-statements/analyze-table/)
- [BINLOG](/sql-statements-structure/sql-statements/administrative-sql-statements/binlog/)
- [CACHE INDEX](/sql-statements-structure/sql-statements/administrative-sql-statements/cache-index/)
- [CALL](/sql-statements-structure/sql-statements/stored-routine-statements/call/)
- [CHANGE MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/change-master-to/)
- [CHECKSUM {TABLE | TABLES}](/sql-statements-structure/sql-statements/table-statements/checksum-table/)
- [COMMIT](/sql-statements-structure/sql-statements/transactions/commit/)
- {[CREATE](/sql-statements-structure/sql-statements/data-definition/create/create-database/) | [DROP](/sql-statements-structure/sql-statements/data-definition/drop/drop-database/)} DATABASE
- {[CREATE](/sql-statements-structure/sql-statements/data-definition/create/create-index/) | [DROP](/sql-statements-structure/sql-statements/data-definition/drop/drop-index/)} INDEX
- {[CREATE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) | [RENAME](/sql-statements-structure/sql-statements/data-definition/rename-table/) | [DROP](/sql-statements-structure/sql-statements/data-definition/drop/drop-table/)} TABLE
- {[CREATE](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user/) | [RENAME](/sql-statements-structure/sql-statements/account-management-sql-commands/rename-user/) | [DROP](/sql-statements-structure/sql-statements/account-management-sql-commands/drop-user/)} USER
- {[CREATE](/programming-customizing-mariadb/views/create-view/) | [DROP](/programming-customizing-mariadb/views/drop-view/)} VIEW
- [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/)
- [DESCRIBE](/sql-statements-structure/sql-statements/administrative-sql-statements/describe/)
- [DO](/sql-statements-structure/sql-statements/stored-routine-statements/do/)
- [EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain/)
- [FLUSH](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) {TABLE | TABLES | TABLES WITH READ LOCK | HOSTS | PRIVILEGES | LOGS | STATUS | 
  MASTER | SLAVE | DES_KEY_FILE | USER_RESOURCES | [QUERY CACHE](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush-query-cache/) | TABLE_STATISTICS | 
  INDEX_STATISTICS | USER_STATISTICS | CLIENT_STATISTICS}
- [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/)
- [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/)
- INSTALL {[PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin/) | [SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname/)}
- [HANDLER READ](/sql-statements-structure/nosql/handler/handler-commands/)
- [KILL](/sql-statements-structure/sql-statements/administrative-sql-statements/kill/)
- [LOAD INDEX INTO CACHE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/load-data-into-tables-or-index/load-index/)
- [OPTIMIZE TABLE](/replication/optimization-and-tuning/optimizing-tables/optimize-table/)
- [REPAIR TABLE](/sql-statements-structure/sql-statements/table-statements/repair-table/)
- [REPLACE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace/)
- [RESET](/sql-statements-structure/sql-statements/administrative-sql-statements/reset/) {[MASTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/reset-master/) | [SLAVE](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/reset-slave-connection_name/) | [QUERY CACHE](/sql-statements-structure/sql-statements/administrative-sql-statements/reset/)}
- [REVOKE](/sql-statements-structure/sql-statements/account-management-sql-commands/revoke/)
- [ROLLBACK](/sql-statements-structure/sql-statements/transactions/rollback/)
- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)
- [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set/)
- [SET GLOBAL SQL_SLAVE_SKIP_COUNTER](/sql-statements-structure/sql-statements/administrative-sql-statements/replication-commands/set-global-sql_slave_skip_counter/)
- [SET ROLE](/sql-statements-structure/sql-statements/account-management-sql-commands/set-role/)
- [SET SQL_LOG_BIN](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set-sql_log_bin/)
- [SET TRANSACTION ISOLATION LEVEL](/sql-statements-structure/sql-statements/transactions/set-transaction/)
- [SHOW EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-explain/)
- SHOW {[DATABASES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-databases/) | [TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-tables/) | [OPEN TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-open-tables/) | [TABLE STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-status/) | [COLUMNS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-columns/) | [INDEX](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-index/) | [TRIGGERS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-triggers/) | 
  [EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-events/) | [GRANTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-grants/) | [CHARACTER SET](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-character-set/) | [COLLATION](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-collation/) | [ENGINES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-events/) | [PLUGINS [SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-plugins/)] | [PRIVILEGES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-privileges/) | 
  [PROCESSLIST](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-processlist/) | [PROFILE](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-profile/) | [PROFILES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-profiles/) | [VARIABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables/) | [STATUS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-status/) | [WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings/) | [ERRORS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-errors/) | 
  [TABLE_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-table-statistics/) | [INDEX_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-index-statistics/) | [USER_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-user-statistics/) | [CLIENT_STATISTICS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-client-statistics/) | [AUTHORS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-authors/) | 
  [CONTRIBUTORS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-contributors/)}
- SHOW CREATE {DATABASE | TABLE | VIEW | PROCEDURE | FUNCTION | TRIGGER | EVENT}
- SHOW {FUNCTION | PROCEDURE} CODE
- SHOW BINLOG EVENTS
- SHOW SLAVE HOSTS
- SHOW {MASTER | BINARY} LOGS
- SHOW {MASTER | SLAVE | TABLES | INNODB | FUNCTION | PROCEDURE} STATUS
- SLAVE {[START](/kb/en/start-slave/) | [STOP](/kb/en/stop-slave/)}
- [TRUNCATE TABLE](/sql-statements-structure/sql-statements/table-statements/truncate-table/)
- [SHUTDOWN](/sql-statements-structure/sql-statements/administrative-sql-statements/shutdown/)
- UNINSTALL {PLUGIN | SONAME}
- [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/)

Synonyms are not listed here, but can be used. For example, DESC can be used instead of DESCRIBE.

##### MariaDB starting with [10.1.1](/kb/en/mariadb-1011-release-notes/)

[Compound statements](/programming-customizing-mariadb/programmatic-compound-statements/using-compound-statements-outside-of-stored-programs/) can be prepared too.

Note that if a statement can be run in a stored routine, it will work even if it is called by a prepared statement. For example, [SIGNAL](/programming-customizing-mariadb/programmatic-compound-statements/signal/) can't be directly prepared. However, it is allowed in [stored routines](/programming-customizing-mariadb/stored-routines/). If the x() procedure contains SIGNAL, you can still prepare and execute the 'CALL x();' prepared statement.

##### MariaDB starting with [10.2.3](/kb/en/mariadb-1023-release-notes/)

PREPARE now supports most kinds of expressions as well, for example:

```sql
PREPARE stmt FROM CONCAT('SELECT * FROM ', table_name);
```

When PREPARE is used with a statement which is not supported, the following error is produced:

```sql
ERROR 1295 (HY000): This command is not supported in the prepared statement protocol yet
```

## Example

```sql
create table t1 (a int,b char(10));
insert into t1 values (1,"one"),(2, "two"),(3,"three");
prepare test from "select * from t1 where a=?";
set @param=2;
execute test using @param;
+------+------+
| a    | b    |
+------+------+
|    2 | two  |
+------+------+
set @param=3;
execute test using @param;
+------+-------+
| a    | b     |
+------+-------+
|    3 | three |
+------+-------+
deallocate prepare test;
```

Since identifiers are not permitted as prepared statements parameters, sometimes it is necessary to dynamically compose an SQL statement. This technique is called <em>dynamic SQL</em>). The following example shows how to use dynamic SQL:

```sql
CREATE PROCEDURE test.stmt_test(IN tab_name VARCHAR(64))
BEGIN
	SET @sql = CONCAT('SELECT COUNT(*) FROM ', tab_name);
	PREPARE stmt FROM @sql;
	EXECUTE stmt;
	DEALLOCATE PREPARE stmt;
END;

CALL test.stmt_test('mysql.user');
+----------+
| COUNT(*) |
+----------+
|        4 |
+----------+
```

Use of variables in prepared statements:

```sql
PREPARE stmt FROM 'SELECT @x;';

SET @x = 1;

EXECUTE stmt;
+------+
| @x   |
+------+
|    1 |
+------+

SET @x = 0;

EXECUTE stmt;
+------+
| @x   |
+------+
|    0 |
+------+

DEALLOCATE PREPARE stmt;
```

## See Also

- [Out parameters in PREPARE](/sql-statements-structure/sql-statements/prepared-statements/out-parameters-in-prepare/)
- [EXECUTE Statement](/sql-statements-structure/sql-statements/prepared-statements/execute-statement/)
- [DEALLOCATE / DROP Prepared Statement](/kb/en/deallocate-drop-prepared-statement/)
- [EXECUTE IMMEDIATE](/sql-statements-structure/sql-statements/prepared-statements/execute-immediate/)
- [Oracle mode from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/#prepared-statements)