# CONNECT MYSQL Table Type: Accessing MySQL/MariaDB Tables

This table type uses libmysql API to access a MySQL or MariaDB table or view. This
table must be created on the current server or on another local or remote
server. This is similar to what the [FederatedX](/kb/en/federatedx/) storage engine provides with some
differences.

Currently the Federated-like syntax can be used to create such a table, for instance:

```sql
create table essai (
  num integer(4) not null,
  line char(15) not null)
engine=CONNECT table_type=MYSQL
connection='mysql://root@localhost/test/people';
```

The connection string can have the same syntax as that used by FEDERATED

```sql
scheme://username:password@hostname:port/database/tablename
scheme://username@hostname/database/tablename
scheme://username:password@hostname/database/tablename
scheme://username:password@hostname/database/tablename
```

However, it can also be mixed with connect standard options. For instance:

```sql
create table essai (
  num integer(4) not null,
  line char(15) not null)
engine=CONNECT table_type=MYSQL dbname=test tabname=people
connection='mysql://root@localhost';
```

It can also be specified as a reference to a federated server:

```sql
connection="connection_one"
connection="connection_one/table_foo"
```

The pure (deprecated) CONNECT syntax is also accepted:

```sql
create table essai (
  num integer(4) not null,
  line char(15) not null)
engine=CONNECT table_type=MYSQL dbname=test tabname=people
option_list='user=root,host=localhost';
```

The specific connection items are:

<table><tbody><tr><th>Option</th><th>Default value</th><th>Description</th></tr>
<tr><td>Table</td><td>The table name</td><td>The name of the table to access.</td></tr>
<tr><td>Database</td><td>The current DB name</td><td>The database where the table is located.</td></tr>
<tr><td>Host</td><td>localhost*</td><td>The host of the server, a name or an IP address.</td></tr>
<tr><td>User</td><td>The current user</td><td>The connection user name.</td></tr>
<tr><td>Password</td><td>No password</td><td>An optional user password.</td></tr>
<tr><td>Port</td><td>The currently used port</td><td>The port of the server.</td></tr>
<tr><td>Quoted</td><td>0</td><td>1 if remote Tabname must be quoted.</td></tr>
</tbody></table>

- - When the host is specified as “localhost”, the connection is established on Linux using Linux sockets. On Windows, the connection is established by default using shared memory if it is enabled. If not, the TCP protocol is used. An alternative is to specify the host as “.” to use a named pipe connection (if it is enabled). This makes possible to use these table types with server skipping networking.

<strong>Caution:</strong> Take care not to refer to the MYSQL table itself to avoid an infinite loop!

MYSQL table can refer to the current server as well as to another server. Views
can be referred by name or directly giving a source definition, for instance:

```sql
create table grp engine=connect table_type=mysql
CONNECTION='mysql://root@localhost/test/people'
SRCDEF='select title, count(*) as cnt from employees group by title';
```

