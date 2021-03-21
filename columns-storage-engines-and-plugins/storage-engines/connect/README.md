# CONNECT

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The CONNECT storage engine was introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

Note: You can download a [PDF version of the CONNECT documentation](https://mariadb.com/kb/en/connect-table-types/+attachment/connect_1_7_01) (1.7.01).

<table><tbody><tr><th>Connect Version</th><th>Introduced</th><th>Maturity</th></tr>
<tr><td>Connect 1.07.0002</td><td><a href="/kb/en/mariadb-1059-release-notes/">MariaDB 10.5.9</a>, <a href="/kb/en/mariadb-10418-release-notes/">MariaDB 10.4.18</a>, <a href="/kb/en/mariadb-10328-release-notes/">MariaDB 10.3.28</a>, <a href="/kb/en/mariadb-10236-release-notes/">MariaDB 10.2.36</a></td><td>Stable</td></tr>
<tr><td>Connect 1.07.0001</td><td><a href="/kb/en/mariadb-10412-release-notes/">MariaDB 10.4.12</a>, <a href="/kb/en/mariadb-10322-release-notes/">MariaDB 10.3.22</a>, <a href="/kb/en/mariadb-10231-release-notes/">MariaDB 10.2.31</a>, <a href="/kb/en/mariadb-10144-release-notes/">MariaDB 10.1.44</a></td><td>Stable</td></tr>
<tr><td>Connect 1.06.0010</td><td><a href="/kb/en/mariadb-1048-release-notes/">MariaDB 10.4.8</a>, <a href="/kb/en/mariadb-10318-release-notes/">MariaDB 10.3.18</a>, <a href="/kb/en/mariadb-10227-release-notes/">MariaDB 10.2.27</a></td><td>Stable</td></tr>
<tr><td>Connect 1.06.0007</td><td><a href="/kb/en/mariadb-1036-release-notes/">MariaDB 10.3.6</a>, <a href="/kb/en/mariadb-10214-release-notes/">MariaDB 10.2.14</a>, <a href="/kb/en/mariadb-10133-release-notes/">MariaDB 10.1.33</a>, <a href="/kb/en/mariadb-10035-release-notes/">MariaDB 10.0.35</a></td><td>Stable</td></tr>
<tr><td>Connect 1.06.0005</td><td><a href="/kb/en/mariadb-1033-release-notes/">MariaDB 10.3.3</a>, <a href="/kb/en/mariadb-10210-release-notes/">MariaDB 10.2.10</a>, <a href="/kb/en/mariadb-10129-release-notes/">MariaDB 10.1.29</a>, <a href="/kb/en/mariadb-10033-release-notes/">MariaDB 10.0.33</a></td><td>Stable</td></tr>
<tr><td>Connect 1.06.0004</td><td><a href="/kb/en/mariadb-1032-release-notes/">MariaDB 10.3.2</a>, <a href="/kb/en/mariadb-1029-release-notes/">MariaDB 10.2.9</a>, <a href="/kb/en/mariadb-10128-release-notes/">MariaDB 10.1.28</a></td><td>Stable</td></tr>
<tr><td>Connect 1.06.0001</td><td><a href="/kb/en/mariadb-1031-release-notes/">MariaDB 10.3.1</a>, <a href="/kb/en/mariadb-1028-release-notes/">MariaDB 10.2.8</a>, <a href="/kb/en/mariadb-10124-release-notes/">MariaDB 10.1.24</a>, <a href="/kb/en/mariadb-10031-release-notes/">MariaDB 10.0.31</a></td><td>Beta</td></tr>
<tr><td>Connect 1.05.0003</td><td><a href="/kb/en/mariadb-1030-release-notes/">MariaDB 10.3.0</a>, <a href="/kb/en/mariadb-1025-release-notes/">MariaDB 10.2.5</a>, <a href="/kb/en/mariadb-10122-release-notes/">MariaDB 10.1.22</a>, <a href="/kb/en/mariadb-10030-release-notes/">MariaDB 10.0.30</a></td><td>Stable</td></tr>
<tr><td>Connect 1.05.0001</td><td><a href="/kb/en/mariadb-1024-release-notes/">MariaDB 10.2.4</a>, <a href="/kb/en/mariadb-10121-release-notes/">MariaDB 10.1.21</a>, <a href="/kb/en/mariadb-10029-release-notes/">MariaDB 10.0.29</a></td><td>Stable</td></tr>
<tr><td>Connect 1.04.0008</td><td><a href="/kb/en/mariadb-1022-release-notes/">MariaDB 10.2.2</a>, <a href="/kb/en/mariadb-10117-release-notes/">MariaDB 10.1.17</a>, <a href="/kb/en/mariadb-10027-release-notes/">MariaDB 10.0.27</a></td><td>Stable</td></tr>
<tr><td>Connect 1.04.0006</td><td><a href="/kb/en/mariadb-1020-release-notes/">MariaDB 10.2.0</a>, <a href="/kb/en/mariadb-10113-release-notes/">MariaDB 10.1.13</a>, <a href="/kb/en/mariadb-10025-release-notes/">MariaDB 10.0.25</a></td><td>Stable</td></tr>
<tr><td>Connect 1.04.0005</td><td><a href="/kb/en/mariadb-10110-release-notes/">MariaDB 10.1.10</a>, <a href="/kb/en/mariadb-10023-release-notes/">MariaDB 10.0.23</a></td><td>Beta</td></tr>
<tr><td>Connect 1.04.0003</td><td><a href="/kb/en/mariadb-1019-release-notes/">MariaDB 10.1.9</a></td><td>Beta</td></tr>
<tr><td>Connect 1.03.0007</td><td><a href="/kb/en/mariadb-1015-release-notes/">MariaDB 10.1.5</a>, <a href="/kb/en/mariadb-10020-release-notes/">MariaDB 10.0.20</a></td><td>Stable</td></tr>
<tr><td>Connect 1.03.0006</td><td><a href="/kb/en/mariadb-1013-release-notes/">MariaDB 10.1.3</a>, <a href="/kb/en/mariadb-10016-release-notes/">MariaDB 10.0.16</a></td><td>Beta</td></tr>
<tr><td>Connect 1.03.0005</td><td><a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a>, <a href="/kb/en/mariadb-10015-release-notes/">MariaDB 10.0.15</a></td><td>Beta</td></tr>
<tr><td>Connect 1.03.0003</td><td><a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a>, <a href="/kb/en/mariadb-10014-release-notes/">MariaDB 10.0.14</a></td><td>Beta</td></tr>
<tr><td>Connect 1.03.0002</td><td><a href="/kb/en/mariadb-10013-release-notes/">MariaDB 10.0.13</a></td><td>Beta</td></tr>
<tr><td>Connect 1.02.0002</td><td><a href="/kb/en/mariadb-1010-release-notes/">MariaDB 10.1.0</a>, <a href="/kb/en/mariadb-10010-release-notes/">MariaDB 10.0.10</a></td><td>Beta</td></tr>
<tr><td>Connect 1.02.0001</td><td><a href="/kb/en/mariadb-1008-release-notes/">MariaDB 10.0.8</a></td><td>Beta</td></tr>
<tr><td>Connect 1.01.0008</td><td><a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td><td>Beta</td></tr>
<tr><td>Connect 1.01.0007</td><td><a href="/kb/en/mariadb-1004-release-notes/">MariaDB 10.0.4</a></td><td>Beta</td></tr>
<tr><td>Connect 1.01.0006</td><td><a href="/kb/en/mariadb-1003-release-notes/">MariaDB 10.0.3</a></td><td>Experimental</td></tr>
<tr><td>Connect 1.01.0004</td><td><a href="/kb/en/mariadb-1002-release-notes/">MariaDB 10.0.2</a></td><td>Experimental</td></tr>
</tbody></table>

The CONNECT storage engine enables MariaDB to access external local or remote
data (MED). This is done by defining tables based on different data types, in
particular files in various formats, data extracted from other DBMS or products
(such as Excel or MongoDB) via ODBC or JDBC, or data retrieved from the environment (for example DIR, WMI, and MAC tables)

This storage engine supports table partitioning, MariaDB virtual columns and permits defining
 <em>special</em> columns such as ROWID, FILEID, and SERVID.

No precise definition of maturity exists. Because CONNECT handles many table types, each type has a different maturity depending on whether it is old and well-tested, less well-tested or newly implemented. This will be indicated for all data types.

- [Introduction to the CONNECT Engine](/columns-storage-engines-and-plugins/storage-engines/connect/introduction-to-the-connect-engine/) — Reasons behind the CONNECT storage engine.
- [Installing the CONNECT Storage Engine](/columns-storage-engines-and-plugins/storage-engines/connect/installing-the-connect-storage-engine/) — Installing the CONNECT storage engine.
- [Creating and Dropping CONNECT Tables](/columns-storage-engines-and-plugins/storage-engines/connect/creating-and-dropping-connect-tables/) — Creating and dropping CONNECT tables.
- [CONNECT Data Types](/columns-storage-engines-and-plugins/storage-engines/connect/connect-data-types/) — Data types supported by CONNECT.
- [Current Status of the CONNECT Handler](/columns-storage-engines-and-plugins/storage-engines/connect/current-status-of-the-connect-handler/) — The current CONNECT handler is a stable release.

### [CONNECT Table Types](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/)

- [CONNECT Table Types Overview](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-table-types-overview/) — CONNECT can handle many table formats.
- [Inward and Outward Tables](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/inward-and-outward-tables/) — The two broad categories of CONNECT tables.
- [CONNECT Table Types - Data Files](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-table-types-data-files/) — CONNECT plain DOS or UNIX data files.
- [CONNECT Zipped File Tables](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-zipped-file-tables/) — When the table file or files are compressed in one or several zip files.
- [CONNECT DOS and FIX Table Types](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-dos-and-fix-table-types/) — CONNECT tables based on text files
- [CONNECT DBF Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-dbf-table-type/) — CONNECT dBASE III or IV tables.
- [CONNECT BIN Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-bin-table-type/) — CONNECT binary files in which each row is a logical record of fixed length
- [CONNECT VEC Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-vec-table-type/) — CONNECT binary files organized in vectors
- [CONNECT CSV and FMT Table Types](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-csv-and-fmt-table-types/) — Variable length CONNECT data files.
- [CONNECT - NoSQL Table Types](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-nosql-table-types/) — Based on files that do not match the relational format but often represent hierarchical data.
- [CONNECT - Files Retrieved Using Rest Queries](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-files-retrieved-using-rest-queries/) — JSON, XML and CSV data files can be retrieved as results from REST queries.
- [CONNECT XML Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-xml-table-type/) — CONNECT XML files
- [CONNECT JSON Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-json-table-type/) — JSON (JavaScript Object Notation) is a widely-used lightweight data-interchange format.
- [CONNECT INI Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-ini-table-type/) — CONNECT INI Windows configuration or initialization files.
- [CONNECT - External Table Types](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-external-table-types/) — Access tables belonging to the current or another server
- [CONNECT ODBC Table Type: Accessing Tables From Another DBMS](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-odbc-table-type-accessing-tables-from-another-dbms/) — CONNECT Table Types - ODBC Table Type: Accessing Tables from other DBMS
- [CONNECT JDBC Table Type: Accessing Tables from Another DBMS](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-jdbc-table-type-accessing-tables-from-another-dbms/) — Using JDBC to access other tables.
- [CONNECT MONGO Table Type: Accessing Collections from MongoDB](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-mongo-table-type/) — Used to directly access MongoDB collections as tables.
- [CONNECT MYSQL Table Type: Accessing MySQL/MariaDB Tables](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-mysql-table-type-accessing-mysqlmariadb-tables/) — Accessing a MySQL or MariaDB table or view
- [CONNECT PROXY Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-proxy-table-type/) — Tables that access and read the data of another table or view
- [CONNECT XCOL Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-xcol-table-type/) — Based on another table/view, used when object tables have a column that contains a list of values
- [CONNECT OCCUR Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-occur-table-type/) — Extension to the PROXY type when referring to a table/view having several c...
- [CONNECT PIVOT Table Type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-pivot-table-type/) — Transform the result of another table into another table along “pivot” and "fact" columns.
- [CONNECT TBL Table Type: Table List](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-tbl-table-type-table-list/) — Define a table as a list of tables of any engine and type.
- [CONNECT - Using the TBL and MYSQL Table Types Together](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-using-the-tbl-and-mysql-table-types-together/) — Used together, the TBL and MYSQL types lift all the limitations of the FEDERATED and MERGE engines
- [CONNECT Table Types - Special "Virtual" Tables](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-table-types-special-virtual-tables/) — VIR, WMI and MAC special table types
- [CONNECT Table Types - VIR](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-table-types-vir/) — VIR virtual type for CONNECT
- [CONNECT Table Types - OEM: Implemented in an External LIB](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-table-types-oem/) — CONNECT OEM table types are implemented in an external library.
- [CONNECT Table Types - Catalog Tables](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-table-types-catalog-tables/) — Catalog tables return information about another table or data source
- [Adding DataFlex 3.1c .dat Files As An External Table Type With CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/adding-dataflex-31c-dat-files-as-an-external-table-type-with-connect/) — I'm using MariaDB's CONNECT engine to access / utilize a set of Visual FoxP...
- [CONNECT engine windows](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-engine-windows/) — with mariadb 10.07 installed on windows, how to setup engines, particulary ...
- [creating pivot table fails](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/creating-pivot-table-fails/) — I tried to create a pivot table based on an existing table "test1" and get ...
- [limit of number of columns](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/limit-of-number-of-columns/) — I have a table of 6MB with 50 values in the pivot-values leading to 50 colu...

### Other CONNECT Articles

- [CONNECT - Security](/columns-storage-engines-and-plugins/storage-engines/connect/connect-security/) — CONNECT requires the FILE privilege for "outward" tables
- [CONNECT - OEM Table Example](/columns-storage-engines-and-plugins/storage-engines/connect/connect-oem-table-example/) — Example showing how an OEM table can be implemented.

### [Using CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/using-connect/)

- [Using CONNECT - General Information](/columns-storage-engines-and-plugins/storage-engines/connect/using-connect/using-connect-general-information/) — Using CONNECT - General Information
- [Using CONNECT - Virtual and Special Columns](/columns-storage-engines-and-plugins/storage-engines/connect/using-connect/using-connect-virtual-and-special-columns/) — Virtual and special columns example usage
- [Using CONNECT - Importing File Data Into MariaDB Tables](/columns-storage-engines-and-plugins/storage-engines/connect/using-connect/using-connect-importing-file-data-into-mariadb-tables/) — Directly using external (file) data has many advantages
- [Using CONNECT - Exporting Data From MariaDB](/columns-storage-engines-and-plugins/storage-engines/connect/using-connect/using-connect-exporting-data-from-mariadb/) — Exporting data from MariaDB with CONNECT
- [Using CONNECT - Indexing](/columns-storage-engines-and-plugins/storage-engines/connect/using-connect/using-connect-indexing/) — Indexing with the CONNECT handler
- [Using CONNECT - Condition Pushdown](/columns-storage-engines-and-plugins/storage-engines/connect/using-connect/using-connect-condition-pushdown/) — Using CONNECT - Condition Pushdown
- [USING CONNECT - Documentation](/columns-storage-engines-and-plugins/storage-engines/connect/using-connect/using-connect-documentation/) — CONNECT Plugin User Manual
- [Using CONNECT - Partitioning and Sharding](/columns-storage-engines-and-plugins/storage-engines/connect/using-connect/using-connect-partitioning-and-sharding/) — Partitioning and Sharding with CONNECT

### Other CONNECT Articles

- [CONNECT - Making the GetRest Library](/columns-storage-engines-and-plugins/storage-engines/connect/connect-making-the-getrest-library/) — Compiling the function calling the cpprestsdk package separately that will be loaded by CONNECT.
- [CONNECT - Adding the REST Feature as a Library Called by an OEM Table](/columns-storage-engines-and-plugins/storage-engines/connect/connect-adding-the-rest-feature-as-a-library-called-by-an-oem-table/) — How the REST feature can be added as a library called by an OEM table.
- [CONNECT - Compiling JSON UDFs in a Separate Library](/columns-storage-engines-and-plugins/storage-engines/connect/connect-compiling-json-udfs-in-a-separate-library/) — There are situations when you may need to have JSON UDFs in a separate library.
- [CONNECT System Variables](/columns-storage-engines-and-plugins/storage-engines/connect/connect-system-variables/) — System variables related to the CONNECT storage engine.
- [JSON Sample Files](/columns-storage-engines-and-plugins/storage-engines/connect/json-sample-files/) — expense.json sample file