# CONNECT Table Types - Special "Virtual" Tables

The special table types supported by CONNECT are the Virtual table type ([VIR](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-table-types-vir/) - introduced in [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/)), Directory Listing table type (DIR), the Windows Management Instrumentation Table Type (WMI), and the “Mac Address” type (MAC).

These tables are “virtual tables”, meaning they have no physical data but rather produce result data using specific algorithms. Note that this is close to what Views are, so they could be regarded as special views.

## DIR Type

A table of type DIR returns a list of file name and description as a result set. To create a DIR table, use a Create Table statement such as:

```sql
create table source (
  DRIVE char(2) NOT NULL,
  PATH varchar(256) NOT NULL,
  FNAME varchar(256) NOT NULL,
  FTYPE char(4) NOT NULL,
  SIZE double(12,0) NOT NULL flag=5,
  MODIFIED datetime NOT NULL)
engine=CONNECT table_type=DIR file_name='..\\*.cc';
```

When used in a query, the table returns the same file information listing than the system "DIR `*.cc`" statement would return if executed in the same current directory (here supposedly ..\)

For instance, the query:

```sql
select fname, size, modified from source
  where fname like '%handler%';
```

Displays:

<table><tbody><tr><th>fname</th><th>size</th><th>modified</th></tr>
<tr><td>handler</td><td>152177</td><td>2011-06-13 18:08:29</td></tr>
<tr><td>sql_handler</td><td>25321</td><td>2011-06-13 18:08:31</td></tr>
</tbody></table>

<strong>Note:</strong> the important item in this table is the flag option value (set sequentially from 0 by default) because it determines which particular information item is returned in the column:

<table><tbody><tr><th>Flag value</th><th>Information</th></tr>
<tr><td>0</td><td>The disk drive (Windows)</td></tr>
<tr><td>1</td><td>The file path</td></tr>
<tr><td>2</td><td>The file name</td></tr>
<tr><td>3</td><td>The file type</td></tr>
<tr><td>4</td><td>The file attribute</td></tr>
<tr><td>5</td><td>The file size</td></tr>
<tr><td>6</td><td>The last write access date</td></tr>
<tr><td>7</td><td>The last read access date</td></tr>
<tr><td>8</td><td>The file creation date</td></tr>
</tbody></table>

### The Subdir option

When specified in the create table statement, the subdir option indicates to list, in addition to the files contained in the specified directory, all the files verifying the filename pattern that are contained in sub-directories of the specified directory. For instance, using:

```sql
create table data (
  PATH varchar(256) NOT NULL flag=1,
  FNAME varchar(256) NOT NULL,
  FTYPE char(4) NOT NULL,
  SIZE double(12,0) NOT NULL flag=5)
engine=CONNECT table_type=DIR file_name='*.frm'
option_list='subdir=1';

select path, count(*), sum(size) from data group by path;
```

You will get the following result set showing how many tables are created in the MariaDB databases and what is the total length of the FRM files:

<table><tbody><tr><th>path</th><th>count(*)</th><th>sum(size)</th></tr>
<tr><td>\CommonSource\mariadb-5.2.7\sql\data\connect\</td><td>30</td><td>264469</td></tr>
<tr><td>\CommonSource\mariadb-5.2.7\sql\data\mysql\</td><td>23</td><td>207168</td></tr>
<tr><td>\CommonSource\mariadb-5.2.7\sql\data\test\</td><td>22</td><td>196882</td></tr>
</tbody></table>

### The Nodir option (Windows)

The Boolean Nodir option can be set to false (0 or no) to add directories that match the file name pattern from the listed files (it is true by default). This is an addition to CONNECT version 1.6. Previously, directory names matching pattern were listed on Windows. Directories were and are never listed on Linux.

Note: The way file names are retrieved makes positional access to them impossible. Therefore, DIR tables cannot be indexed or sorted when it is done using positions.

Be aware, in particular when using the subdir option, that queries on DIR tables are slow and can last almost forever if made on a directory that contains a great number of files in it and its sub-directories.

dir tables can be used to populate a list of files used to create a multiple=2 table. However, this is not as useful as it was when the multiple 3 did not exist.

## Windows Management Instrumentation Table Type “WMI”

<strong>Note:</strong> This table type is available on Windows only.

WMI provides an operating system interface through which instrumented components provide information. Some Microsoft tools to retrieve information through WMI are the WMIC console command and the WMI CMI Studio application.

The CONNECT WMI table type enables administrators and operators not capable of scripting or programming on top of WMI to enjoy the benefit of WMI without even learning about it. It permits to present this information as tables that can be queried, transformed, copied in documents or other tables.

