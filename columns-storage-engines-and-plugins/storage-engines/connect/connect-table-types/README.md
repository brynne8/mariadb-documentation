# CONNECT Table Types

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The CONNECT handler was introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

The main feature of [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) is to give MariaDB the ability to handle tables from many sources, native files, other DBMS’s tables, or special “virtual” tables. Moreover, for all tables physically represented by data files, CONNECT recognizes many different file formats, described below but not limited in the future to this list, because more can be easily added to it on demand ([OEM tables](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-table-types-oem/)).

Note: You can download a [PDF version of the CONNECT documentation](https://mariadb.com/kb/en/connect-table-types/+attachment/connect_1_7_01) (1.7.01).

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