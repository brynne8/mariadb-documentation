# Creating and Dropping CONNECT Tables

Create Table statements for “CONNECT” tables are standard MariaDB create statements specifying
`engine=CONNECT`. There are a few additional table and column options specific to CONNECT.

### Table Options

<table><tbody><tr><th>Table Option</th><th>Type</th><th>Description</th></tr>
<tr><td><code>AVG_ROW_LENGTH</code></td><td>Integer</td><td>Can be specified to help CONNECT estimate the size of a variable record table length.</td></tr>
<tr><td><code>BLOCK_SIZE</code></td><td>Integer</td><td>The number of rows each block of a <a href="/kb/en/connect-dos-and-fix-table-types/">FIX</a>, <a href="/kb/en/connect-bin-table-type/">BIN</a>, <a href="/kb/en/connect-dbf-table-type/">DBF</a>, or <a href="/kb/en/connect-vec-table-type/">VEC</a> table contains. For an <a href="/kb/en/connect-table-types-odbc-table-type-accessing-tables-from-other-dbms/">ODBC</a> table this is the RowSet size option. For a <a href="/kb/en/connect-jdbc-table-type-accessing-tables-from-other-dbms/">JDBC</a> table this is the fetch size.</td></tr>
<tr><td><code>CATFUNC</code></td><td>String</td><td>The catalog function used by a <a href="/kb/en/connect-table-types-catalog-tables/">catalog table</a>.</td></tr>
<tr><td><code>COLIST</code></td><td>String</td><td>The column list of <a href="/kb/en/connect-table-types-occur-table-type/">OCCUR</a> tables or $project of  <a href="/kb/en/connect-mongo-table-type-accessing-collections-from-mongodb/">MONGO</a> tables.</td></tr>
<tr><td><code>COMPRESS</code></td><td>Number</td><td><code>1</code> or <code>2</code> if the data file is g-zip compressed. Defaults to 0. Before CONNECT 1.05.0001, this was boolean, and true if the data file is compressed.</td></tr>
<tr><td><code>CONNECTION</code></td><td>String</td><td>Specifies the connection of an <a href="/kb/en/connect-table-types-odbc-table-type-accessing-tables-from-other-dbms/">ODBC</a>, <a href="/kb/en/connect-jdbc-table-type-accessing-tables-from-other-dbms/">JDBC</a> or <a href="/kb/en/connect-table-types-mysql-table-type-accessing-mysqlmariadb-tables/">MYSQL</a> table.</td></tr>
<tr><td><code>DATA_CHARSET</code></td><td>String</td><td>The character set used in the external file or data source.</td></tr>
<tr><td><code>DBNAME</code></td><td>String</td><td>The target database for <a href="/kb/en/connect-table-types-odbc-table-type-accessing-tables-from-other-dbms/">ODBC</a>, <a href="/kb/en/connect-jdbc-table-type-accessing-tables-from-other-dbms/">JDBC</a>, <a href="/kb/en/connect-table-types-mysql-table-type-accessing-mysqlmariadb-tables/">MYSQL</a>, <a href="/kb/en/connect-table-types-catalog-tables/">catalog</a>, and <a href="/kb/en/connect-table-types-proxy-table-type/">PROXY</a> based tables. The database concept is sometimes known as a <em>schema</em>.</td></tr>
<tr><td><code>ENGINE</code></td><td>String</td><td>Must be specfied as <code>CONNECT</code>.</td></tr>
<tr><td><code>ENDING</code></td><td>Integer</td><td>End of line length. Defaults to <code>1</code> for Unix/Linux and <code>2</code> for Windows.</td></tr>
<tr><td><code>FILE_NAME</code></td><td>String</td><td>The file (path) name for all table types based on files. Can be absolute or relative to the current data directory. If not specified, this is an <a href="/kb/en/inward-and-outward-tables/#inward-tables">Inward table</a> and a default value is used.</td></tr>
<tr><td><code>FILTER</code></td><td>String</td><td>To filter an external table. Currently MONGO tables only.</td></tr>
<tr><td><code>HEADER</code></td><td>Integer</td><td>Applies to <a href="/kb/en/connect-csv-and-fmt-table-types/">CSV</a>, <a href="/kb/en/connect-vec-table-type/">VEC</a>, and HTML files. Its meaning depends on the table type.</td></tr>
<tr><td><code>HTTP</code></td><td>String</td><td>The HTTP of the client of REST queries. From <a href="/kb/en/connect/">Connect 1.06.0010</a>.</td></tr>
<tr><td><code>HUGE</code></td><td>Boolean</td><td>To specify that a table file can be larger than 2GB. For a <a href="/kb/en/connect-table-types-mysql-table-type-accessing-mysqlmariadb-tables/">MYSQL</a> table, prevents the result set from being stored in memory.</td></tr>
<tr><td><code>LRECL</code></td><td>Integer</td><td>The file record size (often calculated by default).</td></tr>
<tr><td><code>MAPPED</code></td><td>Boolean</td><td>Specifies whether <em>file mapping</em> is used to handle the table file.</td></tr>
<tr><td><code>MODULE</code></td><td>String</td><td>The (path) name of the DLL or shared lib implementing the access of a non-standard (<a href="/kb/en/connect-table-types-oem/">OEM</a>) table type.</td></tr>
<tr><td><code>MULTIPLE</code></td><td>Integer</td><td>Used to specify multiple file tables.</td></tr>
<tr><td><code>OPTION_LIST</code></td><td>String</td><td>Used to specify all other options not yet directly defined.</td></tr>
<tr><td><code>QCHAR</code></td><td>String</td><td>Specifies the character used for quoting some fields of a <a href="/kb/en/connect-csv-and-fmt-table-types/">CSV</a> table or the identifiers of an <a href="/kb/en/connect-table-types-odbc-table-type-accessing-tables-from-other-dbms/">ODBC</a>/<a href="/kb/en/connect-jdbc-table-type-accessing-tables-from-other-dbms/">JDBC</a> tables.</td></tr>
<tr><td><code>QUOTED</code></td><td>Integer</td><td>The level of quoting used in <a href="/kb/en/connect-csv-and-fmt-table-types/">CSV</a> table files.</td></tr>
<tr><td><code>READONLY</code></td><td>Boolean</td><td>True if the data file must not be modified or erased.</td></tr>
<tr><td><code>SEP_CHAR</code></td><td>String</td><td>Specifies the field separator character of a <a href="/kb/en/connect-csv-and-fmt-table-types/">CSV</a> or <a href="/kb/en/connect-table-types-xcol-table-type/">XCOL</a> table. Also, used to specify the Jpath separator for <a href="/kb/en/connect-json-table-type/">JSON tables</a>.</td></tr>
<tr><td><code>SEPINDEX</code></td><td>Boolean</td><td>When true, indexes are saved in separate files.</td></tr>
<tr><td><code>SPLIT</code></td><td>Boolean</td><td>True for a <a href="/kb/en/connect-vec-table-type/">VEC</a> table when all columns are in separate files.</td></tr>
<tr><td><code>SRCDEF</code></td><td>String</td><td>The source definition of a table retrieved via <a href="/kb/en/connect-table-types-odbc-table-type-accessing-tables-from-other-dbms/">ODBC</a>, <a href="/kb/en/connect-jdbc-table-type-accessing-tables-from-other-dbms/">JDBC</a> or the MySQL API or used by a <a href="/kb/en/connect-table-types-pivot-table-type/">PIVOT</a> table.</td></tr>
<tr><td><code>SUBTYPE</code></td><td>String</td><td>The subtype of an <a href="/kb/en/connect-table-types-oem/">OEM</a> table type.</td></tr>
<tr><td><code>TABLE_LIST</code></td><td>String</td><td>The comma separated list of TBL table sub-tables.</td></tr>
<tr><td><code>TABLE_TYPE</code></td><td>String</td><td>The external table <a href="/kb/en/connect-table-types-overview/">type</a>: <a href="/kb/en/connect-dos-and-fix-table-types/">DOS</a>, <a href="/kb/en/connect-dos-and-fix-table-types/">FIX</a>, <a href="/kb/en/connect-bin-table-type/">BIN</a>, <a href="/kb/en/connect-csv-and-fmt-table-types/">CSV</a>, <a href="/kb/en/connect-csv-and-fmt-table-types/">FMT</a>, <a href="/kb/en/connect-xml-table-type/">XML</a>, <a href="/kb/en/connect-json-table-type/">JSON</a>, <a href="/kb/en/connect-ini-table-type/">INI</a>, <a href="/kb/en/connect-dbf-table-type/">DBF</a>, <a href="/kb/en/connect-vec-table-type/">VEC</a>, <a href="/kb/en/connect-table-types-odbc-table-type-accessing-tables-from-other-dbms/">ODBC</a>, <a href="/kb/en/connect-jdbc-table-type-accessing-tables-from-other-dbms/">JDBC</a>, <a href="/kb/en/connect-table-types-mysql-table-type-accessing-mysqlmariadb-tables/">MYSQL</a>, <a href="/kb/en/connect-table-types-tbl-table-type-table-list/">TBL</a>, <a href="/kb/en/connect-table-types-proxy-table-type/">PROXY</a>, <a href="/kb/en/connect-table-types-xcol-table-type/">XCOL</a>, <a href="/kb/en/connect-table-types-occur-table-type/">OCCUR</a>, <a href="/kb/en/connect-table-types-pivot-table-type/">PIVOT</a>, <a href="/kb/en/connect-zipped-file-tables/">ZIP</a>, <a href="/kb/en/connect-table-types-vir/">VIR</a>, <a href="/kb/en/connect-table-types-special-virtual-tables/#dir-type">DIR</a>, <a href="/kb/en/connect-table-types-special-virtual-tables/#windows-management-instrumentation-table-type-wmi">WMI</a>, <a href="/kb/en/connect-table-types-special-virtual-tables/#mac-address-table-type-mac">MAC</a>, and <a href="/kb/en/connect-table-types-oem/">OEM</a>. Defaults to <a href="/kb/en/connect-dos-and-fix-table-types/">DOS</a>, <a href="/kb/en/connect-table-types-mysql-table-type-accessing-mysqlmariadb-tables/">MYSQL</a>, or <a href="/kb/en/connect-table-types-proxy-table-type/">PROXY</a> depending on what options are used.</td></tr>
<tr><td><code>TABNAME</code></td><td>String</td><td>The target table or node for <a href="/kb/en/connect-table-types-odbc-table-type-accessing-tables-from-other-dbms/">ODBC</a>, <a href="/kb/en/connect-jdbc-table-type-accessing-tables-from-other-dbms/">JDBC</a>, <a href="/kb/en/connect-table-types-mysql-table-type-accessing-mysqlmariadb-tables/">MYSQL</a>, <a href="/kb/en/connect-table-types-proxy-table-type/">PROXY</a>, or <a href="/kb/en/connect-table-types-catalog-tables/">catalog tables</a>; or the top node name for <a href="/kb/en/connect-xml-table-type/">XML</a> tables.</td></tr>
<tr><td><code>URI</code></td><td>String</td><td>The URI of a REST request.. From <a href="/kb/en/connect/">Connect 1.06.0010</a>.</td></tr>
<tr><td><code>XFILE_NAME</code></td><td>String</td><td>The file (path) base name for table index files. Can be absolute or relative to the data directory. Defaults to the file name.</td></tr>
<tr><td><code>ZIPPED</code></td><td>Boolean</td><td>True if the table file(s) is/are zipped in one or several zip files.</td></tr>
</tbody></table>

