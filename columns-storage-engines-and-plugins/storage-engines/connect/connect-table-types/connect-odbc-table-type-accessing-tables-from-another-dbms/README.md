# CONNECT ODBC Table Type: Accessing Tables From Another DBMS

ODBC (Open Database Connectivity) is a standard API for accessing database management systems (DBMS). CONNECT uses this API to access data contained in other DBMS without having to implement a specific application for each one. An exception is the access to MySQL that should be done using the [MYSQL table type](/kb/en/connect-table-types-mysql-table-type-accessing-mysqlmariadb-tables/).

Note: On Linux, unixODBC must be installed.

These tables are given the type ODBC. For example, if a "Customers" table is
contained in an Access<span>™</span> database you can define it
with a command such as:

```sql
create table Customer (
  CustomerID varchar(5),
  CompanyName varchar(40),
  ContactName varchar(30),
  ContactTitle varchar(30),
  Address varchar(60),
  City varchar(15),
  Region varchar(15),
  PostalCode varchar(10),
  Country varchar(15),
  Phone varchar(24),
  Fax varchar(24))
engine=connect table_type=ODBC block_size=10
tabname='Customers'
Connection='DSN=MS Access Database;DBQ=C:/Program
Files/Microsoft Office/Office/1033/FPNWIND.MDB;';
```

Tabname option defaults to the table name. It is required if the source table
name is different from the name of the CONNECT table. Note also that for some data sources this name is case sensitive.

Often, because CONNECT can retrieve the table description using ODBC catalog
functions, the column definitions can be unspecified. For instance this table
can be simply created as:

```sql
create table Customer engine=connect table_type=ODBC
  block_size=10 tabname='Customers'
  Connection='DSN=MS Access Database;DBQ=C:/Program Files/Microsoft Office/Office/1033/FPNWIND.MDB;';
```

The `BLOCK_SIZE` specification will be used later to set the RowsetSize when
retrieving rows from the ODBC table. A reasonably large RowsetSize can greatly
accelerate the fetching process.

If you specify the column description, the column names of your table must
exist in the data source table. However, you are not obliged to define all the
data source columns and you can change the order of the columns. Some type
conversion can also be done if appropriate. For instance, to access the
FireBird sample table EMPLOYEE, you could define your table as:

```sql
create table empodbc (
  EMP_NO smallint(5) not null,
  FULL_NAME varchar(37) not null),
  PHONE_EXT varchar(4) not null,
  HIRE_DATE date,
  DEPT_NO smallint(3) not null,
  JOB_COUNTRY varchar(15),
  SALARY double(12,2) not null)
engine=CONNECT table_type=ODBC tabname='EMPLOYEE'
connection='DSN=firebird';
```

This definition ignores the FIRST_NAME, LAST_NAME, JOB_CODE, and JOB_GRADE
columns. It places the FULL_NAME last column of the original table in second
position. The type of the HIRE_DATE column was changed from <em>timestamp</em> to
 <em>date</em> and the type of the DEPT_NO column was changed from <em>char</em> to
 <em>integer</em>.

Currently, some restrictions apply to ODBC tables:

1 Cursor type is forward only (sequential reading).
2 No indexing of ODBC tables (do not specify any columns as key). However,
  because CONNECT can often add a where clause to the query sent to the data
  source, indexing will be used by the data source if it supports it. (Remote indexing is available with version 1.04, released with [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/))
3 CONNECT ODBC supports [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) and [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/). [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) and [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) are also supported
  in a somewhat restricted way (see below). For other operations, use an ODBC
  table with the EXECSRC option (see below) to directly send proper commands
  to the data source.

## Random Access of ODBC Tables

In CONNECT version 1.03 (until [MariaDB 10.1.5](/kb/en/mariadb-1015-release-notes/)) ODBC tables are not indexable. Version 1.04 (from [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/)) adds remote indexing facility to the ODBC table type.

However, some queries require random access to an ODBC table; for instance when it is joined to another table or used in an order by queries applied to a long column or large tables.

There are several ways to enable random (position) access to a CONNECT ODBC table. They are dependant on the following table options:

<table><tbody><tr><th>Option</th><th>Type</th><th>Used For</th></tr>
<tr><td>Block_Size</td><td>Integer</td><td>Specifying the rowset size.</td></tr>
<tr><td>Memory*</td><td>Integer</td><td>Storing the result set in memory.</td></tr>
<tr><td>Scrollable*</td><td>Boolean</td><td>Using a scrollable cursor.</td></tr>
</tbody></table>