To create a WMI table displaying information coming from a WMI provider, you must provide the namespace and the class name that characterize the information you want to retrieve. The best way to find them is to use the WMI CIM Studio that have tools to browse namespaces and classes and that can display the names of the properties of that class.

The column names of the tables must be the names (case insensitive) of the properties you want to retrieve. For instance:

```sql
create table alias (
  friendlyname char(32) not null,
  target char(50) not null)
engine=CONNECT table_type='WMI'
option_list='Namespace=root\\cli,Class=Msft_CliAlias';
```

WMI tables returns one row for each instance of the related information. The above example is handy to get the class equivalent of the alias of the WMIC command and also to have a list of many classes commonly used.

Because most of the useful classes belong to the 'root\cimv2' namespace, this
is the default value for WMI tables when the namespace is not specified. Some
classes have many properties whose name and type may not be known when creating
the table. To find them, you can use the WMI CMI Studio application but his
will be rarely required because CONNECT is able to retrieve them.

Actually, the class specification also has default values for some namespaces.
For the ‘root\cli’ namespace the class name defaults to ‘Msft_CliAlias’ and for
the ‘root_cimv2’ namespace the class default value is
‘Win32_ComputerSystemProduct’. Because many class names begin with ‘Win32_’ it
is not necessary to say it and specifying the class as ‘Product’ will
effectively use class ‘Win32_Product’.

For example if you define a table as:

```sql
create table CSPROD engine=CONNECT table_type='WMI';
```

It will return the information on the current machine, using the class
ComputerSystemProduct of the CIMV2 namespace. For instance:

```sql
select * from csprod;
```

Will return a result such as:

<table><tbody><tr><th>Column</th><th>Row 1</th></tr>
<tr><td>Caption</td><td>Computer system product</td></tr>
<tr><td>Description</td><td>Computer system product</td></tr>
<tr><td>IdentifyingNumber</td><td>LXAP50X32982327A922300</td></tr>
<tr><td>Name</td><td>Aspire 8920</td></tr>
<tr><td>SKUNumber</td><td></td></tr>
<tr><td>UUID</td><td>00FC523D-B8F7-DC12-A70E-00B0D1A46136</td></tr>
<tr><td>Vendor</td><td>Acer</td></tr>
<tr><td>Version</td><td>Aspire 8920</td></tr>
</tbody></table>

<strong>Note:</strong> This is a transposed display that can be obtained with some GUI.

### Getting column information

An issue, when creating a WMI table, is to make its column definition. Indeed,
even when you know the namespace and the class for the wanted information, it
is not easy to find what are the names and types of its properties. However,
because CONNECT can retrieve this information from the WMI provider, you can
simply omit defining columns and CONNECT will do the job.

Alternatively, you can get this information using a catalog table (see below).

### Performance Consideration

Some WMI providers can be very slow to answer. This is not an issue for those
that return few object instances, such as the ones returning computer,
motherboard, or Bios information. They generally return only one row
(instance). However, some can return many rows, in particular the
"CIM_DataFile" class. This is why care must be taken about them.

Firstly, it is possible to limit the allocated result size by using the
‘Estimate’ create table option. To avoid result truncation, CONNECT allocates a
result of 100 rows that is enough for almost all tables.The 'Estimate' option
permits to reduce this size for all classes that return only a few rows, and in
some rare case to increase it to avoid truncation.

However, it is not possible to limit the time taken by some WMI providers to
answer, in particular the CIM_DATAFILE class. Indeed the Microsoft
documentation says about it:

"Avoid enumerating or querying for all instances of CIM_DataFile on a
 computer because the volume of data is likely to either affect performance or
 cause the computer to stop responding."

Sure enough, even a simple query such as:

```sql
select count(*) from cim where drive = 'D:' and path like '\\MariaDB\\%';
```

is prone to last almost forever (probably due to the LIKE clause). This is why,
when not asking for some specific items, you should consider using the DIR
table type instead.

### Syntax of WMI queries

Queries to WMI providers are done using the WQL language, not the SQL language.
CONNECT does the job of making the WQL query. However, because of the
restriction of the WQL syntax, the WHERE clause will be generated only when
respecting the following restrictions:

1 No function.
2 No comparison between two columns.
3 No expression (currently a CONNECT restriction)
4 No BETWEEN and IN predicates.

Filtering with WHERE clauses not respecting these conditions will still be done by MariaDB only,
except in the case of CIM_Datafile class for the reason given above.

However, there is one point that is not covered yet, the syntax used to specify dates in queries. WQL does not recognize dates as number items but translates them to its internal format dates specified as text. Many formats are recognized as described in the Microsoft documentation but only one is useful because common to WQL and MariaDB SQL. Here is an example of a query on a table named "cim" created by:

