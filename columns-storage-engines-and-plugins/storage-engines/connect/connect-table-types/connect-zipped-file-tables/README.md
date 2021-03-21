# CONNECT Zipped File Tables

##### MariaDB starting with [10.2.4](/kb/en/mariadb-1024-release-notes/)

Connect can work on table files that are compressed in one or several zip files.

The specific options used when creating tables based on zip files are:

<table><tbody><tr><th>Table Option</th><th>Type</th><th>Description</th></tr>
<tr><td>ZIPPED</td><td>Boolean</td><td>Required to be set as true.</td></tr>
<tr><td>ENTRY*</td><td>String</td><td>The optional name or pattern of the zip entry or entries to be used with the table. If not specified, all entries or only the first one will be used depending on the <em>mulentries</em> option setting.</td></tr>
<tr><td>MULENTRIES*</td><td>Boolean</td><td>True if several entries are part of the table. If not specified, it defaults to false if the <em>entry</em> option is not specified. If the entry option is specified, it defaults to true if the entry name contains wildcard characters or false if it does not.</td></tr>
<tr><td>APPEND*</td><td>Boolean</td><td>Used when creating new zipped tables (see below)</td></tr>
<tr><td>LOAD*</td><td>String</td><td>Used when creating new zipped tables (see below)</td></tr>
</tbody></table>

Options marked with a ‘*’ must be specified in the option list.

### Examples: CONNECT CSV for Zipped File Tables

An example of a generic table definition, which contains some of the most common table_options used in CONNECT Table_Type=CSV Engine when dealing with Zipped File Tables, would be as follows:

```sql
CREATE [OR REPLACE] TABLE [IF NOT EXISTS] tbl_name
(
     ... optional column definition
)
     ENGINE=CONNECT
     TABLE_TYPE=CSV
     FILE_NAME='zipped_file_path'
     ZIPPED={0|1|NO|YES}
     [MULTIPLE={0|1|2|3}]
     [HEADER ={0|1|NO|YES}]
     [SEP_CHAR='{\t|;|,|other_sep_char}']
     [QCHAR = '{"|''|other_quotation_marks}']
     [QUOTED={0|1|2|3}]
     [READONLY={0|1|NO|YES}]
     [OPTION_LIST="[maxerr=n],[accept={0|1|NO|YES}],[entry='file_inside_zip'],[mulentries={0|1|NO|YES}],[load='load_file_path'],[append={0|1|NO|YES}]"]
```

Note that zipped_file_path can contain wildcards " * " when used with MULTIPLE={1|3}, a file path example would be as follows:
     C:/SubFolder/Folder/Filename*.zip

Multiple tables are specified by the option MULTIPLE=<em>n</em>, which can take
four values:

<table><tbody><tr><td>0</td><td>Not a multiple table (the default). This can be used in an alter table statement.</td></tr>
<tr><td>1</td><td>The table is made from files located in the same directory. The FILE_NAME option is a pattern such as <code>'cash*.log'</code> that all the table file path/names verify.</td></tr>
<tr><td>2</td><td>The FILE_NAME gives the name of a file that contains the path/names of all the table files. This file can be made using a <a href="/kb/en/connect-table-types-special-virtual-tables/#dir-type">DIR</a> table.</td></tr>
<tr><td>3</td><td>The table is made from files located in the same directory, and all its sub-directories (sub-foders). The FILE_NAME option is a pattern such as <code>'cash*.log'</code> that all the table file path/names verify.</td></tr>
</tbody></table>

The column descriptions can be retrieved by the discovery process for table types allowing it. It cannot be done for multiple tables or multiple entries.
A catalog table can be created by adding <em>catfunc=columns</em>. This can be used to show the column definitions of multiple tables. <em>Multiple</em> must be set to false and the column definitions will be the ones of the first table or entry.

This first implementation has some restrictions:

1 This is a read-only implementation. No insert, update or delete.
2 The inside files are decompressed into memory. Memory problems may arise with huge files.
3 Only file types that can be handled from memory are eligible for this. This includes [DOS](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-dos-and-fix-table-types), [FIX](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-dos-and-fix-table-types), [BIN](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-bin-table-type), [CSV](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-csv-and-fmt-table-types), [FMT](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-csv-and-fmt-table-types), [JSON](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-json-table-type), and [XML](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-xml-table-type) table types.

Optimization by indexing or block indexing is possible for table types supporting it. However, it applies on the uncompressed table. This means that the whole table is always uncompressed.

Partitioning is also supported. See how to do it in the section about partitioning.

Examples of use:

#### Example 1: Single CSV File included in a Single ZIP File

Let's suppose you have a CSV file from which you would create a table by:

```sql
create table emp
... optional column definition
engine=connect table_type=CSV file_name='E:/Data/employee.csv'
sep_char=';' header=1;
```

If the CSV file is included in a ZIP file, the CREATE TABLE becomes:

```sql
create table empzip
... optional column definition
engine=connect table_type=CSV file_name='E:/Data/employee.zip'
sep_char=';' header=1 zipped=1 option_list='Entry=emp.csv';
```

