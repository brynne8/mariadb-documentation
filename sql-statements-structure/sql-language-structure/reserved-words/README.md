# Reserved Words

The following is a list of all reserved words in MariaDB.

Reserved words cannot be used as [Identifiers](/sql-statements-structure/sql-language-structure/identifier-names/), unless they are quoted.

The definitive list of reserved words for each version can be found by examining the  `sql/lex.h` and `sql/sql_yacc.yy` files.

## Reserved Words

<table><tbody><tr><th>Keyword</th><th>Notes</th></tr>
<tr><td>ACCESSIBLE</td><td></td></tr>
<tr><td>ADD</td><td></td></tr>
<tr><td>ALL</td><td></td></tr>
<tr><td>ALTER</td><td></td></tr>
<tr><td>ANALYZE</td><td></td></tr>
<tr><td>AND</td><td></td></tr>
<tr><td>AS</td><td></td></tr>
<tr><td>ASC</td><td></td></tr>
<tr><td>ASENSITIVE</td><td></td></tr>
<tr><td>BEFORE</td><td></td></tr>
<tr><td>BETWEEN</td><td></td></tr>
<tr><td>BIGINT</td><td></td></tr>
<tr><td>BINARY</td><td></td></tr>
<tr><td>BLOB</td><td></td></tr>
<tr><td>BOTH</td><td></td></tr>
<tr><td>BY</td><td></td></tr>
<tr><td>CALL</td><td></td></tr>
<tr><td>CASCADE</td><td></td></tr>
<tr><td>CASE</td><td></td></tr>
<tr><td>CHANGE</td><td></td></tr>
<tr><td>CHAR</td><td></td></tr>
<tr><td>CHARACTER</td><td></td></tr>
<tr><td>CHECK</td><td></td></tr>
<tr><td>COLLATE</td><td></td></tr>
<tr><td>COLUMN</td><td></td></tr>
<tr><td>CONDITION</td><td></td></tr>
<tr><td>CONSTRAINT</td><td></td></tr>
<tr><td>CONTINUE</td><td></td></tr>
<tr><td>CONVERT</td><td></td></tr>
<tr><td>CREATE</td><td></td></tr>
<tr><td>CROSS</td><td></td></tr>
<tr><td>CURRENT_DATE</td><td></td></tr>
<tr><td>CURRENT_ROLE</td><td>Added in <a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td>CURRENT_TIME</td><td></td></tr>
<tr><td>CURRENT_TIMESTAMP</td><td></td></tr>
<tr><td>CURRENT_USER</td><td></td></tr>
<tr><td>CURSOR</td><td></td></tr>
<tr><td>DATABASE</td><td></td></tr>
<tr><td>DATABASES</td><td></td></tr>
<tr><td>DAY_HOUR</td><td></td></tr>
<tr><td>DAY_MICROSECOND</td><td></td></tr>
<tr><td>DAY_MINUTE</td><td></td></tr>
<tr><td>DAY_SECOND</td><td></td></tr>
<tr><td>DEC</td><td></td></tr>
<tr><td>DECIMAL</td><td></td></tr>
<tr><td>DECLARE</td><td></td></tr>
<tr><td>DEFAULT</td><td></td></tr>
<tr><td>DELAYED</td><td></td></tr>
<tr><td>DELETE</td><td></td></tr>
<tr><td>DESC</td><td></td></tr>
<tr><td>DESCRIBE</td><td></td></tr>
<tr><td>DETERMINISTIC</td><td></td></tr>
<tr><td>DISTINCT</td><td></td></tr>
<tr><td>DISTINCTROW</td><td></td></tr>
<tr><td>DIV</td><td></td></tr>
<tr><td>DO_DOMAIN_IDS</td><td>Added in <a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a></td></tr>
<tr><td>DOUBLE</td><td></td></tr>
<tr><td>DROP</td><td></td></tr>
<tr><td>DUAL</td><td></td></tr>
<tr><td>EACH</td><td></td></tr>
<tr><td>ELSE</td><td></td></tr>
<tr><td>ELSEIF</td><td></td></tr>
<tr><td>ENCLOSED</td></tr>
<tr><td>ESCAPED</td><td></td></tr>
<tr><td>EXCEPT</td><td>Added in <a href="/kb/en/mariadb-1030-release-notes/">MariaDB 10.3.0</a></td></tr>
<tr><td>EXISTS</td><td></td></tr>
<tr><td>EXIT</td><td></td></tr>
<tr><td>EXPLAIN</td><td></td></tr>
<tr><td>FALSE</td><td></td></tr>
<tr><td>FETCH</td><td></td></tr>
<tr><td>FLOAT</td><td></td></tr>
<tr><td>FLOAT4</td><td></td></tr>
<tr><td>FLOAT8</td><td></td></tr>
<tr><td>FOR</td><td></td></tr>
<tr><td>FORCE</td><td></td></tr>
<tr><td>FOREIGN</td><td></td></tr>
<tr><td>FROM</td></tr>
<tr><td>FULLTEXT</td><td></td></tr>
<tr><td>GENERAL</td><td></td></tr>
<tr><td>GRANT</td><td></td></tr>
<tr><td>GROUP</td><td></td></tr>
<tr><td>HAVING</td><td></td></tr>
<tr><td>HIGH_PRIORITY</td><td></td></tr>
<tr><td>HOUR_MICROSECOND</td><td></td></tr>
<tr><td>HOUR_MINUTE</td><td></td></tr>
<tr><td>HOUR_SECOND</td><td></td></tr>
<tr><td>IF</td><td></td></tr>
<tr><td>IGNORE</td><td></td></tr>
<tr><td>IGNORE_DOMAIN_IDS</td><td>Added in <a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a></td></tr>
<tr><td>IGNORE_SERVER_IDS</td><td></td></tr>
<tr><td>IN</td><td></td></tr>
<tr><td>INDEX</td><td></td></tr>
<tr><td>INFILE</td><td></td></tr>
<tr><td>INNER</td><td></td></tr>
<tr><td>INOUT</td><td></td></tr>
<tr><td>INSENSITIVE</td><td></td></tr>
<tr><td>INSERT</td><td></td></tr>
<tr><td>INT</td><td></td></tr>
<tr><td>INT1</td><td></td></tr>
<tr><td>INT2</td><td></td></tr>
<tr><td>INT3</td><td></td></tr>
<tr><td>INT4</td><td></td></tr>
<tr><td>INT8</td><td></td></tr>
<tr><td>INTEGER</td><td></td></tr>
<tr><td>INTERSECT</td><td>Added in <a href="/kb/en/mariadb-1030-release-notes/">MariaDB 10.3.0</a></td></tr>
<tr><td>INTERVAL</td><td></td></tr>
<tr><td>INTO</td><td></td></tr>
<tr><td>IS</td><td></td></tr>
<tr><td>ITERATE</td><td></td></tr>
<tr><td>JOIN</td><td></td></tr>
<tr><td>KEY</td><td></td></tr>
<tr><td>KEYS</td><td></td></tr>
<tr><td>KILL</td><td></td></tr>
<tr><td>LEADING</td><td></td></tr>
<tr><td>LEAVE</td><td></td></tr>
<tr><td>LEFT</td><td></td></tr>
<tr><td>LIKE</td><td></td></tr>
<tr><td>LIMIT</td><td></td></tr>
<tr><td>LINEAR</td><td></td></tr>
<tr><td>LINES</td><td></td></tr>
<tr><td>LOAD</td><td></td></tr>
<tr><td>LOCALTIME</td><td></td></tr>
<tr><td>LOCALTIMESTAMP</td><td></td></tr>
<tr><td>LOCK</td><td></td></tr>
<tr><td>LONG</td><td></td></tr>
<tr><td>LONGBLOB</td><td></td></tr>
<tr><td>LONGTEXT</td><td></td></tr>
<tr><td>LOOP</td><td></td></tr>
<tr><td>LOW_PRIORITY</td><td></td></tr>
<tr><td>MASTER_HEARTBEAT_PERIOD</td><td></td></tr>
<tr><td>MASTER_SSL_VERIFY_SERVER_CERT</td><td></td></tr>
<tr><td>MATCH</td><td></td></tr>
<tr><td>MAXVALUE</td><td></td></tr>
<tr><td>MEDIUMBLOB</td><td></td></tr>
<tr><td>MEDIUMINT</td><td></td></tr>
<tr><td>MEDIUMTEXT</td><td></td></tr>
<tr><td>MIDDLEINT</td><td></td></tr>
<tr><td>MINUTE_MICROSECOND</td><td></td></tr>
<tr><td>MINUTE_SECOND</td><td></td></tr>
<tr><td>MOD</td><td></td></tr>
<tr><td>MODIFIES</td><td></td></tr>
<tr><td>NATURAL</td><td></td></tr>
<tr><td>NOT</td><td></td></tr>
<tr><td>NO_WRITE_TO_BINLOG</td><td></td></tr>
<tr><td>NULL</td><td></td></tr>
<tr><td>NUMERIC</td><td></td></tr>
<tr><td>ON</td><td></td></tr>
<tr><td>OPTIMIZE</td><td></td></tr>
<tr><td>OPTION</td><td></td></tr>
<tr><td>OPTIONALLY</td><td></td></tr>
<tr><td>OR</td><td></td></tr>
<tr><td>ORDER</td><td></td></tr>
<tr><td>OUT</td><td></td></tr>
<tr><td>OUTER</td><td></td></tr>
<tr><td>OUTFILE</td><td></td></tr>
<tr><td>OVER</td><td>Added in <a href="/kb/en/mariadb-1020-release-notes/">MariaDB 10.2.0</a></td></tr>
<tr><td>PAGE_CHECKSUM</td><td></td></tr>
<tr><td>PARSE_VCOL_EXPR</td><td></td></tr>
<tr><td>PARTITION</td><td>Added in <a href="/kb/en/what-is-mariadb-100/">MariaDB 10.0</a></td></tr>
<tr><td>POSITION</td><td></td></tr>
<tr><td>PRECISION</td><td></td></tr>
<tr><td>PRIMARY</td><td></td></tr>
<tr><td>PROCEDURE</td><td></td></tr>
<tr><td>PURGE</td><td></td></tr>
<tr><td>RANGE</td><td></td></tr>
<tr><td>READ</td><td></td></tr>
<tr><td>READS</td><td></td></tr>
<tr><td>READ_WRITE</td><td></td></tr>
<tr><td>REAL</td></tr>
<tr><td>RECURSIVE</td><td>Added in <a href="/kb/en/mariadb-1020-release-notes/">MariaDB 10.2.0</a></td></tr>
<tr><td>REF_SYSTEM_ID</td><td>Added in <a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a></td></tr>
<tr><td>REFERENCES</td><td></td></tr>
<tr><td>REGEXP</td><td></td></tr>
<tr><td>RELEASE</td><td></td></tr>
<tr><td>RENAME</td><td></td></tr>
<tr><td>REPEAT</td><td></td></tr>
<tr><td>REPLACE</td><td></td></tr>
<tr><td>REQUIRE</td><td></td></tr>
<tr><td>RESIGNAL</td><td></td></tr>
<tr><td>RESTRICT</td><td></td></tr>
<tr><td>RETURN</td><td></td></tr>
<tr><td>RETURNING</td><td>Added in <a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td>REVOKE</td><td></td></tr>
<tr><td>RIGHT</td><td></td></tr>
<tr><td>RLIKE</td><td></td></tr>
<tr><td>ROWS</td><td>Added in <a href="/kb/en/mariadb-1024-release-notes/">MariaDB 10.2.4</a></td></tr>
<tr><td>SCHEMA</td><td></td></tr>
<tr><td>SCHEMAS</td><td></td></tr>
<tr><td>SECOND_MICROSECOND</td><td></td></tr>
<tr><td>SELECT</td><td></td></tr>
<tr><td>SENSITIVE</td><td></td></tr>
<tr><td>SEPARATOR</td><td></td></tr>
<tr><td>SET</td><td></td></tr>
<tr><td>SHOW</td><td></td></tr>
<tr><td>SIGNAL</td><td></td></tr>
<tr><td>SLOW</td><td></td></tr>
<tr><td>SMALLINT</td><td></td></tr>
<tr><td>SPATIAL</td><td></td></tr>
<tr><td>SPECIFIC</td><td></td></tr>
<tr><td>SQL</td><td></td></tr>
<tr><td>SQLEXCEPTION</td><td></td></tr>
<tr><td>SQLSTATE</td><td></td></tr>
<tr><td>SQLWARNING</td><td></td></tr>
<tr><td>SQL_BIG_RESULT</td><td></td></tr>
<tr><td>SQL_CALC_FOUND_ROWS</td><td></td></tr>
<tr><td>SQL_SMALL_RESULT</td><td></td></tr>
<tr><td>SSL</td><td></td></tr>
<tr><td>STARTING</td><td></td></tr>
<tr><td>STATS_AUTO_RECALC</td><td>Added in <a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td>STATS_PERSISTENT</td><td>Added in <a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td>STATS_SAMPLE_PAGES</td><td>Added in <a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td></tr>
<tr><td>STRAIGHT_JOIN</td><td></td></tr>
<tr><td>TABLE</td><td></td></tr>
<tr><td>TERMINATED</td><td></td></tr>
<tr><td>THEN</td><td></td></tr>
<tr><td>TINYBLOB</td><td></td></tr>
<tr><td>TINYINT</td><td></td></tr>
<tr><td>TINYTEXT</td><td></td></tr>
<tr><td>TO</td><td></td></tr>
<tr><td>TRAILING</td><td></td></tr>
<tr><td>TRIGGER</td><td></td></tr>
<tr><td>TRUE</td><td></td></tr>
<tr><td>UNDO</td><td></td></tr>
<tr><td>UNION</td><td></td></tr>
<tr><td>UNIQUE</td><td></td></tr>
<tr><td>UNLOCK</td><td></td></tr>
<tr><td>UNSIGNED</td><td></td></tr>
<tr><td>UPDATE</td><td></td></tr>
<tr><td>USAGE</td><td></td></tr>
<tr><td>USE</td><td></td></tr>
<tr><td>USING</td><td></td></tr>
<tr><td>UTC_DATE</td><td></td></tr>
<tr><td>UTC_TIME</td><td></td></tr>
<tr><td>UTC_TIMESTAMP</td><td></td></tr>
<tr><td>VALUES</td><td></td></tr>
<tr><td>VARBINARY</td><td></td></tr>
<tr><td>VARCHAR</td><td></td></tr>
<tr><td>VARCHARACTER</td><td></td></tr>
<tr><td>VARYING</td><td></td></tr>
<tr><td>WHEN</td><td></td></tr>
<tr><td>WHERE</td><td></td></tr>
<tr><td>WHILE</td><td></td></tr>
<tr><td>WINDOW</td><td>Added in <a href="/kb/en/mariadb-1020-release-notes/">MariaDB 10.2.0</a>. From <a href="/kb/en/mariadb-10212-release-notes/">MariaDB 10.2.12</a> only disallowed for table aliases.</td></tr>
<tr><td>WITH</td><td></td></tr>
<tr><td>WRITE</td><td></td></tr>
<tr><td>XOR</td><td></td></tr>
<tr><td>YEAR_MONTH</td><td></td></tr>
<tr><td>ZEROFILL</td><td></td></tr>
</tbody></table>

