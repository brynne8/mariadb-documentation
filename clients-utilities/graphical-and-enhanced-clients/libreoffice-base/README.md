# LibreOffice Base

[LibreOffice Base](https://www.libreoffice.org/discover/base/) is an open source RDBMS (relational database management system) front-end tool to create and manage various databases.

## Preparing the ODBC Connection

First, make sure to prepare MariaDB Connector/ODBC as explained in [MariaDB Connector/ODBC](https://mariadb.com/kb/en/about-mariadb-connector-odbc/).

That includes:

- Download [the latest MariaDB Connector/ODBC](https://mariadb.com/downloads/#connectors)
- Copy the shared library <em>libmaodbc.so</em> to <em>/usr/lib/[multi-arch]</em>
- Install the <em>unixodbc, unixodbc-dev, openssh-client, odbcinst</em> packages
- Create a template file for the [ODBC driver](https://mariadb.com/kb/en/creating-a-data-source-with-mariadb-connectorodbc/#configuring-mariadb-connectorodbc-as-a-unixodbc-driver-on-linux). A sample <em>“MariaDB_odbc_driver_template.ini”</em> could be:

<table><tbody><tr><td>[MariaDB ODBC 3.1 Driver]</td></tr>
<tr><td>Description = MariaDB Connector/ODBC v.3.1</td></tr>
<tr><td>Driver = /usr/lib/x86_64-linux-gnu/libmaodbc.so</td></tr>
</tbody></table>

- Install the ODBC driver from the template file by running:

```sql
$ odbcinst -i -d -f MariaDB_odbc_driver_template.ini
```

Verify successful installation in <em>/etc/odbcinst.ini</em> file.

- Create a template file for the [Data Source Name (DSN)](https://mariadb.com/kb/en/creating-a-data-source-with-mariadb-connectorodbc/#configuring-a-dsn-with-unixodbc-on-linux). A sample <em>“MariaDB_odbc_data_source_template.ini”</em> could be:

<table><tbody><tr><td>[MariaDB-server]</td></tr>
<tr><td>Description=MariaDB server</td></tr>
<tr><td>Driver=MariaDB ODBC 3.1 Driver</td></tr>
<tr><td>SERVER=MariaDB</td></tr>
<tr><td>USER=anel</td></tr>
<tr><td>PASSWORD=</td></tr>
<tr><td>DATABASE=test</td></tr>
<tr><td>PORT=3306</td></tr>
</tbody></table>

- Install data source:

```sql
odbcinst -i -s -h -f MariaDB_odbc_data_source_template.ini
```

Verify successful installation in the <em>/.odbc.ini</em> file and also using the [isql](https://mariadb.com/kb/en/creating-a-data-source-with-mariadb-connectorodbc/#verifying-a-dsn-configuration-with-unixodbc-on-linux) utility, for example:

```sql
$ isql MariaDB-server
+---------------------------------------+
| Connected!                            |
|                                       |
| sql-statement                         |
| help [tablename]                      |
| quit                                  |
|                                       |
+---------------------------------------+
SQL> show tables;
+--------------------------------------------------------------------------+
| Tables_in_test                                                           |
+--------------------------------------------------------------------------+
| Authors                                                                  |
| tbl_names                                                                |
| webposts                                                                 |
| webusers                                                                 |
+--------------------------------------------------------------------------+
SQLRowCount returns 4
4 rows fetched

```

## Start with LibreOffice Base

Start Libreoffice Base from the terminal by running <em>lobase</em> (make sure to install the <em>libreoffice-base</em> package if needed). The default option is to create a new database, which is <em>HSQLDB</em>. In order to connect to a running MariaDB server, choose <em>“Connect to an existing database”</em> and choose <em>“ODBC”</em> driver as shown below:

<img src="/kb/en/libreoffice-base/+image/librebase_1" alt="librebase_1" title="librebase_1">

After that, choose DSN (the one that we created in the previous step) and click <em>“Next”</em>:

<img src="/kb/en/libreoffice-base/+image/librebase_2" alt="librebase_2" title="librebase_2">

Provide a user name (and password if needed) and again check the connection (with the <em>“Test Connection”</em> button) and click <em>“Next”</em>:

<img src="/kb/en/libreoffice-base/+image/librebase_3" alt="librebase_3" title="librebase_3">

After that, we have options to register the database. Registration in this sense means that the database is viewable by other LibreOffice modules (like <em>LibreOffice Calc</em> and  <em>LibreOffice Writer</em>). So this step is optional. In this example, we will save as <em>“fosdem21_mariadb.odb”</em>. See [Using a Registered Database](##using-a-registered-database).

<img src="/kb/en/libreoffice-base/+image/librebase_4" alt="librebase_4" title="librebase_4">

It opens the following window:

<img src="/kb/en/libreoffice-base/+image/librebase_5" alt="librebase_5" title="librebase_5">

It consists of three windows/panels:

1 <em>“Database”</em> window with the options
<ol start="1"><li><em>"Tables"</em>, 
</li><li><em>"Queries"</em>,
</li><li><em>"Forms"</em>,
</li><li><em>"Reports"</em>.
</li></ol>
2 "Tasks window (dependent on what is selected in the <em>“Database”</em> window). When <em>“Tables”</em> is selected, the options are:
<ol start="1"><li><em>"Create Table in Design View"</em>, 
</li><li><em>"Use Wizard to Create Table"</em> and 
</li><li><em>"Create View"</em>.
</li></ol>
3 <em>"Tables"</em> window - shows list of tables that are created.

As we can see, there are system tables in the <em>“mysql”</em> database as well as <em>“test”</em> database.

Let’s say we create a table using the REST API from JSON data from [http://jsonplaceholder.typicode.com/posts](http://jsonplaceholder.typicode.com/posts), and another table using the same mechanism from [http://jsonplaceholder.typicode.com/users](http://jsonplaceholder.typicode.com/users), and let’s call them <em>webposts</em> and <em>webusers</em>. In order to do so, we have to enable the <strong>CONNECT</strong> storage engine plugin and start with REST_API. See more in the [CONNECT - Files Retrieved Using Rest Queries](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-files-retrieved-using-rest-queries) article.

The queries we need to run in MariaDB are:

```sql
CREATE TABLE webusers ENGINE=CONNECT TABLE_TYPE=JSON
  HTTP='http://jsonplaceholder.typicode.com/users';

CREATE TABLE webposts ENGINE=CONNECT TABLE_TYPE=JSON
  HTTP='http://jsonplaceholder.typicode.com/posts';
```

The result in LibreOffice Base will be as shown below:

<img src="/kb/en/libreoffice-base/+image/librebase_6" alt="librebase_6" title="librebase_6">

Double clicking on the table opens a new window with the data displayed to inspect:

<img src="/kb/en/libreoffice-base/+image/librebase_7" alt="librebase_7" title="librebase_7">

To create the table from the <em>“Tasks”</em> window, use the option <em>“Create Table in Design View”</em>, where one can specify specific field names and types as shown:

<img src="/kb/en/libreoffice-base/+image/librebase_8" alt="librebase_8" title="librebase_8">

From the “Tasks” window one can create a table using the option <em>“Use Wizard to Create Table”</em> to create some sample tables:

<span class="attachment_not_found">Attachment 'librebase_9' not found</span>

One can fill the data in the existing table, or create and define the new table from the <em>LibreOffice Calc</em> module with simple copy-paste (in the <em>"Tasks"</em> window).

## Using a Registered Database

Other modules can use the registered database, for example, open <em>"LibreOffice Calc"</em> and go to <em>"Tools"</em>, <em>"Options"</em> and you will see the <em>"odb"</em> file we registered when starting <em>"LibreOffice Base"</em>.

<span class="attachment_not_found">Attachment 'librebase_register_1' not found</span>