`*` - To be specified in the option_list.

When dealing with small tables, the simpler way to enable random access is to specify a rowset size equal or larger than the table size (or the result set size if a push down where clause is used). This means that the whole result is in memory on the first fetch and CONNECT will use it for further positional accesses.

Another way to have the result set in memory is to use the memory option. This option can be set to the following values:

<strong>0.</strong> No memory used (the default). Best when the table is read sequentially as in SELECT statements with only eventual WHERE clauses.<br>
<strong>1.</strong> Memory size required is calculated during the first sequential table read. The allocated memory is filled during the second sequential read. Then the table rows are retrieved from the memory. This should be used when the table will be accessed several times randomly, such as in sub-selects or being the target table of a join.<br>
<strong>2.</strong> A first query is executed to get the result set size and the needed memory is allocated. It is filled on the first sequential reading. Then random access of the table is possible. This can be used in the case of ORDER BY clauses, when MariaDB uses position reading.<br>

Note that the best way to handle ORDER BY is to set the max_length_for_sort_data variable to a larger value (its default value is 1024 that is pretty small). Indeed, it requires less memory to be used, particularly when a WHERE clause limits the retrieved data set. This is because in the case of an order by query, MariaDB firstly retrieves the sequentially the result set and the position of each records. Often the sort can be done from the result set if it is not too big. But if too big, or if it implies some “long” columns, only the positions are sorted and MariaDB retrieves the final result from the table read in random order. If setting the max_length_for_sort_data variable is not feasible or does not work, to be able to retrieve table data from memory after the first sequential read, the memory option must be set to 2.

For tables too large to be stored in memory another possibility is to make your table to use a scrollable cursor. In this case each randomly accessed row can be retrieved from the data source specifying its cursor position, which is reasonably fast. However, scrollable cursors are not supported by all data sources.

With CONNECT version 1.04 (from [MariaDB 10.1.6](/kb/en/mariadb-1016-release-notes/)), another way to provide random access is to specify some columns to be indexed. This should be done only when the corresponding column of the source table is also indexed. This should be used for tables too large to be stored in memory and is similar to the remote indexing used by the [MYSQL table type](/kb/en/connect-table-types-mysql-table-type-accessing-mysqlmariadb-tables/) and by the [FEDERATED engine](/columns-storage-engines-and-plugins/storage-engines/federatedx-storage-engine/).

There remains the possibility to extract data from the external table and to construct
another table of any file format from the data source. For instance to construct
a fixed formatted DOS table containing the CUSTOMER table data, create the
table as

```sql
create table Custfix engine=connect File_name='customer.txt'
  table_type=fix block_size=20 as select * from customer;
```

Now you can use <em>custfix</em> for fast database operations on the copied
 <em>customer</em> table data.

## Retrieving data from a spreadsheet

ODBC can also be used to create tables based on tabular data belonging to an
Excel spreadsheet:

```sql
create table XLCONT
engine=CONNECT table_type=ODBC tabname='CONTACT'
Connection='DSN=Excel Files;DBQ=D:/Ber/Doc/Contact_BP.xls;';
```

This supposes that a tabular zone of the sheet including column headers is
defined as a table named CONTACT or using a “named reference”. Refer to the Excel documentation for how to
specify tables inside sheets. Once done, you can ask:

```sql
select * from xlcont;
```

This will extract the data from Excel and display:

<table><tbody><tr><th>Nom</th><th>Fonction</th><th>Societe</th></tr>
<tr><td>Boisseau Frederic</td><td></td><td>9 Telecom</td></tr>
<tr><td>Martelliere Nicolas</td><td></td><td>Vidal SA (Groupe UBM)</td></tr>
<tr><td>Remy Agathe</td><td></td><td>Price Minister</td></tr>
<tr><td>Du Halgouet Tanguy</td><td></td><td>Danone</td></tr>
<tr><td>Vandamme Anna</td><td></td><td>GDF</td></tr>
<tr><td>Thomas Willy</td><td></td><td>Europ Assistance France</td></tr>
<tr><td>Thomas Dominique</td><td></td><td>Acoss (DG des URSSAF)</td></tr>
<tr><td>Thomas Berengere</td><td>Responsable SI Decisionnel</td><td>DEXIA Credit Local</td></tr>
<tr><td>Husy Frederic</td><td>Responsable Decisionnel</td><td>Neuf Cegetel</td></tr>
<tr><td>Lemonnier Nathalie</td><td>Directeur Marketing Client</td><td>Louis Vuitton</td></tr>
<tr><td>Louis Loic</td><td>Reporting International Decisionnel</td><td>Accor</td></tr>
<tr><td>Menseau Eric</td><td></td><td>Orange France</td></tr>
</tbody></table>