When specified, the columns of the mysql table must exist in the accessed table with the same name, but can be only a subset of them and specified in a different order. Their type must be a type supported by CONNECT and, if it is not identical to the type of the accessed table matching column, a conversion can be done according to the rules given in [Data type conversion](/kb/en/connect-data-types/#data-type-conversion).

Note: For columns prone to be targeted by a where clause, keep the column type compatible with the source table column type (numeric or character) to have a correct rephrasing of the where&nbsp;clause.

If you do not want to restrict or change the column definition, do not provide it and leave CONNECT get the column definition from the remote server. For instance:

```sql
create table essai engine=CONNECT table_type=MYSQL
connection='mysql://root@localhost/test/people';
```

This will create the <em>essai</em> table with the same columns than the people table. If the target table contains CONNECT incompatible type columns, see [Data type conversion](/kb/en/connect-data-types/#data-type-conversion) to know how these columns can be converted or skipped.

## Charset Specification

When accessing the remote table, CONNECT sets the connection charset set to the default local table charset as the FEDERATED engine does.

Do not specify a column character set if it is different from the table default character set even when it is the case on the remote table. This is because the remote column is translated to the local table character set when reading it. This is the default but it can be modified by the setting the [character_set_results](/kb/en/server-system-variables/#character_set_results) variable of the target server. If it must keep its setting, for instance to UTF8 when containing Unicode characters, specify the local default charset to its character set.

This means that it is not possible to correctly retrieve a remote table if it contains columns having different character sets. A solution is to retrieve it by several local tables, each accessing only columns with the same character set.

## Indexing of MYSQL tables

Indexes are rarely useful with MYSQL tables. This is because CONNECT tries to access only the
requested rows. For instance if you ask:

```sql
select * from essai where num = 23;
```

##### MariaDB until [10.1.1](/kb/en/mariadb-1011-release-notes/)

Until 10.1.1, MariaDB required the [optimizer_switch](/kb/en/server-system-variables/#optimizer_switch) variable to be set globally as follows:
<code class="fixed" style="white-space:pre-wrap">optimizer_switch=engine_condition_pushdown=on</code>. Otherwise CONNECT cannot get the WHERE clause of the query.

CONNECT will construct and send to the server the query:

```sql
SELECT num, line FROM people WHERE num = 23
```

If the <em>people</em> table is indexed on <em>num</em>, indexing will be used on the remote server. This, in all cases,
will limit the amount of data to retrieve on the network.

However, an index can be specified for columns that are prone to be used to join another table to the
MYSQL table. For instance:

```sql
select d.id, d.name, f.dept, f.salary
from loc_tab d straight_join cnc_tab f on d.id = f.id
where f.salary > 10000;
```

If the <em>id</em> column of the remote table addressed by the <em>cnc_tab</em> MYSQL table is indexed (which is likely
if it is a key) you should also index the <em>id</em> column of the MYSQL <em>cnc_tab</em> table. If so, using “remote”
indexing as does FEDERATED, only the useful rows of the remote table will be retrieved during the
join process. However, because these rows are retrieved by separate [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) statements, this will be
useful only when retrieving a few rows of a big table.

In particular, you should not specify an index for columns not used for joining and above all DO NOT
index a joined column if it is not indexed in the remote table. This would cause multiple scans of the
remote table to retrieve the joined rows one by one.

## Data Modifying Operations

The CONNECT MYSQL type supports [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) and [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/) and a somewhat limited form
of [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) and [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/). These are described below.

The MYSQL type uses similar methods than the ODBC type to implement the [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert/),
[UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) and [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) commands. Refer to the ODBC chapter for the restrictions
concerning them.

For the [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) and [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete/) commands, there are fewer restrictions because the
remote server being a MySQL server, the syntax of the command will be always
acceptable by the remote server.

For instance, you can freely use keywords like IGNORE or LOW_PRIORITY as well
as scalar functions in the SET and WHERE clauses.

However, there is still an issue on multi-table statements. Let us suppose you
have a <em>t1</em> table on the remote server and want to execute a query such as:

```sql
update essai as x set line = (select msg from t1 where id = x.num)
where num = 2;
```

When parsed locally, you will have errors if no <em>t1</em> table exists or if it
does not have the referenced columns. When <em>t1</em> does not exist, you can
overcome this issue by creating a local dummy <em>t1</em> table:

```sql
create table t1 (id int, msg char(1)) engine=BLACKHOLE;
```

This will make the local parser happy and permit to execute the command on the
remote server. Note however that having a local MySQL table defined on the
remote <em>t1</em> table does not solve the problem unless it is also names <em>t1</em>
locally.

This is why, to permit to have all types of commands executed by the data
source without any restriction, CONNECT provides a specific MySQL table subtype
described now.

## Sending commands to a MariaDB Server

This can be done like for ODBC or JDBC tables by defining a specific table that will be used to send commands and get the result of their execution..

```sql
create table send (
  command varchar(128) not null,
  warnings int(4) not null flag=3,
  number int(5) not null flag=1,
  message varchar(255) flag=2)
engine=connect table_type=mysql
connection='mysql://user@host/database'
option_list='Execsrc=1,Maxerr=2';
```

The key points in this create statement are the EXECSRC option and the column definition.

The EXECSRC option tells that this table will be used to send commands to the
MariaDB server. Most of the sent commands do not return result set. Therefore,
the table columns are used to specify the command to be executed and to get the
result of the execution. The name of these columns can be chosen arbitrarily,
their function coming from the FLAG value:

<table><tbody><tr><td><strong>Flag=0:</strong></td><td>The command to execute (the default)</td></tr>
<tr><td><strong>Flag=1:</strong></td><td>The number of affected rows, or the result number of columns if the command would return a result set.</td></tr>
<tr><td><strong>Flag=2:</strong></td><td>The returned (eventually error) message.</td></tr>
<tr><td><strong>Flag=3:</strong></td><td>The number of warnings.</td></tr>
</tbody></table>

How to use this table and specify the command to send? By executing a command such as:

```sql
select * from send where command = 'a command';
```

This will send the command specified in the WHERE clause to the data source and
return the result of its execution. The syntax of the WHERE clause must be
exactly as shown above. For instance:

```sql
select * from send where command =
'CREATE TABLE people (
num integer(4) primary key autoincrement,
line char(15) not null';
```

This command returns:

<table><tbody><tr><th>command</th><th>warnings</th><th>number</th><th>message</th></tr>
<tr><td><code>CREATE TABLE people (num integer(4) primary key aut...</code></td><td>0</td><td>0</td><td>Affected rows</td></tr>
</tbody></table>

### Sending several commands in one call

It can be faster to execute because there will be only one connection for all
of them. To send several commands in one call, use the following syntax:

```sql
select * from send where command in (
"update people set line = 'Two' where id = 2",
"update people set line = 'Three' where id = 3");
```

When several commands are sent, the execution stops at the end of them or after
a command that is in error. To continue after n errors, set the option
maxerr=<em>n</em> (0 by default) in the option list.

<strong>Note 1:</strong> It is possible to specify the SRCDEF option when creating an
EXECSRC table. It will be the command sent by default when a WHERE clause is
not specified.

<strong>Note 2:</strong> Backslashes inside commands must be escaped. Simple quotes must be
escaped if the command is specified between simple quotes, and double quotes if
it is specified between double quotes.

<strong>Note 3:</strong> Sent commands apply in the specified database. However, they can
address any table within this database.

<strong>Note 4:</strong> Currently, all commands are executed in mode AUTOCOMMIT.

### Retrieving Warnings and Notes

If a sent command causes warnings to be issued, it is useless to resend a “show
warnings” command because the MariaDB server is opened and closed when sending
commands. Therefore, getting warnings requires a specific (and tricky) way.

To indicate that warning text must be added to the returned result, you must
send a multi-command query containing “pseudo” commands that are not sent to
the server but directly interpreted by the EXECSRC table. These “pseudo”
commands are:

<table><tbody><tr><td><strong>Warning</strong></td><td>To get warnings</td></tr>
<tr><td><strong>Note</strong></td><td>To get notes</td></tr>
<tr><td><strong>Error</strong></td><td>To get errors returned as warnings (?)</td></tr>
</tbody></table>

Note that they must be spelled (case insensitive) exactly as above, no final “s”. For instance:

```sql
select * from send where command in ('Warning','Note',
'drop table if exists try',
'create table try (id int key auto_increment, msg varchar(32) not
null) engine=aria',
"insert into try(msg) values('One'),(NULL),('Three') ",
"insert into try values(2,'Deux') on duplicate key update msg =
'Two'",
"insert into try(message) values('Four'),('Five'),('Six')",
'insert into try(id) values(NULL)',
"update try set msg = 'Four' where id = 4",
'select * from try');
```

This can return something like this:

<table><tbody><tr><th>command</th><th>warnings</th><th>number</th><th>message</th></tr>
<tr><td><code>drop table if exists try</code></td><td>1</td><td>0</td><td>Affected rows</td></tr>
<tr><td>Note</td><td>0</td><td>1051</td><td>Unknown table 'try'</td></tr>
<tr><td><code>create table try (id int key auto_increment, msg...</code></td><td>0</td><td>0</td><td>Affected rows</td></tr>
<tr><td><code>insert into try(msg) values('One'),(NULL),('Three')</code></td><td>1</td><td>3</td><td>Affected rows</td></tr>
<tr><td>Warning</td><td>0</td><td>1048</td><td>Column 'msg' cannot be null</td></tr>
<tr><td><code>insert into try values(2,'Deux') on duplicate key...</code></td><td>0</td><td>2</td><td>Affected rows</td></tr>
<tr><td><code>insert into try(msge) values('Four'),('Five'),('Six')</code></td><td>0</td><td>1054</td><td>Unknown column 'msge' in 'field list'</td></tr>
<tr><td><code>insert into try(id) values(NULL)</code></td><td>1</td><td>1</td><td>Affected rows</td></tr>
<tr><td>Warning</td><td>0</td><td>1364</td><td>Field 'msg' doesn't have a default value</td></tr>
<tr><td><code>update try set msg = 'Four' where id = 4</code></td><td>0</td><td>1</td><td>Affected rows</td></tr>
<tr><td><code>select * from try</code></td><td>0</td><td>2</td><td>Result set columns</td></tr>
</tbody></table>

The execution continued after the command in error because of the MAXERR
option. Normally this would have stopped the execution.

Of course, the last “select” command is useless here because it cannot return
the table contain. Another MYSQL table without the EXECSRC option and with
proper column definition should be used instead.

## Connection Engine Limitations

### Data types

There is a maximum key.index length of 255 bytes. You may be able to declare the table without an index and rely on the engine condition pushdown and remote schema.

The following types can't be used:

- [BIT](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/bit/)
- [BINARY](/columns-storage-engines-and-plugins/data-types/string-data-types/binary/)
- [TINYBLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/tinyblob/), [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/), [MEDIUMBLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/mediumblob/), [LONGBLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/longblob/)
- [TINYTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/tinytext/), [MEDIUMTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/mediumtext/), [LONGTEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/longtext/)
- [ENUM](/columns-storage-engines-and-plugins/data-types/string-data-types/enum/)
- [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set/)
- [Geometry types](/sql-statements-structure/geographic-geometric-features/geometry-types/)

Note: [TEXT](/columns-storage-engines-and-plugins/data-types/string-data-types/text/) is allowed. However, the handling depends on the values given to the [connect_type_conv](/kb/en/connect-system-variables/#connect_type_conv) and [connect_conv_size](/kb/en/connect-system-variables/#connect_conv_size) system variables, and by default no conversion of TEXT columns is permitted.

### SQL Limitations

The following SQL queries are not supported

- [REPLACE INTO](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/replace/)
- [INSERT ... ON  DUPLICATE KEY UPDATE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-on-duplicate-key-update/)

## CONNECT MYSQL versus FEDERATED

The CONNECT MYSQL table type should not be regarded as a replacement for the
[FEDERATED(X)](/columns-storage-engines-and-plugins/storage-engines/federatedx-storage-engine/) engine. The main use of the MYSQL type is to access other engine tables as if they were CONNECT tables. This was necessary when accessing tables
from some CONNECT table types such as [TBL](/kb/en/connect-table-types-tbl-table-type-table-list/), [XCOL](/kb/en/connect-table-types-xcol-table-type/), [OCCUR](/kb/en/connect-table-types-occur-table-type/), or [PIVOT](/kb/en/connect-table-types-pivot-table-type/) that are
designed to access CONNECT tables only. When their target table is not a
CONNECT table, these types are silently using internally an intermediate MYSQL
table.

However, there are cases where you can use MYSQL CONNECT tables yourself, for instance:

1 When the table will be used by a [TBL](/kb/en/connect-table-types-tbl-table-type-table-list/) table. This enables you to specify the connection parameters for each sub-table and is more efficient than using a
  local FEDERATED sub-table.
2 When the desired returned data is directly specified by the SRCDEF option.
  This is great to let the remote server do most of the job, such as grouping
  and/or joining tables. This cannot be done with the FEDERATED engine.
3 To take advantage of the <em>push_cond</em> facility that adds a where clause to the command sent to the remote table. This restricts the size of the result set and can be crucial for big tables.
4 For tables with the EXECSRC option on.
5 When doing tests. For instance to check a connection string.

If you need multi-table updating, deleting, or bulk inserting on a remote
table, you can alternatively use the FEDERATED engine or a “send” table
specifying the EXECSRC option on.

## See also

- [Using the TBL and MYSQL types together](/kb/en/connect-table-types-using-the-tbl-and-mysql-types-together/)