All integers in the above table are unsigned big integers.

Because CONNECT handles many table types; many table type specific options are
not in the above list and must be entered using the `OPTION_LIST` option. The
syntax to use is:

```sql
... option_list='opname1=opvalue1,opname2=opvalue2...'
```

Be aware that until Connect 1.5.5, no blanks should be inserted before or after the '`=`' and
'`,`' characters. The option name is all that is between the start of the
string or the last '`,`' character and the next '`=`' character, and the
option value is all that is between this '`=`' character and the next '`,`'
or end of string. For instance:

```sql
option_list='name=TABLE,coltype=HTML,attribute=border=1;cellpadding=5,headattr=bgcolor=yellow';
```

This defines four options, '`name`', '`coltype`', '`attribute`', and
'`headattr`'; with values '`TABLE`', '`HTML`',
'`border=1;cellpadding=5`', and '`bgcolor=yellow`', respectively. The only
restriction is that values cannot contain commas, but they can contain equal
signs.

### Column Options

<table><tbody><tr><th>Column Option</th><th>Type</th><th>Description</th></tr>
<tr><td><code>DATE_FORMAT</code></td><td>String</td><td>The format indicating how a date is stored in the file.</td></tr>
<tr><td><code>DISTRIB</code></td><td>Enum</td><td>“scattered”, “clustered”, “sorted” (ascending).</td></tr>
<tr><td><code>FIELD_FORMAT</code></td><td>String</td><td>The column format for some table types.</td></tr>
<tr><td><code>FIELD_LENGTH</code></td><td>Integer</td><td>Set the internal field length for DATE columns.</td></tr>
<tr><td><code>FLAG</code></td><td>Integer</td><td>An integer value whose meaning depends on the table type.</td></tr>
<tr><td><code>JPATH</code></td><td>String</td><td>The Json path of JSON table columns.</td></tr>
<tr><td><code>MAX_DIST</code></td><td>Integer</td><td>Maximum number of distinct values in this column.</td></tr>
<tr><td><code>SPECIAL</code></td><td>String</td><td>The name of the SPECIAL column that set this column value.</td></tr>
<tr><td><code>XPATH</code></td><td>String</td><td>The XML path of XML table columns.</td></tr>
</tbody></table>

