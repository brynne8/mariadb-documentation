# CONNECT Table Types Overview

CONNECT can handle very many table formats; it is indeed one of its main
features. The Type option specifies the type and format of the table. The Type
options available values and their descriptions are listed in the following
table:

<table><tbody><tr><th>Type</th><th>Description</th></tr>
<tr><td><a href="/kb/en/connect-bin-table-type/">BIN</a></td><td>Binary file with numeric values in platform representation, also with columns at fixed offset within records and fixed record length.</td></tr>
<tr><td>BSON</td><td>(Temporary) JSON table handled by the new JSON handling.</td></tr>
<tr><td><a href="/kb/en/connect-csv-and-fmt-table-types/">CSV</a>*$</td><td>"Comma Separated Values" file in which each variable length record contains column values separated by a specific character (defaulting to the comma)</td></tr>
<tr><td><a href="/kb/en/connect-dbf-table-type/">DBF</a>*</td><td>File having the dBASE format.</td></tr>
<tr><td><a href="/kb/en/connect-dos-and-fix-table-types/">DOS</a></td><td>The table is contained in one or several files. The file format can be refined by some other options of the command or more often using a specific type as many of those described below. Otherwise, it is a flat text file where columns are placed at a fixed offset within each record, the last column being of variable length.</td></tr>
<tr><td><a href="/kb/en/connect-table-types-special-virtual-tables/#dir-type">DIR</a></td><td>Virtual table that returns a file list like the Unix <code>ls</code> or DOS <code>dir</code> command.</td></tr>
<tr><td><a href="/kb/en/connect-dos-and-fix-table-types/">FIX</a></td><td>Text file arranged like DOS but with fixed length records.</td></tr>
<tr><td><a href="/kb/en/connect-csv-and-fmt-table-types/">FMT</a></td><td>File in which each record contains the column values in a non-standard format (the same for each record) This format is specified in the column definition.</td></tr>
<tr><td><a href="/kb/en/connect-ini-table-type/">INI</a></td><td>File having the format of the initialization or configuration files used by many applications.</td></tr>
<tr><td><a href="/kb/en/connect-table-types-special-virtual-tables/#mac-address-table-type-mac">MAC</a></td><td>Virtual table returning information about the machine and network cards (Windows only).</td></tr>
<tr><td><a href="/kb/en/connect-jdbc-table-type-accessing-tables-from-other-dbms/">JDBC</a>*</td><td>Table accessed via a JDBC driver. (from Connect 1.04.0006)</td></tr>
<tr><td><a href="/kb/en/connect-json-table-type/">JSON</a>*$</td><td>File having the JSON format. (from <a href="/kb/en/mariadb-10016-release-notes/">MariaDB 10.0.16</a>)</td></tr>
<tr><td><a href="/kb/en/connect-mongo-table-type/">MONGO</a>*</td><td>Table accessed via the MongoDB C Driver API.</td></tr>
<tr><td><a href="/kb/en/connect-table-types-mysql-table-type-accessing-mysqlmariadb-tables/">MYSQL</a>*</td><td>Table accessed using the MySQL API like the FEDERATED engine.</td></tr>
<tr><td><a href="/kb/en/connect-table-types-occur-table-type/">OCCUR</a>*</td><td>A table based on another table existing on the current server, several columns of the object table containing values that can be grouped in only one column.</td></tr>
<tr><td><a href="/kb/en/connect-table-types-odbc-table-type-accessing-tables-from-other-dbms/">ODBC</a>*</td><td>Table extracted from an application accessible via ODBC or unixODBC. For example from another DBMS or from an Excel spreadsheet.</td></tr>
<tr><td><a href="/kb/en/connect-table-types-oem-type-table-type-implemented-in-an-external-lib/">OEM</a>*</td><td>Table of any other formats not directly handled by CONNECT but whose access is implemented by an external FDW (foreign data wrapper) written in C++ (as a DLL or Shared Library).</td></tr>
<tr><td><a href="/kb/en/connect-table-types-pivot-table-type/">PIVOT</a>*</td><td>Used to "pivot" the display of an existing table or view.</td></tr>
<tr><td><a href="/kb/en/connect-table-types-proxy-table-type/">PROXY</a>*</td><td>A table based on another table existing on the current server.</td></tr>
<tr><td><a href="/kb/en/connect-table-types-tbl-table-type-table-list/">TBL</a>*</td><td>Accessing a collection of tables as one table (like the MERGE engine does for MyIsam tables)</td></tr>
<tr><td><a href="/kb/en/connect-vec-table-type/">VEC</a></td><td>Binary file organized in vectors, in which column values are grouped consecutively, either split in separate files or in a unique file.</td></tr>
<tr><td><a href="/kb/en/connect-table-types-vir/">VIR*</a></td><td>Virtual table containing only special and virtual columns.</td></tr>
<tr><td><a href="/kb/en/connect-table-types-special-virtual-tables/#windows-management-instrumentation-table-type-wmi">WMI</a>*</td><td>Windows Management Instrumentation table displaying information coming from a WMI provider. This type enables to get in tabular format all sorts of information about the machine hardware and operating system (Windows only).</td></tr>
<tr><td><a href="/kb/en/connect-table-types-xcol-table-type/">XCOL</a>*</td><td>A table based on another table existing on the current server with one of its columns containing of comma separated values.</td></tr>
<tr><td><a href="/kb/en/connect-xml-table-type/">XML</a>*$</td><td>File having the XML or HTML format.</td></tr>
<tr><td><a href="/kb/en/connect-zipped-file-tables/">ZIP</a></td><td>Table giving information about the contents of a zip file.</td></tr>
</tbody></table>

## Catalog Tables

For all table types marked with a '*' in the table above, CONNECT is able to
analyze the data source to retrieve the column definition. This can be used to
define a “catalog” table that display the column description of the source, or
to create a table without specifying the column definition that will be
automatically constructed by CONNECT when creating the table.

When marked with a ‘$’ the file can be the result returned by a REST query.