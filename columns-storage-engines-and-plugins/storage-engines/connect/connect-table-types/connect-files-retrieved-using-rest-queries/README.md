# CONNECT - Files Retrieved Using Rest Queries

Starting with [CONNECT version 1.07.0001](/columns-storage-engines-and-plugins/storage-engines/connect/), JSON, XML and possibly CSV data files can be retrieved as results from REST queries when creating or querying such tables. This is done internally by CONNECT using the CURL program generally available on all systems (if not just install it).

This can also be done using the Microsoft Casablanca (cpprestsdk) package. To enable it, first, install the package as explained in [https://github.com/microsoft/cpprestsdk](https://github.com/microsoft/cpprestsdk). Then make the GetRest library (dll or so) as explained in [Making the GetRest Library](/columns-storage-engines-and-plugins/storage-engines/connect/connect-making-the-getrest-library/).

Note: If both are available, cpprestsdk is used preferably because it is faster. This can be changed by specifying ‘curl=1’ in the option list.

Note: If you want to use this feature with an older distributed version of MariaDB not featuring REST, it is possible to add it as an OEM module as explained in [Adding the REST Feature as a Library Called by an OEM Table](/columns-storage-engines-and-plugins/storage-engines/connect/connect-making-the-getrest-library/).

### Creating Tables using REST

To do so, specify the HTTP of the web client and eventually the URI of the request in the [CREATE TABLE](/sql-statements-structure/sql-statements/data-definition/create/create-table/) statement. For example, for a query returning JSON data:

```sql
CREATE TABLE webusers (
  id bigint(2) NOT NULL,
  name char(24) NOT NULL,
  username char(16) NOT NULL,
  email char(25) NOT NULL,
  address varchar(256) DEFAULT NULL,
  phone char(21) NOT NULL,
  website char(13) NOT NULL,
  company varchar(256) DEFAULT NULL
) ENGINE=CONNECT DEFAULT CHARSET=utf8
TABLE_TYPE=JSON FILE_NAME='users.json' HTTP='http://jsonplaceholder.typicode.com' URI='/users';
```

As with standard JSON tables, discovery is possible, meaning that you can leave CONNECT to define the columns by analyzing the JSON file. Here you could just do:

```sql
CREATE TABLE webusers
ENGINE=CONNECT DEFAULT CHARSET=utf8
TABLE_TYPE=JSON FILE_NAME='users.json'
HTTP='http://jsonplaceholder.typicode.com' URI='/users';
```

For example, executing:

```sql
SELECT name, address FROM webusers2 LIMIT 1;
```

returns:

<table><tbody><tr><th>name</th><th>address</th></tr>
<tr><td>Leanne Graham</td><td>Kulas Light Apt. 556 Gwenborough 92998-3874 -37.3159 81.1496</td></tr>
</tbody></table>

Here we see that for some complex elements such as <em>address</em>, which is a Json object containing values and objects, CONNECT by default has just listed their texts separated by blanks. But it is possible to ask it to analyze in more depth the json result by adding the DEPTH option. For instance:

```sql
CREATE OR REPLACE TABLE webusers
ENGINE=CONNECT DEFAULT CHARSET=utf8
TABLE_TYPE=JSON FILE_NAME='users.json'
HTTP='http://jsonplaceholder.typicode.com' URI='/users'
OPTION_LIST='Depth=2';
```

Then the table will be created as:

```sql
CREATE TABLE `webusers3` (
  `id` bigint(2) NOT NULL,
  `name` char(24) NOT NULL,
  `username` char(16) NOT NULL,
  `email` char(25) NOT NULL,
  `address_street` char(17) NOT NULL `JPATH`='$.address.street',
  `address_suite` char(9) NOT NULL `JPATH`='$.address.suite',
  `address_city` char(14) NOT NULL `JPATH`='$.address.city',
  `address_zipcode` char(10) NOT NULL `JPATH`='$.address.zipcode',
  `address_geo_lat` char(8) NOT NULL `JPATH`='$.address.geo.lat',
  `address_geo_lng` char(9) NOT NULL `JPATH`='$.address.geo.lng',
  `phone` char(21) NOT NULL,
  `website` char(13) NOT NULL,
  `company_name` char(18) NOT NULL `JPATH`='$.company.name',
  `company_catchPhrase` char(40) NOT NULL `JPATH`='$.company.catchPhrase',
  `company_bs` varchar(36) NOT NULL `JPATH`='$.company.bs'
) ENGINE=CONNECT DEFAULT CHARSET=utf8 `TABLE_TYPE`='JSON' `FILE_NAME`='users.json' `OPTION_LIST`='Depth=2' `HTTP`='http://jsonplaceholder.typicode.com' `URI`='/users';
```

Allowing one to get all the values of the Json result, for example:

```sql
SELECT name, address_city city, company_name company FROM webusers3;
```

That results in:

<table><tbody><tr><th>name</th><th>city</th><th>company</th></tr>
<tr><td>Leanne Graham</td><td>Gwenborough</td><td>Romaguera-Crona</td></tr>
<tr><td>Ervin Howell</td><td>Wisokyburgh</td><td>Deckow-Crist</td></tr>
<tr><td>Clementine Bauch McKenziehaven</td><td>Romaguera-Jacobson</td></tr>
<tr><td>Patricia Lebsack</td><td>South Elvis</td><td>Robel-Corkery</td></tr>
<tr><td>Chelsey Dietrich</td><td>Roscoeview</td><td>Keebler LLC</td></tr>
<tr><td>Mrs. Dennis Schulist</td><td>South Christy</td><td>Considine-Lockman</td></tr>
<tr><td>Kurtis Weissnat</td><td>Howemouth</td><td>Johns Group</td></tr>
<tr><td>Nicholas Runolfsdottir V</td><td>Aliyaview</td><td>Abernathy Group</td></tr>
<tr><td>Glenna Reichert</td><td>Bartholomebury</td><td>Yost and Sons</td></tr>
<tr><td>Clementina DuBuque</td><td>Lebsackbury</td><td>Hoeger LLC</td></tr>
</tbody></table>

Of course, the complete create table (obtained by SHOW CREATE TABLE) can later be edited to make your table return exactly what you want to get. See the [JSON table type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-json-table-type/) for details about what and how to specify these.

Note that such tables are read only. In addition, the data will be retrieved from the web each time you query the table with a [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statement. This is fine if the result varies each time, such as when you query a weather forecasting site. But if you want to use the retrieved file many times without reloading it, just create another table on the same file without specifying the HTTP option.

Note: For JSON tables, specifying the file name is optional and defaults to tabname.type. However, you should specify it if you want to use the file later for other tables.

See the [JSON table type](/columns-storage-engines-and-plugins/storage-engines/connect/connect-table-types/connect-json-table-type/) for changes that will occur in the new CONNECT versions (distributed in early 2021).