The <em>file_name</em> option is the name of the zip file. The <em>entry</em> option is the name of the entry inside the zip file. If there is only one entry file inside the zip file, this option can be omitted.

#### Example 2: Several CSV Files included in a Single ZIP File

If the table is made from several files such as emp01.csv, emp02.csv, etc., the standard create table would be:

```sql
create table empmul (
... required column definition
) engine=connect table_type=CSV file_name='E:/Data/emp*.csv' 
sep_char=';' header=1 multiple=1;
```

But if these files are all zipped inside a unique zip file, it becomes:

```sql
create table empzmul
... required column definition
engine=connect table_type=CSV file_name='E:/Data/emp.zip'
sep_char=';' header=1 zipped=1 option_list='Entry=emp*.csv';
```

Here the <em>entry</em> option is the pattern that the files inside the zip file must match. If all entry files are ok, the <em>entry</em> option can be omitted but the Boolean option <em>mulentries</em> must be specified as true.

#### Example 3: Single CSV File included in Multiple ZIP Files (Without considering subfolders)

If the table is created on several zip files, it is specified as for all other multiple tables:

```sql
create table zempmul (
... required column definition
) engine=connect table_type=CSV file_name='E:/Data/emp*.zip' 
sep_char=';' header=1 multiple=1 zipped=yes 
option_list='Entry=employee.csv';
```

Here again the <em>entry</em> option is used to restrict the entry file(s) to be used inside the zip files and can be omitted if all are ok.

### Creating new zipped tables

Tables can be created to access already existing zip files. However, is it also possible to make the zip file from an existing file or table. Two ways are available to make the zip file:

#### Insert method:

insert can be used to make the table file for table types based on records (this excludes XML and JSON when pretty is not 0). However, the current implementation of the used package (minizip) does not support adding to an already existing zip entry. This means that when executing an insert statement the inserted records are not added but replace the existing ones. CONNECT protects existing data by not allowing such inserts, Therefore, only three ways are available to do so:

1 Using only one insert statement to make the whole table. This is possible only for small tables and is principally useful when making tests.
2 Making the table from the data of another table. This can be done by executing an “insert into table select * from another_table” or by specifying “as select * from another_table” in the create table statement.
3 Making the table from a file whose format enables to use the “load data infile” statement.

To add a new entry in an existing zip file, specify “append=YES” in the option list. When inserting several entries, use ALTER to specify the required options, for instance:

```sql
create table znumul (
Chiffre int(3) not null,
Lettre char(16) not null)
engine=CONNECT table_type=CSV
file_name='C:/Data/FMT/mnum.zip' header=1 lrecl=20 zipped=1
option_list='Entry=Num1';
insert into znumul select * from num1;
alter table znumul option_list='Entry=Num2,Append=YES';
insert into znumul select * from num2;
alter table znumul option_list='Entry=Num3,Append=YES';
insert into znumul select * from num3;
alter table znumul option_list='Entry=Num*,Append=YES';
select * from znumul;
```

The last ALTER is needed to display all the entries.

#### File zipping method

This method enables to make the zip file from another file when creating the table. It applies to all table types including XML and JSON. It is specified in the create table statement with the load option. For example:

```sql
create table XSERVZIP (
NUMERO varchar(4) not null,
LIEU varchar(15) not null,
CHEF varchar(5) not null,
FONCTION varchar(12) not null,
NOM varchar(21) not null)
engine=CONNECT table_type=XML file_name='E:/Xml/perso.zip' zipped=1
option_list='entry=services,load=E:/Xml/serv2.xml';
```

When executing this statement, the <em>serv2.xml</em> file will be zipped as /perso.zip<em>. The entry name must be specified as well as the column descriptions that cannot be retrieved from the zip entry file that does not exist yet.</em>

It is also possible to create a multi-entries table from several files:

```sql
CREATE TABLE znewcities (
  _id char(5) NOT NULL,
  city char(16) NOT NULL,
  lat double(18,6) NOT NULL `FIELD_FORMAT`='loc:[0]',
  lng double(18,6) NOT NULL `FIELD_FORMAT`='loc:[1]',
  pop int(6) NOT NULL,
  state char(2) NOT NULL
) ENGINE=CONNECT TABLE_TYPE=JSON FILE_NAME='E:/Json/newcities.zip' ZIPPED=1 LRECL=1000 OPTION_LIST='Load=E:/Json/city_*.json,mulentries=YES,pretty=0';
```

Here the files to load are specified with wildcard characters and the <em>mulentries</em> options must be specified. However, the <em>entry</em> option must not be specified, entry names will be made from the file names.

### ZIP table type

A ZIP table type is also available. It is not meant to read the inside files but to display information about the zip file contents. For instance:

```sql
create table xzipinfo2 (
fn varchar(256)not null,
cmpsize bigint not null flag=1,
uncsize bigint not null flag=2,
method int not null flag=3,
date datetime not null flag=4)
engine=connect table_type=ZIP file_name='E:/Data/Json/cities.zip';
```

This will display the name, compressed size, uncompressed size, and compress method of all entries inside the zip file. Column names are irrelevant; these are flag values that mean what information to retrieve.