Here again, the columns description was left to CONNECT when creating the table.

## Multiple ODBC tables

The concept of multiple tables can be extended to ODBC tables when they are
physically represented by files, for instance to Excel or Access tables. The
condition is that the connect string for the table must contain a field
DBQ=<em>filename</em>, in which wildcard characters can be included as for
multiple=1 tables in their filename. For instance, a table contained in several
Excel files CA200401.xls, CA200402.xls, ...CA200412.xls can be created by a
command such as:

```sql
create table ca04mul (Date char(19), Operation varchar(64),
  Debit double(15,2), Credit double(15,2))
engine=CONNECT table_type=ODBC multiple=1
qchar= '"' tabname='bank account'
connection='DSN=Excel Files;DBQ=D:/Ber/CA/CA2004*.xls;';
```

Providing that in each file the applying information is internally set for
Excel as a table named "bank account". This extension to ODBC does not support
 <em>multiple</em>=2. The <em>qchar</em> option was specified to make the identifiers
quoted in the select statement sent to ODBC, in particular the when the table
or column names contain blanks, to avoid SQL syntax errors.

<strong>Caution:</strong> Avoid accessing tables belonging to the currently running MariaDB server via the MySQL ODBC connector. This may not work and may cause the server to be restarted.

## Performance consideration

To avoid extracting entire tables from an ODBC source, which can be a lengthy
process, CONNECT extracts the "compatible" part of query WHERE clauses and adds
it to the ODBC query. Compatible means that it must be understood by the data
source. In particular, clauses involving scalar functions are not kept because
the data source may have different functions than MariaDB or use a different
syntax. Of course, clauses involving sub-select are also skipped. This will
transfer eventual indexing to the data source.

Take care with clauses involving string items because you may not know whether
they are treated by the data source as case sensitive or case insensitive. If in
doubt, make your queries as if the data source was processing strings as case
sensitive to avoid incomplete results.

## Using ODBC Tables inside correlated sub-queries

Unlike not correlated subqueries that are executed only once, correlated subqueries are executed many
times. It is what ODBC calls a "requery". Several methods can be used by CONNECT to deal with this
depending on the setting of the MEMORY or SCROLLABLE Boolean options:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td>Default</td><td>Implementing "requery" by discarding the current result set and re submitting the query (as MFC does)</td></tr>
<tr><td>Memory=1 or 2</td><td>Storing the result set in memory as MYSQL tables do.</td></tr>
<tr><td>Scrollable=Yes</td><td>Using a scrollable cursor.</td></tr>
</tbody></table>

Note: the MEMORY and SCROLLABLE options must be specified in the OPTION _ LIST.

Because the table is accessed several times, this can make queries last very long except for small tables
and is almost unacceptable for big tables. However, if it cannot be avoided, using the memory method
is the best choice and can be more than four times faster than the default method. If it is supported by the driver, using a scrollable cursor is slightly slower than using memory but can be an alternative to avoid memory problems when the sub-query returns a huge result set.

If the result set is of reasonable size, it is also possible to specify the block_size option equal or slightly larger than the result set. The whole result set being read on the first fetch, can be accessed many times without having to do anything else.

Another good workaround is to replace within the correlated sub-query the ODBC table by a local copy of it because MariaDB is often able to optimize the query and to provide a very fast execution.

## Accessing specified views

Instead of specifying a source table name via the TABNAME option, it is
possible to retrieve data from a “view” whose definition is given in a new
option SRCDEF. For instance:

```sql
CREATE TABLE custnum (
  country varchar(15) NOT NULL,
  customers int(6) NOT NULL)
ENGINE=CONNECT TABLE_TYPE=ODBC BLOCK_SIZE=10
CONNECTION='DSN=MS Access Database;DBQ=C:/Program Files/Microsoft Office/Office/1033/FPNWIND.MDB;'
SRCDEF='select country, count(*) as customers from customers group by country';
```

Or simply, because CONNECT can retrieve the returned column definition:

```sql
CREATE TABLE custnum ENGINE=CONNECT TABLE_TYPE=ODBC BLOCK_SIZE=10
CONNECTION='DSN=MS Access Database;DBQ=C:/Program Files/Microsoft Office/Office/1033/FPNWIND.MDB;'
SRCDEF='select country, count(*) as customers from customers group by country';
```

Then, when executing for instance:

```sql
select * from custnum where customers > 3;
```

The processing of the group by is done by the data source, which returns only
the generated result set on which only the where clause is performed locally.
The result:

<table><tbody><tr><th>country</th><th>customers</th></tr>
<tr><td>Brazil</td><td>9</td></tr>
<tr><td>France</td><td>11</td></tr>
<tr><td>Germany</td><td>11</td></tr>
<tr><td>Mexico</td><td>5</td></tr>
<tr><td>Spain</td><td>5</td></tr>
<tr><td>UK</td><td>7</td></tr>
<tr><td>USA</td><td>13</td></tr>
<tr><td>Venezuela</td><td>4</td></tr>
</tbody></table>

This makes possible to let the data source do complicated operations, such as
joining several tables or executing procedures returning a result set. This
minimizes the data transfer through ODBC.

## Data Modifying Operations

The only data modifying operations are the [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) , [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) and [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) commands.
They can be executed successfully only if the data source database or tables
are not read/only.

### INSERT Command

When inserting values to an ODBC table, local values are used and sent to the
ODBC table. This does not make any difference when the values are constant but
in a query such as:

```sql
insert into t1 select * from t2;
```

Where t1 is an ODBC table, t2 is a locally defined table that must exist on the
local server. Besides, it is a good way to create a distant ODBC table from
local data.

CONNECT does not directly support INSERT commands such as:

```sql
insert into t1 values(2,'Deux') on duplicate key update msg = 'Two';
```

Sure enough, the “on duplicate key update” part of it is ignored, and will
result in error if the key value is duplicated.

### UPDATE and DELETE Commands

Unlike the [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) command, [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) and [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) are supported in a simplified way. Only simple table commands are supported; CONNECT does not support multi-table commands, commands sent from a procedure, or issued via a trigger.
These commands are just rephrased to correspond to the data source syntax and sent to the
data source for execution. Let us suppose we created the table:

```sql
create table tolite (
  id int(9) not null,
  nom varchar(12) not null,
  nais date default null,
  rem varchar(32) default null)
ENGINE=CONNECT TABLE_TYPE=ODBC tabname='lite'
CONNECTION='DSN=SQLite3 Datasource;Database=test.sqlite3'
CHARSET=utf8 DATA_CHARSET=utf8;
```

We can populate it by:

```sql
insert into tolite values(1,'Toto',now(),'First'),
(2,'Foo','2012-07-14','Second'),(4,'Machin','1968-05-30','Third');
```

The function `now()` will be executed by MariaDB and it returned value sent
to the ODBC table.

Let us see what happens when updating the table. If we use the query:

```sql
update tolite set nom = 'Gillespie' where id = 10;
```

CONNECT will rephrase the command as:

```sql
update lite set nom = 'Gillespie' where id = 10;
```

What it did is just to replace the local table name with the remote table name
and change all the back ticks to blanks or to the data source identifier quoting characters if QUOTED is specified.
Then this command will be sent to the data source to be executed by it.

This is simpler and can be faster than doing a positional update using a cursor
and commands such as “select ... for update of ...” that are not supported by
all data sources. However, there are some restrictions that must be understood
due to the way it is handled by MariaDB.

1 MariaDB does not know about all the above. The command will be parsed as if
  it were to be executed locally. Therefore, it must respect the MariaDB syntax.
2 Being executed by the data source, the (rephrased) command must also respect
  the data source syntax.
3 All data referenced in the SET and WHERE clause belongs to the data source.

This is possible because both MariaDB and the data source are using the SQL
language. But you must use only the basic features that are part of the core
SQL language. For instance, keywords like IGNORE or LOW_PRIORITY will cause
syntax error with many data source.

Scalar function names also can be different, which severely restrict the use of
them. For instance:

```sql
update tolite set nais = now() where id = 2;
```

This will not work with SQLite3, the data source returning an “unknown scalar
function” error message. Note that in this particular case, you can rephrase it
to:

```sql
update tolite set nais = date('now') where id = 2;
```