## Exceptions

Some keywords are exceptions for historical reasons, and are permitted as unquoted identifiers. These include:

<table><tbody><tr><th>Keyword</th></tr>
<tr><td>ACTION</td></tr>
<tr><td><a href="/kb/en/bit/">BIT</a></td></tr>
<tr><td><a href="/kb/en/date/">DATE</a></td></tr>
<tr><td><a href="/kb/en/enum/">ENUM</a></td></tr>
<tr><td>NO</td></tr>
<tr><td><a href="/kb/en/text/">TEXT</a></td></tr>
<tr><td><a href="/kb/en/time/">TIME</a></td></tr>
<tr><td><a href="/kb/en/timestamp/">TIMESTAMP</a></td></tr>
</tbody></table>

## Oracle Mode

In [Oracle mode, from MariaDB 10.3](/kb/en/sql_modeoracle-from-mariadb-103/), there are a number of extra reserved words:

<table><tbody><tr><th>Keyword</th><th>Notes</th></tr>
<tr><td>BODY</td><td></td></tr>
<tr><td>ELSIF</td><td></td></tr>
<tr><td>GOTO</td><td></td></tr>
<tr><td>HISTORY</td><td>&lt;= <a href="/kb/en/mariadb-1036-release-notes/">MariaDB 10.3.6</a> only</td></tr>
<tr><td>OTHERS</td><td></td></tr>
<tr><td>PACKAGE</td></tr>
<tr><td>PERIOD</td><td>&lt;= <a href="/kb/en/mariadb-1036-release-notes/">MariaDB 10.3.6</a> only</td></tr>
<tr><td>RAISE</td><td></td></tr>
<tr><td>ROWTYPE</td><td></td></tr>
<tr><td>SYSTEM</td><td>&lt;= <a href="/kb/en/mariadb-1036-release-notes/">MariaDB 10.3.6</a> only</td></tr>
<tr><td>SYSTEM_TIME</td><td>&lt;= <a href="/kb/en/mariadb-1036-release-notes/">MariaDB 10.3.6</a> only</td></tr>
<tr><td>VERSIONING</td><td>&lt;= <a href="/kb/en/mariadb-1036-release-notes/">MariaDB 10.3.6</a> only</td></tr>
<tr><td>WITHOUT</td><td>&lt;= <a href="/kb/en/mariadb-1036-release-notes/">MariaDB 10.3.6</a> only</td></tr>
</tbody></table>

## Function Names

If the `IGNORE_SPACE` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode/) flag is set, function names become reserved words.