```sql
create table cim (
  Name varchar(255) not null,
  LastModified datetime not null)
engine=CONNECT table_type='WMI'
option_list='class=CIM_DataFile,estimate=5000';
```

The date must be specified with the format in which CIM DATETIME values are stored (WMI uses the date and time formats defined by the Distributed Management Task Force).

```sql
select * from cim where drive = 'D:' and path = '\\PlugDB\\Bin\\'
     and lastmodified > '20120415000000.000000+120';
```

This syntax must be strictly respected. The text has the format:

```sql
yyyymmddHHMMSS.mmmmmmsUUU
```

It is: year, month, day, hour, minute, second, millisecond, and signed minute deviation from [UTC](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/coordinated-universal-time/). This format is locale-independent so you can write a query that runs on any machine.

<strong>Note 1:</strong> The WMI table type is available only in Windows versions of CONNECT.

<strong>Note 2:</strong> WMI tables are read only.

<strong>Note 3:</strong> WMI tables are not indexable.

<strong>Note 4:</strong> WMI consider all strings as case insensitive.

## MAC Address Table Type “MAC”

<strong>Note:</strong> This table type is available on Windows only.

This type is used to display various general information about the computer and, in particular, about its network cards. To create such a table, the syntax to use is:

```sql
create table tabname (column definition)
engine=CONNECT table_type=MAC;
```

Column names can be freely chosen because their signification, i.e. the values they will display, comes from the specified Flag option. The valid values for Flag are:

<table><tbody><tr><th>Flag</th><th>Valeur</th><th>Type</th></tr>
<tr><td>1</td><td>Host name</td><td>varchar(132)</td></tr>
<tr><td>2</td><td>Domain</td><td>varchar(132)</td></tr>
<tr><td>3</td><td>DNS address</td><td>varchar(24)</td></tr>
<tr><td>4</td><td>Node type</td><td>int(1)</td></tr>
<tr><td>5</td><td>Scope ID</td><td>varchar(256)</td></tr>
<tr><td>6</td><td>Routing</td><td>int(1)</td></tr>
<tr><td>7</td><td>Proxy</td><td>int(1)</td></tr>
<tr><td>8</td><td>DNS</td><td>int(1)</td></tr>
<tr><td>10</td><td>Name</td><td>varchar(260)</td></tr>
<tr><td>11</td><td>Description</td><td>varchar(132)</td></tr>
<tr><td>12</td><td>MAC address</td><td>char(24)</td></tr>
<tr><td>13</td><td>Type</td><td>int(3)</td></tr>
<tr><td>14</td><td>DHCP</td><td>int(1)</td></tr>
<tr><td>15</td><td>IP address</td><td>char(16)</td></tr>
<tr><td>16</td><td>SUBNET mask</td><td>char(16)</td></tr>
<tr><td>17</td><td>GATEWAY</td><td>char(16)</td></tr>
<tr><td>18</td><td>DHCP server</td><td>char(16)</td></tr>
<tr><td>19</td><td>Have WINS</td><td>int(1)</td></tr>
<tr><td>20</td><td>Primary WINS</td><td>char(16)</td></tr>
<tr><td>21</td><td>Secondary WINS</td><td>char(16)</td></tr>
<tr><td>22</td><td>Lease obtained</td><td>datetime</td></tr>
<tr><td>23</td><td>Lease expires</td><td>datetime</td></tr>
</tbody></table>

<strong>Note:</strong> The information of columns having a Flag value less than 10 are unique for the computer, the other ones are specific to the network cards of the computer.

For instance, you can define the table <em>macaddr</em> as:

```sql
create table macaddr (
  Host varchar(132) flag=1,
  Card varchar(132) flag=11,
  Address char(24) flag=12,
  IP char(16) flag=15,
  Gateway char(16) flag=17,
  Lease datetime flag=23)
engine=CONNECT table_type=MAC;
```

If you execute the query:

```sql
select host, address, ip, gateway, lease from macaddr;
```

It will return, for example:

<table><tbody><tr><th>Host</th><th>Address</th><th>IP</th><th>Gateway</th><th>Lease</th></tr>
<tr><td>OLIVIER</td><td>00-A0-D1-A4-61-36</td><td>0.0.0.0</td><td>0.0.0.0</td><td>1970-01-01 00:00:00</td></tr>
<tr><td>OLIVIER</td><td>00-1D-E0-9B-90-0B</td><td>192.168.0.10</td><td>192.168.0.254</td><td>2011-09-18 10:28:5</td></tr>
</tbody></table>