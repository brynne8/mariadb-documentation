# CONNECT MONGO Table Type: Accessing Collections from MongoDB

The source files of this new type are currently distributed only with MariaDB version 10.1, 10.2 and 10.3. This MONGO type is available only when compiling MariaDB from source with:

```sql
cmake -DCONNECT_WITH_MONGO=ON
```

Such a version will not be rated GA (stable) anymore.

Starting with CONNECT version 1.06.005 (see [versions](/columns-storage-engines-and-plugins/storage-engines/connect/)) the MONGO table type will be also available with binary distributions supporting JDBC. However, this version of CONNECT being rated as GA (stable), the MONGO table type will be disabled for MariaDB versions 10.0 and 10.1, not being thoroughly tested yet. It is still possible to enable it, for testing or use at user’s risk, when compiling MariaDB from source with the option: `CONNECT_WITH_MONGO ON`.

Classified as a&nbsp;NoSQL&nbsp;database program, MongoDB uses&nbsp;JSON-like documents (BSON) grouped in collections. The MONGO type is used to directly access MongoDB collections as tables.

## Accessing MongDB from CONNECT

Accessing MongoDB from CONNECT can be done in different ways:

1 As a MONGO table via the MongoDB C Driver.
2 As a MONGO table via the MongoDB Java Driver.
3 As a JDBC table using some commercially available MongoDB JDBC drivers.
4 As a JSON table via the MongoDB C or Java Driver.

#### Using the MongoDB C Driver

This is currently not available from binary distributions but only for versions compiled from source. The preferred version of the MongoDB C Driver is 1.7, because they provide package recognition. What must be done is:

1 Install libbson and the MongoDB C Driver 1.7.
2 Configure, compile and install MariaDB.

With earlier versions of the Mongo C Driver, the additional include directories and libraries will have to be specified manually when compiling.

When possible, this is the preferred means of access because it does not require all the Java path settings etc. and is faster than using the Java driver.

#### Using the Mongo Java Driver

This is possible with all distributions including JDBC support when using with a [MariaDB 10.2](/kb/en/what-is-mariadb-102/) or [MariaDB 10.3](/kb/en/what-is-mariadb-103/) binary distribution, or compiling from source. With a binary distribution that does not enable the MONGO table type, it is possible to access MongoDB using an OEM module. See [CONNECT OEM Table Example](/columns-storage-engines-and-plugins/storage-engines/connect/connect-oem-table-example/) for details. The only additional things to do are:

1 Install the MongoDB Java Driver by downloading its jar file. Several versions are available. If possible use the latest version 3 one.
2 Add the path to it in the CLASSPATH environment variable or in the connect_class_path variable. This is like what is done to declare JDBC drivers.

Connection is established by new Java wrappers Mongo3Interface and Mongo2Interface. They are available in a JDBC distribution in the JavaWrappers.jar file. If version 2 of the Java Driver is used, specify “Version=2” in the option list when creating tables.

#### Using JDBC

See the documentation of the existing commercial JDBC Mongo drivers.

#### Using JSON

See the specific chapter of the JSON Table Type.

The following describes the MONGO table type.

## CONNECT MONGO Tables

Creating and running MONGO tables requires a connection to a running local or remote MongoDB server.

A MONGO table is defined to access a MongoDB collection. The table rows will be the collection documents. For instance, to create a table based on the MongoDB sample collection restaurants, you can do something such as the following:

```sql
create table resto (
_id varchar(24) not null,
name varchar(64) not null,
cuisine char(200) not null,
borough char(16) not null,
restaurant_id varchar(12) not null)
engine=connect table_type=MONGO tabname='restaurants'
data_charset=utf8 connection='mongodb://localhost:27017';
```

Note: The used driver is by default the C driver if only the MongoDB C Driver is installed and the Java driver if only the MongoDB Java Driver is installed. If both are available, it can be specified by the DRIVER option to be specified in the option list and defaults to C.