This understood by both parsers, and even if this function would return NULL
executed by MariaDB, it does return the current date when executed by SQLite3.
But this begins to become too trickery so to overcome all these restrictions,
and permit to have all types of commands executed by the data source, CONNECT
provides a specific ODBC table subtype described now.

## Sending commands to a Data Source

This can be done using a special subtype of ODBC table. Let us see this in an
example:

```sql
create table crlite (
  command varchar(128) not null,
  number int(5) not null flag=1,
  message varchar(255) flag=2)
engine=connect table_type=odbc
connection='Driver=SQLite3 ODBC Driver;Database=test.sqlite3;NoWCHAR=yes'
option_list='Execsrc=1';
```

The key points in this create statement are the EXECSRC option and the column
definition.

The EXECSRC option tells that this table will be used to send a command to the
data source. Most of the sent commands do not return result set. Therefore, the
table columns are used to specify the command to be executed and to get the
result of the execution. The name of these columns can be chosen arbitrarily,
their function coming from the FLAG value:

<table><tbody><tr><td>Flag=0:</td><td>The command to execute.</td></tr>
<tr><td>Flag=1:</td><td>The affected rows, or -1 in case of error, or the result number of column if the command returns a result set.</td></tr>
<tr><td>Flag=2:</td><td>The returned (eventually error) message.</td></tr>
</tbody></table>

How to use this table and specify the command to send? By executing a command
such as:

```sql
select * from crlite where command = 'a command';
```

This will send the command specified in the WHERE clause to the data source and
return the result of its execution. The syntax of the WHERE clause must be
exactly as shown above. For instance:

```sql
select * from crlite where command =
'CREATE TABLE lite (
ID integer primary key autoincrement,
name char(12) not null,
birth date,
rem varchar(32))';
```

This command returns:

<table><tbody><tr><th>command</th><th>number</th><th>message</th></tr>
<tr><td><code>CREATE TABLE lite (ID integer primary key autoincrement, name...</code></td><td>0</td><td>Affected rows</td></tr>
</tbody></table>

Now we can create a standard ODBC table on the newly created table:

```sql
CREATE TABLE tlite
ENGINE=CONNECT TABLE_TYPE=ODBC tabname='lite'
CONNECTION='Driver=SQLite3 ODBC Driver;Database=test.sqlite3;NoWCHAR=yes'
CHARSET=utf8 DATA_CHARSET=utf8;
```

We can populate it directly using the supported [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) statement:

```sql
insert into tlite(name,birth) values('Toto','2005-06-12');
insert into tlite(name,birth,rem) values('Foo',NULL,'No ID');
insert into tlite(name,birth) values('Truc','1998-10-27');
insert into tlite(name,birth,rem) values('John','1968-05-30','Last');
```

And see the result:

```sql
select * from tlite;
```

<table><tbody><tr><th>ID</th><th>name</th><th>birth</th><th>rem</th></tr>
<tr><td>1</td><td>Toto</td><td>2005-06-12</td><td>NULL</td></tr>
<tr><td>2</td><td>Foo</td><td>NULL</td><td>No ID</td></tr>
<tr><td>3</td><td>Truc</td><td>1998-10-27</td><td>NULL</td></tr>
<tr><td>4</td><td>John</td><td>1968-05-30</td><td>Last</td></tr>
</tbody></table>

Any command, for instance [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/), can be executed from the <em>crlite</em> table:

```sql
select * from crlite where command =
'update lite set birth = ''2012-07-14'' where ID = 2';
```

This command returns:

<table><tbody><tr><th>command</th><th>number</th><th>message</th></tr>
<tr><td><code>update lite set birth = '2012-07-15' where ID = 2</code></td><td>1</td><td>Affected rows</td></tr>
</tbody></table>

Let us verify it:

```sql
select * from tlite where ID = 2;
```

<table><tbody><tr><th>ID</th><th>name</th><th>birth</th><th>rem</th></tr>
<tr><td>2</td><td>Foo</td><td>2012-07-15</td><td>No ID</td></tr>
</tbody></table>

The syntax to send a command is rather strange and may seem unnatural. It is possible to use an easier
syntax by defining a stored procedure such as:

```sql
create procedure send_cmd(cmd varchar(255))
MODIFIES SQL DATA
select * from crlite where command = cmd;
```

Now you can send commands like this:

```sql
call send_cmd('drop tlite');
```

This is possible only when sending one single command.

### Sending several commands together

Grouping commands uses an easier syntax and is faster because only one
connection is made for the all of them. To send several commands in one call,
use the following syntax:

```sql
select * from crlite where command in (
  'update lite set birth = ''2012-07-14'' where ID = 2',
  'update lite set birth = ''2009-08-10'' where ID = 3');
```

When several commands are sent, the execution stops at the end of them or after
a command that is in error. To continue after <em>n</em> errors, set the option
maxerr=<em>n</em> (0 by default) in the option list.

<strong>Note 1:</strong> It is possible to specify the SRCDEF option when creating an
EXECSRC table. It will be the command sent by default when a WHERE clause is
not specified.

<strong>Note 2:</strong> Most data sources do not allow sending several commands separated
by semi-colons.

<strong>Note 3:</strong> Quotes inside commands must be escaped. This can be avoided by
using a different quoting character than the one used in the command

<strong>Note 4:</strong> The sent command must obey the data source syntax.

<strong>Note 5:</strong> Sent commands apply in the specified database. However, they can
address any table within this database, or belonging to another database using
the name syntax <em>schema.tabname</em>.

## Connecting to a Data Source

There are two ways to establish a connection to a data source:

1 Using SQLDriverConnect and a Connection String
2 Using SQLConnect and a Data Source Name (DSN)

The first way uses a Connection String whose components describe what is needed to establish the connection. It is the most complete way to do it and by default CONNECT uses it.

The second way is a simplified way in which ODBC is just given the name of a DSN that must have been defined to ODBC or UnixOdbc and that contains the necessary information to establish the connection. Only the user name and password can be specified out of the DSN specification.

### Defining the Connection String

Using the first way, the connection string must be specified. This is sometimes the most difficult task when creating ODBC tables because, depending on the
operating system and the data source, this string can widely differ.

The format of the ODBC Connection String is:

```sql
connection-string::= empty-string[;] | attribute[;] | attribute; connection-string
empty-string ::=
attribute ::= attribute-keyword=attribute-value | DRIVER=[{]attribute-value[}]
attribute-keyword ::= DSN | UID | PWD | driver-defined-attribute-keyword
attribute-value ::= character-string
driver-defined-attribute-keyword = identifier
```

Where character-string has zero or more characters; identifier has one or more
characters; attribute- keyword is not case-sensitive; attribute-value may be
case-sensitive; and the value of the DSN keyword does not consist solely of
blanks. Due to the connection string grammar, keywords and attribute values
that contain the characters `[]{}(),;?*=!@` should be avoided. The value of
the DSN keyword cannot consist only of blanks, and should not contain leading
blanks. Because of the grammar of the system information, keywords and data
source names cannot contain the backslash (\) character. Applications do not
have to add braces around the attribute value after the DRIVER keyword unless
the attribute contains a semicolon (;), in which case the braces are required.
If the attribute value that the driver receives includes the braces, the driver
should not remove them, but they should be part of the returned connection
string.

### ODBC Defined Connection Attributes

The ODBC defined attributes are:

- DSN - the name of the data source to connect to. You must create this before attempting to refer to it. You create new DSNs
  through the ODBC Administrator (Windows), ODBCAdmin (unixODBC's GUI manager)
  or in the odbc.ini file.
- DRIVER - the name of the driver to connect to. You can use this in DSN-less
  connections.
- FILEDSN - the name of a file containing the connection attributes.
- UID/PWD - any username and password the database requires for authentication.
- SAVEFILE - request the DSN attributes are saved in this file.

Other attributes are DSN dependent attributes. The connection string can give
the name of the driver in the DRIVER field or the data source in the DSN field
(attention! meet the spelling and case) and has other fields that depend on the
data source. When specifying a file, the DBQ field must give the <strong>full</strong> path
and name of the file containing the table. Refer to the specific ODBC connector
documentation for the exact syntax of the connection string.

### Using a Predefined DSN

This is done by specifying in the option list the Boolean option “UseDSN” as yes or 1. In addition, string options “user” and “password” can be optionally specified in the option list.

When doing so, the connection string just contains the name of the predefined Data Source. For instance:

```sql
CREATE TABLE tlite ENGINE=CONNECT TABLE_TYPE=ODBC tabname='lite'
CONNECTION='SQLite3 Datasource' 
OPTION_LIST='UseDSN=Yes,User=me,Password=mypass';
```

Note: the connection data source name (limited to 32 characters) should not be preceded by “DSN=”.

## ODBC Tables on Linux/Unix

In order to use ODBC tables, you will need to have unixODBC installed. Additionally, you will need the ODBC driver for your foreign server's protocol. For example, for MS SQL Server or Sybase, you will need to have FreeTDS installed.

Make sure the user running mysqld (usually the mysql user) has permission to the ODBC data source configuration and the ODBC drivers.
If you get an error on Linux/Unix when using TABLE_TYPE=ODBC:

```sql
Error Code: 1105 [unixODBC][Driver Manager]Can't open lib
'/usr/cachesys/bin/libcacheodbc.so' : file not found
```

You must make sure that the user running mysqld (usually "mysql") has enough
permission to load the ODBC driver library. It can happen that the driver file
does not have enough read privileges (use chmod to fix this), or loading is
prevented by SELinux configuration (see below).

Try this command in a shell to check if the driver had enough permission:

```sql
sudo -u mysql ldd /usr/cachesys/bin/libcacheodbc.so
```

#### SELinux

SELinux can cause various problems. If you think SELinux is causing problems, check the system log (e.g. /var/log/messages) or the audit log (e.g. /var/log/audit/audit.log).

<strong>mysqld can't load some executable code, so it can't use the ODBC driver.</strong>

Example error:

```sql
Error Code: 1105 [unixODBC][Driver Manager]Can't open lib
'/usr/cachesys/bin/libcacheodbc.so' : file not found
```

Audit log:

```sql
type=AVC msg=audit(1384890085.406:76): avc: denied { execute }
for pid=1433 comm="mysqld"
path="/usr/cachesys/bin/libcacheodbc.so" dev=dm-0 ino=3279212
scontext=unconfined_u:system_r:mysqld_t:s0
tcontext=unconfined_u:object_r:usr_t:s0 tclass=file
```

<strong>mysqld can't open TCP sockets on some ports, so it can't connect to the foreign server.</strong>

Example error:

```sql
ERROR 1296 (HY000): Got error 174 '[unixODBC][FreeTDS][SQL Server]Unable to connect to data source' from CONNECT
```

Audit log:

```sql
type=AVC msg=audit(1423094175.109:433): avc:  denied  { name_connect } for  pid=3193 comm="mysqld" dest=1433 scontext=system_u:system_r:mysqld_t:s0 tcontext=system_u:object_r:mssql_port_t:s0 tclass=tcp_socket
```

## ODBC Catalog Information

Depending on the version of the used ODBC driver, some additional information
on the tables are existing, such as table QUALIFIER or OWNER for old versions,
now named CATALOG or SCHEMA since version 3.

CATALOG is apparently rarely used by most data sources, but SCHEMA (formerly
OWNER) is and corresponds to the DATABASE information of MySQL.

The issue is that if no schema name is specified, some data sources return
information for all schemas while some others only return the information of
the “default” schema. In addition, the used “schema” or “database” is sometimes
implied by the connection string and sometimes is not. Sometimes, it also can
be included in a data source definition.

CONNECT offers two ways to specify this information:

1 When specified, the DBNAME create table option is regarded by ODBC tables as
  the SCHEMA name.
2 Table names can be specified as “<em>cat.sch.tab</em>” allowing to set the catalog and
  schema info.

When both are used, the qualified table name has precedence over DBNAME . For
instance:

<table><tbody><tr><th>Tabname</th><th>DBname</th><th>Description</th></tr>
<tr><td>test.t1</td><td></td><td>The t1 table of the test schema.</td></tr>
<tr><td>test.t1</td><td>mydb</td><td>The t1 table of the test schema (test has precedence)</td></tr>
<tr><td>t1</td><td>mydb</td><td>The t1 table of the mydb schema</td></tr>
<tr><td>%.%.%</td><td></td><td>All tables in all catalogs and all schemas</td></tr>
<tr><td>t1</td><td></td><td>The t1 table in the default or all schema depending on the DSN</td></tr>
<tr><td>%.t1</td><td></td><td>The t1 table in all schemas for all DSN</td></tr>
<tr><td>test.%</td><td></td><td>All tables in the test schema</td></tr>
</tbody></table>

When creating a standard ODBC table, you should make sure only one source table
is specified.  Specifying more than one source table must be done only for
CONNECT catalog tables (with CATFUNC=tables or columns).

In particular, when column definition is left to the Discovery feature, if tables with the same name are present in several schemas and the schema name is not specified, several columns with the same name will be generated. This will make the creation fail with a not very explicit error message.

Note: With some ODBC drivers, the DBNAME option or qualified table name is useless because the
schema implied by the connection string or the definition of the data source has priority over the
specified DBNAME .

### Table name case

Another issue when dealing with ODBC tables is the way table and column names
are handled regarding of the case.

For instance, Oracle follows to the SQL standard here. It converts non-quoted
identifiers to upper case. This is correct and expected. PostgreSQL is not
standard. It converts identifiers to lower case.  MySQL/MariaDB is not
standard. They preserve identifiers on Linux, and convert to lower case on
Windows.

Think about that if you fail to see a table or a column on an ODBC data source.

## Non-ASCII Character Sets with Oracle

When connecting through ODBC, the MariaDB Server operates as a client to the foreign database management system. As such, it requires that you configure MariaDB as you would configure native clients for the given database server.

In the case of connecting to Oracle, when using non-ASCI character sets, you need to properly set the&nbsp;NLS_LANG environment variable before starting the MariaDB Server.

For instance, to test this on Oracle, create a table that contains a series of special characters:

```sql
CREATE TABLE t1 (letter VARCHAR(4000));

INSERT INTO t1 VALUES
   (UTL_RAW.CAST_TO_VARCHAR2(HEXTORAW('C4'))),
   (UTL_RAW.CAST_TO_VARCHAR2(HEXTORAW('C5'))),
   (UTL_RAW.CAST_TO_VARCHAR2(HEXTORAW('C6')));

SELECT letter, RAWTOHEX(letter) FROM t1;

letter | RAWTOHEX(letter)
-------|-----------------
Ä     | C4
Å     | C5
Æ     | C6
```

Then create a connecting table on MariaDB and attempt the same query:

```sql
CREATE TABLE t1 (
   letter VARCHAR(4000))
ENGINE=CONNECT
DEFAULT CHARSET=utf8mb4
CONNECTION='DSN=YOUR_DSN'
TABLE_TYPE = 'ODBC'
DATA_CHARSET = latin1
TABNAME = 'YOUR_SCHEMA.T1';

SELECT letter, HEX(letter) FROM t1;

+--------+-------------+
| letter | HEX(letter) |
+--------+-------------+
| A      | 	    41 |
| ?      | 	    3F |
| ?      | 	    3F |
+--------+-------------+
```

While the character set is defined in a way that satisfies MariaDB, it has not been defined for Oracle, (that is, setting the&nbsp;NLS_LANG&nbsp;environment variable). As a result, Oracle is not providing the characters you want to MariaDB and Connect.
The specific method of setting the&nbsp;NLS_LANG&nbsp;variable can vary depending on your operating system or distribution. If you're experiencing this issue, check your OS documentation for more details on how to properly set environment variables.

### Using systemd

With Linux distributions that use [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/), you need to set the environment variable in the service file, (systemd doesn't read from the&nbsp;/etc/environment&nbsp;file).

This is done by setting the&nbsp;Environment&nbsp;variable in the&nbsp;[Service]&nbsp;unit. For instance,

```sql
# vi /usr/lib/systemd/system/mariadb.service

[Service]
...
Environment=NLS_LANG=GERMAN_GERMANY.WE8ISO8859P1
```

Once this is done, reload the systemd units:

```sql
# systemctl daemon-reload
```

Then restart MariaDB,

```sql
# systemctl restart mariadb.service
```

You can now retrieve the appropriate characters from Oracle tables:

```sql
SELECT letter, HEX(letter) FROM t1;

+--------+-------------+
| letter | HEX(letter) |
+--------+-------------+
| Ä      | C384        |
| Å      | C385        |
| Æ      | C386        |
+--------+-------------+
```

### Using Windows

Microsoft Windows doesn't ignore environment variables the way systemd does on Linux, but it does require that you set the&nbsp;NLS_LANG&nbsp;environment variable on your system. In order to do so, you need to open an elevated command-prompt, (that is,&nbsp;Cmd.exe&nbsp;with administrative privileges).

From here, you can use the&nbsp;Setx&nbsp;command to set the variable. For instance,

```sql
Setx NLS_LANG GERMAN_GERMANY.WE8ISO8859P1 /m
```

Note: For more detail about this, see [MDEV-17501](https://jira.mariadb.org/browse/MDEV-17501).