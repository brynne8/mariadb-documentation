# CONNECT TBL Table Type: Table List

This type allows defining a table as a list of tables of any engine and type.
This is more flexible than multiple tables that must be all of the same file
type. This type does, but is more powerful than, what is done with the [MERGE](/columns-storage-engines-and-plugins/storage-engines/merge)
engine.

The list of the columns of the TBL table may not necessarily include all the
columns of the tables of the list. If the name of some columns is different in
the sub-tables, the column to use can be specified by its position given by the
`FLAG` option of the column. If the `ACCEPT` option is set to true (Y or 1)
columns that do not exist in some of the sub-tables are accepted and their
value will be null or
pseudo-null (this depends on the nullability of the column) for
the tables not having this column. The column types can also be different and
an automatic conversion will be done if necessary.

<strong>Note:</strong> If not specified, the column definitions are retrieved from the first
table of the table list.

The default database of the sub-tables is the current database or if not, can
be specified in the DBNAME option. For the tables that are not in the default
database, this can be specified in the table list. For instance, to create a
table based on the French table <em>employe</em> in the current database and on the
English table <em>employee</em> of the <em>db2</em> database, the syntax of the create
statement can be:

```sql
CREATE TABLE allemp (
  SERIALNO char(5) NOT NULL flag=1,
  NAME varchar(12) NOT NULL flag=2,
  SEX smallint(1),
  TITLE varchar(15) NOT NULL flag=3,
  MANAGER char(5) DEFAULT NULL flag=4,
  DEPARTMENT char(4) NOT NULL flag=5,
  SECRETARY char(5) DEFAULT NULL flag=6,
  SALARY double(8,2) NOT NULL flag=7)
ENGINE=CONNECT table_type=TBL
table_list='employe,db2.employee' option_list='Accept=1';
```

The search for columns in sub tables is done by name and, if they exist with a
different name, by their position given by a not null `FLAG` option.
Column <em>sex</em> exists only in the English table (`FLAG` is `0`). Its values
will null value for the French table.

For instance, the query:

```sql
select name, sex, title, salary from allemp where department = 318;
```

Can reply:

<table><tbody><tr><th>NAME</th><th>SEX</th><th>TITLE</th><th>SALARY</th></tr>
<tr><td>BARBOUD</td><td>NULL</td><td>VENDEUR</td><td>9700.00</td></tr>
<tr><td>MARCHANT</td><td>NULL</td><td>VENDEUR</td><td>8800.00</td></tr>
<tr><td>MINIARD</td><td>NULL</td><td>ADMINISTRATIF</td><td>7500.00</td></tr>
<tr><td>POUPIN</td><td>NULL</td><td>INGENIEUR</td><td>7450.00</td></tr>
<tr><td>ANTERPE</td><td>NULL</td><td>INGENIEUR</td><td>6850.00</td></tr>
<tr><td>LOULOUTE</td><td>NULL</td><td>SECRETAIRE</td><td>4900.00</td></tr>
<tr><td>TARTINE</td><td>NULL</td><td>OPERATRICE</td><td>2800.00</td></tr>
<tr><td>WERTHER</td><td>NULL</td><td>DIRECTEUR</td><td>14500.00</td></tr>
<tr><td>VOITURIN</td><td>NULL</td><td>VENDEUR</td><td>10130.00</td></tr>
<tr><td>BANCROFT</td><td>2</td><td>SALESMAN</td><td>9600.00</td></tr>
<tr><td>MERCHANT</td><td>1</td><td>SALESMAN</td><td>8700.00</td></tr>
<tr><td>SHRINKY</td><td>2</td><td>ADMINISTRATOR</td><td>7500.00</td></tr>
<tr><td>WALTER</td><td>1</td><td>ENGINEER</td><td>7400.00</td></tr>
<tr><td>TONGHO</td><td>1</td><td>ENGINEER</td><td>6800.00</td></tr>
<tr><td>HONEY</td><td>2</td><td>SECRETARY</td><td>4900.00</td></tr>
<tr><td>PLUMHEAD</td><td>2</td><td>TYPIST</td><td>2800.00</td></tr>
<tr><td>WERTHER</td><td>1</td><td>DIRECTOR</td><td>14500.00</td></tr>
<tr><td>WHEELFOR</td><td>1</td><td>SALESMAN</td><td>10030.00</td></tr>
</tbody></table>

The first 9 rows, coming from the French table, have a null for the <em>sex</em>
value. They would have 0 if the sex column had been created NOT NULL.

### Sub-tables of not CONNECT engines

Sub-tables are accessed as <a undefined>PROXY</a>
tables. For not CONNECT sub-tables that are accessed via the MySQL API, it is
possible like with `PROXY` to change the MYSQL default options. Of course,
this will apply to all not CONNECT tables of the list.

### Using the TABID special column

The TABID special column can be used to see from which table the rows come from
and to restrict the access to only some of the sub-tables.

Let us see the following example where t1 and t2 are MyISAM tables similar to
the ones given in the `MERGE` description:

```sql
create table xt1 (
  a int(11) not null,
  message char(20))
engine=CONNECT table_type=MYSQL tabname='t1'
option_list='database=test,user=root';

create table xt2 (
  a int(11) not null,
  message char(20))
engine=CONNECT table_type=MYSQL tabname='t2'
option_list='database=test,user=root';

create table toto (
  tabname char(8) not null special='TABID',
  a int(11) not null,
  message char(20))
engine=CONNECT table_type=TBL table_list='xt1,xt2';

select * from total;
```

The result returned by the SELECT statement is:

<table><tbody><tr><th>tabname</th><th>a</th><th>message</th></tr>
<tr><td>xt1</td><td>1</td><td>Testing</td></tr>
<tr><td>xt1</td><td>2</td><td>table</td></tr>
<tr><td>xt1</td><td>3</td><td>t1</td></tr>
<tr><td>xt2</td><td>1</td><td>Testing</td></tr>
<tr><td>xt2</td><td>2</td><td>table</td></tr>
<tr><td>xt2</td><td>3</td><td>t2</td></tr>
</tbody></table>

Now if you send the query:

```sql
select * from total where tabname = 'xt2';
```

CONNECT will analyze the where clause and only read the <em>xt1</em> table. This can
save time if you want to retrieve only a few sub-tables from a TBL table
containing many sub-tables.

### Parallel Execution

Parallel Execution is currently unavailable until some bugs are fixed.

When the sub-tables are located on different servers, it is possible to execute the remote queries simultaneously instead of sequentially. To enable this, set the thread option to yes.

Additional options available for this table type:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td>Maxerr</td><td>The max number of missing tables in the table list before an error is raised. Defaults to 0.</td></tr>
<tr><td>Accept</td><td>If true, missing columns are accepted and return null values. Defaults to false.</td></tr>
<tr><td>Thread</td><td>If true, enables parallel execution of remote sub-tables.</td></tr>
</tbody></table>

These options can be specified in the `OPTION_LIST`.