Here we did not define all the items of the collection documents but only those that are JSON values. The database is test by default. The connection value is the URI used to establish a connection to a local or remote MongoDB server. The value shown in this example corresponds to a local server started with its default port. It is the default connection value for MONGO tables so we could have omit specifying it.

Using discovery is available. This table could have been created by:

```sql
create table resto
engine=connect table_type=MONGO tabname='restaurants'
data_charset=utf8 option_list='level=-1';
```

Here “level=-1” is used to create only columns that are simple values (no array or object). Without this, with the default value “level=0” the table had been created as:

```sql
CREATE TABLE `resto` (
  `_id` char(24) NOT NULL,
  `address` varchar(136) NOT NULL,
  `borough` char(13) NOT NULL,
  `cuisine` char(64) NOT NULL,
  `grades` varchar(638) NOT NULL,
  `name` char(98) NOT NULL,
  `restaurant_id` char(8) NOT NULL
) ENGINE=CONNECT DEFAULT CHARSET=latin1 `TABLE_TYPE`='MONGO' `TABNAME`='restaurants' `DATA_CHARSET`='utf8';
```

### Fixing Problems With mysqldump

In some case or some platforms, when CONNECT is set up for use with JDBC table types, this causes [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/) with the --all-databases option to fail.

This was reported by Robert Dyas who found the cause of it and how to fix it (see [MDEV-11238](https://jira.mariadb.org/browse/MDEV-11238)).

This occurs when the Java JRE “Usage Tracker” is enabled. In that case, Java creates a directory #mysql50#.oracle_jre_usage in the mysql data directory that shows up as a database but cannot be accessed via MySQL Workbench nor apparently backed up by mysqldump --all-databases.

Per the Oracle documentation ([https://docs.oracle.com/javacomponents/usage-tracker/overview/](https://docs.oracle.com/javacomponents/usage-tracker/overview/)) the 
“Usage Tracker” is disabled by default. It is enabled only when creating the properties file &lt;JRE directory&gt;/lib/management/usagetracker.properties. This turns out to be WRONG on some platforms as the file does exist by default on a new installation, and the existence of this file enables the usage tracker.

The solution on CentOS 7 with the Oracle JVM is to rename or delete the usagetracker.properties file (to disable it) and then delete the bogus folder it created in the mysql database directory, then restart.

For example, the following works:

```sql
sudo mv /usr/java/default/jre/lib/management/management.properties /usr/java/default/jre/lib/management/management.properties.TRACKER-OFF
sudo reboot
sudo rm -rf  /var/lib/mysql/.oracle_jre_usage
sudo reboot
```

In this collection, the address column is a JSON object and the column grades is a JSON array. Unlike the JSON table, just specifying the column name with no Jpath result in displaying the JSON representation of them. For instance:

```sql
select name, address from resto limit 3;
```

<table><tbody><tr><th>name</th><th>address</th></tr>
<tr><td>Morris Park Bake Shop</td><td>{"building":"1007","coord":[-73.8561,40.8484], "street":"Morris ParkAve", "zipcode":"10462"}</td></tr>
<tr><td>Wendy'S</td><td>{"building":"469","coord":[-73.9617,40.6629], "street":"Flatbush Avenue", "zipcode":"11225"}</td></tr>
<tr><td>Reynolds Restaurant</td><td>{"building":"351","coord":[-73.9851,40.7677], "street":"West 57Street", "zipcode":"10019"}</td></tr>
</tbody></table>

To address the items inside object or arrays, specify the Jpath in MongoDB syntax3:

```sql
create table newresto (
_id varchar(24) not null,
name varchar(64) not null,
cuisine char(200) not null,
borough char(16) not null,
street varchar(65) field_format='address.street',
building char(16) field_format='address.building',
zipcode char(5) field_format='address.zipcode',
grade char(1) field_format='grades.0.grade',
score int(4) not null field_format='grades.0.score', 
`date` date field_format='grades.0.date',
restaurant_id varchar(255) not null)
engine=connect table_type=MONGO tabname='restaurants'
data_charset=utf8 connection='mongodb://localhost:27017';
```

If this is not done, the Oracle JVM will start the usage tracker, which will create the hidden folder .oracle_jre_usage in the mysql home directory, which will cause a mysql dump of the server to fail.

```sql
select name, street, score, date from newresto limit 5;
```

<table><tbody><tr><th>name</th><th>street</th><th>score</th><th>date</th></tr>
<tr><td>Morris Park Bake Shop</td><td>Morris Park Ave</td><td>2</td><td>03/03/2014</td></tr>
<tr><td>Wendy'S</td><td>Flatbush Avenue</td><td>8</td><td>30/12/2014</td></tr>
<tr><td>Dj Reynolds Pub And Restaurant</td><td>West 57 Street</td><td>2</td><td>06/09/2014</td></tr>
<tr><td>Riviera Caterer</td><td>Stillwell Avenue</td><td>5</td><td>10/06/2014</td></tr>
<tr><td>Tov Kosher Kitchen</td><td>63 Road</td><td>20</td><td>24/11/2014</td></tr>
</tbody></table>

## MONGO Specific Options

The MongoDB syntax for Jpath does not allow the CONNECT specific items on arrays. The same effect can still be obtained by a different way. For this, additional options are used when creating MONGO tables.

<table><tbody><tr><th>Option</th><th>Type</th><th>Description</th></tr>
<tr><td>Colist</td><td>String</td><td>Options to pass to the MongoDB cursor.</td></tr>
<tr><td>Filter</td><td>String</td><td>Query used by the MongoDB cursor.</td></tr>
<tr><td>Pipeline</td><td>Boolean</td><td>If True, Colist is a pipeline.</td></tr>
<tr><td>Fullarray</td><td>Boolean</td><td>Used when creating with Discovery.</td></tr>
</tbody></table>

Note: For the content of these options, refer to the MongoDB documentation.

### Colist Option

Used to pass different options when making the MongoDB cursor used to retrieve the collation documents. One of them is the projection, allowing to limit the items retrieved in documents. It is hardly useful because this limitation is made automatically by CONNECT. However, it can be used when using discovery to eliminate the _id (or another) column when you are not willing to keep it:

```sql
create table restest
engine=connect table_type=MONGO tabname='restaurants'
data_charset=utf8 option_list='level=-1'
colist='{"projection":{"_id":0},"limit":5}';
```

In this example, we added another cursor option, the limit option that works like the limit SQL clause.

### Filter Option

This option is used to specify a “filter” that works as a where clause on the table. Supposing we want to create a table restricted to the restaurant making English cuisine that are not located in the Manhattan borough, we can do it by:

```sql
create table english
engine=connect table_type=MONGO tabname='restaurants'
data_charset=utf8
colist='{"projection":{"cuisine":0}}'
filter='{"cuisine":"English","borough":{"$ne":"Manhattan"}}'
option_list='Level=-1';
```

And if we ask:

```sql
select * from english;
```

This query will return:

<table><tbody><tr><th>_id</th><th>borough</th><th>name</th><th>restaurant_id</th></tr>
<tr><td>58ada47de5a51ddfcd5ee1f3</td><td>Brooklyn</td><td>The Park Slope Chipshop</td><td>40816202</td></tr>
<tr><td>58ada47de5a51ddfcd5ee999</td><td>Brooklyn</td><td>Chip Shop</td><td>41076583</td></tr>
<tr><td>58ada47ee5a51ddfcd5f13d5</td><td>Brooklyn</td><td>The Monro</td><td>41660253</td></tr>
<tr><td>58ada47ee5a51ddfcd5f176e</td><td>Brooklyn</td><td>Dear Bushwick</td><td>41690534</td></tr>
<tr><td>58ada47ee5a51ddfcd5f1e91</td><td>Queens</td><td>Snowdonia Pub</td><td>50000290</td></tr>
</tbody></table>

### Pipeline Option

When this option is specified as true (by YES or 1) the Colist option contains a MongoDB pipeline applying to the table collation. This is a powerful mean for doing things such as expanding arrays like we do with JSON tables. For instance:

```sql
create table resto2 (
name varchar(64) not null,
borough char(16) not null,
date datetime not null,
grade char(1) not null,
score int(4) not null)
engine=connect table_type=MONGO tabname='restaurants' data_charset=utf8
colist='{"pipeline":[{"$match":{"cuisine":"French"}},{"$unwind":"$grades"},{"$project":{"_id":0,"name":1,"borough":1,"date":"$grades.date","grade":"$grades.grade","score":"$grades.score"}}]}'
option_list='Pipeline=1';
```

In this pipeline “$match” is an early filter, “$unwind” means that the grades array will be expanded (one Document for each array values) and “$project” eliminates the _id and cuisine columns and gives the Jpath for the date, grade and score columns.

```sql
select name, grade, score, date from resto2
where borough = 'Bronx';
```

This query replies:

<table><tbody><tr><th>name</th><th>grade</th><th>score</th><th>date</th></tr>
<tr><td>Bistro Sk</td><td>A</td><td>10</td><td>21/11/2014 01:00:00</td></tr>
<tr><td>Bistro Sk</td><td>A</td><td>12</td><td>19/02/2014 01:00:00</td></tr>
<tr><td>Bistro Sk</td><td>B</td><td>18</td><td>12/06/2013 02:00:00</td></tr>
</tbody></table>

This make possible to get things like we do with JSON tables:

```sql
select name, avg(score) average from resto2
group by name having average >= 25;
```

Can be used to get the average score inside the grades array.

<table><tbody><tr><th>name</th><th>average</th></tr>
<tr><td>Bouley Botanical</td><td>25,0000</td></tr>
<tr><td>Cheri</td><td>46,0000</td></tr>
<tr><td>Graine De Paris</td><td>30,0000</td></tr>
<tr><td>Le Pescadeux</td><td>29,7500</td></tr>
</tbody></table>

### Fullarray Option

This option, like the Level option, is only interpreted when creating a table with Discovery (meaning not specifying the columns). It tells CONNECT to generate a column for all existing values in the array. For instance, let us see the MongoDB collection tar by:

```sql
create table seetar (
Collection varchar(300) not null field_format='*')
engine=CONNECT table_type=MONGO tabname=tar;
```

The format ‘*’ indicates we want to see the Json documents. This small collection is:

<table><tbody><tr><th>Collection</th></tr>
<tr><td>{"_id":{"$oid":"58f63a5099b37d9c930f9f3b"},"item":"journal","prices":[87.0,45.0,63.0,12.0,78.0]}</td></tr>
<tr><td>{"_id":{"$oid":"58f63a5099b37d9c930f9f3c"},"item":"notebook","prices":[123.0,456.0,789.0]}</td></tr>
</tbody></table>

The Fullarray option can be used here to generate enough columns to see all the prices of the document prices array.

```sql
create table tar
engine=connect table_type=MONGO
colist='{"projection":{"_id":0}}'
option_list='level=1,Fullarray=YES';
```

The table has been created as:

```sql
CREATE TABLE `tar` (
  `item` char(8) NOT NULL,
  `prices_0` double(12,6) NOT NULL `FIELD_FORMAT`='prices.0',
  `prices_1` double(12,6) NOT NULL `FIELD_FORMAT`='prices.1',
  `prices_2` double(12,6) NOT NULL `FIELD_FORMAT`='prices.2',
  `prices_3` double(12,6) DEFAULT NULL `FIELD_FORMAT`='prices.3',
  `prices_4` double(12,6) DEFAULT NULL `FIELD_FORMAT`='prices.4'
) ENGINE=CONNECT DEFAULT CHARSET=latin1 `TABLE_TYPE`='MONGO' `COLIST`='{"projection":{"_id":0}}' `OPTION_LIST`='level=1,Fullarray=YES';
```

And is displayed as:

<table><tbody><tr><th>item</th><th>prices_0</th><th>prices_1</th><th>prices_2</th><th>prices_3</th><th>prices_4</th></tr>
<tr><td>journal</td><td>87.00</td><td>45.00</td><td>63.00</td><td>12.00</td><td>78.00</td></tr>
<tr><td>notebook</td><td>123.00</td><td>456.00</td><td>789.00</td><td>NULL</td><td>NULL</td></tr>
</tbody></table>

## Create, Read, Update and Delete Operations

All modifying operations are supported. However, inserting into arrays must be done in a specific way. Like with the Fullarray option, we must have enough columns to specify the array values. For instance, we can create a new table by:

```sql
create table testin (
n int not null,
m char(12) not null,
surname char(16) not null field_format='person.name.first',
name char(16) not null field_format='person.name.last',
age int(3) not null field_format='person.age',
price_1 double(8,2) field_format='d.0',
price_2 double(8,2) field_format='d.1',
price_3 double(8,2) field_format='d.2')
engine=connect table_type=MONGO tabname='tin'
connection='mongodb://localhost:27017';
```

Now it is possible to populate it by:

```sql
insert into testin values
(1789, 'Welcome', 'Olivier','Bertrand',56, 3.14, 2.36, 8.45),
(1515, 'Hello', 'John','Smith',32, 65.17, 98.12, NULL),
(2014, 'Coucou', 'Foo','Bar',20, -1.0, 74, 81356);
```

The result will be:

<table><tbody><tr><th>n</th><th>m</th><th>surname</th><th>name</th><th>age</th><th>price_1</th><th>price_2</th><th>price_3</th></tr>
<tr><td>1789</td><td>Welcome</td><td>Olivier</td><td>Bertrand</td><td>56</td><td>3,14</td><td>2,36</td><td>8,45</td></tr>
<tr><td>1515</td><td>Hello</td><td>John</td><td>Smith</td><td>32</td><td>65,17</td><td>98,12</td><td>NULL</td></tr>
<tr><td>2014</td><td>Coucou</td><td>Foo</td><td>Bar</td><td>20</td><td>-1</td><td>74</td><td>81356</td></tr>
</tbody></table>

Note: If the collection does not exist yet when creating the table and inserting in it, MongoDB creates it automatically.

It can be updated by queries such as:

```sql
update tintin set price_3 = 83.36 where n = 2014;
```

To look how the array is generated, let us create another table:

```sql
create table tintin (
n int not null,
name char(16) not null field_format='person.name.first',
prices varchar(255) field_format='d')
engine=connect table_type=MONGO tabname='in';
```

This table is displayed as:

<table><tbody><tr><th>n</th><th>name</th><th>prices</th></tr>
<tr><td>1789</td><td>Olivier</td><td>[3.14, 2.36, 8.45]</td></tr>
<tr><td>1515</td><td>John</td><td>[65.17, 98.12]</td></tr>
<tr><td>2014</td><td>Foo</td><td>[&lt;null&gt;, 74.0, 83.36]</td></tr>
</tbody></table>

Note: This last table can be used to make array calculations like with JSON tables using the JSON UDF functions. For instance:

```sql
select name, jsonget_real(prices,'[+]') sum_prices, jsonget_real(prices,'[!]') avg_prices from tintin;
```

This query returns:

<table><tbody><tr><th>name</th><th>sum_prices</th><th>avg_prices</th></tr>
<tr><td>Olivier</td><td>13.95</td><td>4.65</td></tr>
<tr><td>John</td><td>163.29</td><td>81.64</td></tr>
<tr><td>Foo</td><td>157,36</td><td>78.68</td></tr>
</tbody></table>

Note: When calculating on arrays, null values are ignored.

## Status of MONGO Table Type

This table type is still under development. It has significant advantages over the JSON type to access MongoDB collections. Firstly, the access being direct, tables are always up to date whether the collection has been modified by another application. Performance wise, it is much faster than JSON, because most processing is done by MongoDB on BSON, its internal representation of JSON data, which is designed to optimize all operations. Note that using the MongoDB C Driver is about twice as fast as using the MongoDB Java Driver.

## Current Restrictions

- Option “CATFUNC=tables” is not implemented yet.
- Options SRCDEF and EXECSRC do not apply to MONGO tables.