- The `MAX_DIST` and `DISTRIB` column options are used for block indexing.
- All integers in the above table are unsigned big integers.
- JPATH and XPATH were added to make CREATE TABLE statements more readable, but they do the same thing as FIELD_FORMAT and any of them can be used with the same result.

### Index Options

<table><tbody><tr><th>Index Option</th><th>Type</th><th>Description</th></tr>
<tr><td>DYNAM</td><td>Boolean</td><td>Set the index as “dynamic”.</td></tr>
<tr><td>MAPPED</td><td>Boolean</td><td>Use index file mapping.</td></tr>
</tbody></table>

<strong>Note 1:</strong> Creating a CONNECT table based on file does not erase or create the
file if the file name is specified in the CREATE TABLE statement ([“outward”](/kb/en/inward-and-outward-tables/#outward-tables) table). If the file does not exist, it will be populated by subsequent INSERT or LOAD
commands or by the “AS select statement” of the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/)
command. Unlike the CSV engine, CONNECT easily permits the creation of tables
based on already existing files, for instance files made by other applications.
However, if the file name is not specified, a file with a name defaulting to
`tablename.tabletype` will be created in the data directory ([“inward”](/kb/en/inward-and-outward-tables/#inward-tables) table).

<strong>Note 2:</strong> Dropping a CONNECT table is done with a standard DROP statement.
For [outward tables](/kb/en/inward-and-outward-tables/#inward-tables), this drops only the CONNECT table definition but does not
erase the corresponding data file and index files. Use `DELETE` or
`TRUNCATE` to do so. This is contrary to data and index files of [inward
tables](/kb/en/inward-and-outward-tables/#inward-tables) are erased on DROP like for other MariaDB engines.