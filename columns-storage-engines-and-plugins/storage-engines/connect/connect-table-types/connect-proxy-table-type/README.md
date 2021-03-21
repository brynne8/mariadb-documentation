# CONNECT PROXY Table Type

A `PROXY` table is a table that accesses and reads the data of another table or view.
For instance, to create a table based on the boys `FIX` table:

```sql
create table xboy engine=connect 
  table_type=PROXY tabname=boys;
```

Simply, `PROXY` being the default type when `TABNAME` is specified:

```sql
create table xboy engine=connect tabname=boys;
```

Because the boys table can be directly used, what can be the use of a proxy
table? Well, its main use is to be internally used by other table types such as
[TBL](/kb/en/connect-table-types-tbl-table-type-table-list/), [XCOL](/kb/en/connect-table-types-xcol-table-type/), [OCCUR](/kb/en/connect-table-types-occur-table-type/), or [PIVOT](/kb/en/connect-table-types-pivot-table-type/). Sure enough, PROXY tables are CONNECT tables, meaning that
they can be based on tables of any engines and accessed by table types that
need to access CONNECT tables.

## Proxy on non-CONNECT Tables

When the sub-table is a view or not a CONNECT table, CONNECT internally creates a
temporary CONNECT table of [MYSQL](/kb/en/connect-table-types-mysql-table-type-accessing-mysqlmariadb-tables/) type to access it. This connection uses
the same default parameters as for a `MYSQL` table. It is also possible to
specify them to the `PROXY` table using in the `PROXY` declaration the same
`OPTION_LIST` options as for a `MYSQL` table. Of course, it is simpler and 
more natural to use directly the MYSQL type in this case.

Normally, the default parameters should enable the `PROXY` table to reconnect
the server. However, an issue is when the current user was logged using a
password. The security protocol prevents CONNECT to retrieve this password and
requires it to be given in the `PROXY` table create statement. For instance
adding to it:

```sql
... option_list='Password=mypass';
```

However, it is often not advisable to write in clear a password that can be
seen by all user able to see the table declaration by show create table, in
particular, if the table is used when the current user is root. To avoid this,
a specific user should be created on the local host that will be used by proxy
tables to retrieve local tables. This user can have minimum grant options, for
instance SELECT on desired directories, and needs no password. Supposing
‘proxy’ is such a user, the option list to add will be:

```sql
... option_list='user=proxy';
```

## Using a PROXY Table as a View

A `PROXY` table can also be used by itself to modify the way a table is
viewed. For instance, a proxy table does not use the indexes of the object
table. It is also possible to define its columns with different names or type,
to use only some of them or to changes their order. For instance:

```sql
create table city (
  city varchar(11),
  boy char(12) flag=1,
  birth date)
engine=CONNECT tabname=boys;
select * from city;
```

This will display:

<table><tbody><tr><th>city</th><th>boy</th><th>birth</th></tr>
<tr><td>Boston</td><td>John</td><td>1986-01-25</td></tr>
<tr><td>Boston</td><td>Henry</td><td>1987-06-07</td></tr>
<tr><td>San Jose</td><td>George</td><td>1981-08-10</td></tr>
<tr><td>Chicago</td><td>Sam</td><td>1979-11-22</td></tr>
<tr><td>Dallas</td><td>James</td><td>1992-05-13</td></tr>
<tr><td>Boston</td><td>Bill</td><td>1986-09-11</td></tr>
</tbody></table>

Here we did not have to specify column format or offset because data are
retrieved from the boys table, not directly from the boys.txt file. The flag
option of the <em>boy</em> column indicates that it correspond to the first column
of the boys table, the <em>name</em> column.

## Avoiding PROXY table loop

CONNECT is able to test whether a `PROXY`, or `PROXY`-based, table refers
directly or indirectly to itself. If a direct reference can tested at table
creation, an indirect reference can only be tested when executing a query on
the table. However, this is possible only for local tables. When using remote
tables or views, a problem can occur if the remote table or the view refers 
back to one of the local tables of the chain. The same caution should be used
than when using [FEDERATEDX](/columns-storage-engines-and-plugins/storage-engines/federatedx-storage-engine) tables.

<strong>Note:</strong> All `PROXY` or `PROXY`-based tables are read-only in this
version.

## Modifying Operations

All [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert) / [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update) / [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete) operations can be used with proxy tables. However, the same restrictions applying to the source table also apply to the proxy table.

Note: All PROXY and PROXY-based table types are not indexable.