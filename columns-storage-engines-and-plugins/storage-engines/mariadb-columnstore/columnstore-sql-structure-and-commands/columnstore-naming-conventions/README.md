# ColumnStore Naming Conventions

This lists the different naming conventions enforced by the column store, compared to the normal [MariaDB naming conventions](https://mariadb.com/kb/en/mariadb/identifier-names).

- User names: 64 characters (MariaDB has 80)
- Table and column names are restricted to alphanumeric and underscore only, i.e "A-Z a-z 0-9 _".
- The first character of all table and column names should be an ASCII  letter (a-z A-Z).
- ColumnStore reserves certain words that MariaDB does not, such as SELECT, CHAR and TABLE, so even wrapped in backticks these cannot be used.

## Reserved words

In addition to MariaDB Server [reserved words](/sql-statements-structure/sql-language-structure/reserved-words/), ColumnStore has additional reserved words that cannot be used as table names, column names or user defined variables, functions or stored procedure names.

<table><tbody><tr><th>Keyword</th></tr>
<tr><td>ACTION</td></tr>
<tr><td>ADD</td></tr>
<tr><td>ALTER</td></tr>
<tr><td>AUTO_INCREMENT</td></tr>
<tr><td>BIGINT</td></tr>
<tr><td>BIT</td></tr>
<tr><td>CASCADE</td></tr>
<tr><td>CHANGE</td></tr>
<tr><td>CHARACTER</td></tr>
<tr><td>CHARSET</td></tr>
<tr><td>CHECK</td></tr>
<tr><td>CLOB</td></tr>
<tr><td>COLUMN</td></tr>
<tr><td>COLUMNS</td></tr>
<tr><td>COMMENT</td></tr>
<tr><td>CONSTRAINT</td></tr>
<tr><td>CONSTRAINTS</td></tr>
<tr><td>CREATE</td></tr>
<tr><td>CURRENT_USER</td></tr>
<tr><td>DATETIME</td></tr>
<tr><td>DEC</td></tr>
<tr><td>DECIMAL</td></tr>
<tr><td>DEFERRED</td></tr>
<tr><td>DEFAULT</td></tr>
<tr><td>DEFERRABLE</td></tr>
<tr><td>DOUBLE</td></tr>
<tr><td>DROP</td></tr>
<tr><td>ENGINE</td></tr>
<tr><td>EXISTS</td></tr>
<tr><td>FOREIGN</td></tr>
<tr><td>FULL</td></tr>
<tr><td>IDB_BLOB</td></tr>
<tr><td>IDB_CHAR</td></tr>
<tr><td>IDB_DELETE</td></tr>
<tr><td>IDB_FLOAT</td></tr>
<tr><td>IDB_INT</td></tr>
<tr><td>IF</td></tr>
<tr><td>IMMEDIATE</td></tr>
<tr><td>INDEX</td></tr>
<tr><td>INITIALLY</td></tr>
<tr><td>INTEGER</td></tr>
<tr><td>KEY</td></tr>
<tr><td>MATCH</td></tr>
<tr><td>MAX_ROWS</td></tr>
<tr><td>MIN_ROWS</td></tr>
<tr><td>MODIFY</td></tr>
<tr><td>NO</td></tr>
<tr><td>NOT</td></tr>
<tr><td>NULL_TOK</td></tr>
<tr><td>NUMBER</td></tr>
<tr><td>NUMERIC</td></tr>
<tr><td>ON</td></tr>
<tr><td>PARTIAL</td></tr>
<tr><td>PRECISION</td></tr>
<tr><td>PRIMARY</td></tr>
<tr><td>REAL</td></tr>
<tr><td>REFERENCES</td></tr>
<tr><td>RENAME</td></tr>
<tr><td>RESTRICT</td></tr>
<tr><td>SESSION_USER</td></tr>
<tr><td>SET</td></tr>
<tr><td>SMALLINT</td></tr>
<tr><td>SYSTEM_USER</td></tr>
<tr><td>TABLE</td></tr>
<tr><td>TIME</td></tr>
<tr><td>TINYINT</td></tr>
<tr><td>TO</td></tr>
<tr><td>TRUNCATE</td></tr>
<tr><td>UNIQUE</td></tr>
<tr><td>UNSIGNED</td></tr>
<tr><td>UPDATE</td></tr>
<tr><td>USER</td></tr>
<tr><td>VARBINARY</td></tr>
<tr><td>VARCHAR</td></tr>
<tr><td>VARYING</td></tr>
<tr><td>WITH</td></tr>
<tr><td>ZONE</td></tr>
</tbody></table>