# Using CONNECT - Virtual and Special Columns

CONNECT supports MariaDB [virtual and persistent columns](/kb/en/virtual-columns/). It is also possible to declare a column as
being a CONNECT special column. Let us see on an example how this can be done. The boys table we
have seen previously can be recreated as:

```sql
create table boys (
  linenum int(6) not null default 0 special=rowid,
  name char(12) not null,
  city char(12) not null,
  birth date not null date_format='DD/MM/YYYY',
  hired date not null date_format='DD/MM/YYYY' flag=36,
  agehired int(3) as (floor(datediff(hired,birth)/365.25))
  virtual,
  fn char(100) not null default '' special=FILEID)
engine=CONNECT table_type=FIX file_name='boys.txt' mapped=YES lrecl=47;
```

We have defined two CONNECT special columns. You can give them any name; it is
the field SPECIAL option that specifies the special column functional name.

<strong>Note:</strong> the default values specified for the special columns do not mean
anything. They are specified just to prevent getting warning messages when
inserting new rows.

For the definition of the <em>agehired</em> virtual column, no CONNECT options can be specified as it has no offset or length, not being stored in the file.

The command:

```sql
select * from boys where city = 'boston';
```

will return:

<table><tbody><tr><th>linenum</th><th>name</th><th>city</th><th>birth</th><th>hired</th><th>agehired</th><th>fn</th></tr>
<tr><td>1</td><td>John</td><td>Boston</td><td>1986-01-25</td><td>2010-06-02</td><td>24</td><td>d:\mariadb\sql\data\boys.txt</td></tr>
<tr><td>2</td><td>Henry</td><td>Boston</td><td>1987-06-07</td><td>2008-04-01</td><td>20</td><td>d:\mariadb\sql\data\boys.txt</td></tr>
<tr><td>6</td><td>Bill</td><td>Boston</td><td>1986-09-11</td><td>2008-02-10</td><td>21</td><td>d:\mariadb\sql\data\boys.txt</td></tr>
</tbody></table>

Existing special columns are listed in the following table:

<table><tbody><tr><th>Special Name</th><th>Type</th><th>Description of the column value</th></tr>
<tr><td>ROWID</td><td>Integer</td><td>The row ordinal number in the table. This is not quite equivalent to a virtual column with an auto increment of 1 because rows are renumbered when deleting rows.</td></tr>
<tr><td>ROWNUM</td><td>Integer</td><td>The row ordinal number in the file. This is different from ROWID for multiple tables, TBL/XCOL/OCCUR/PIVOT tables, XML tables with a multiple column, and for DBF tables where ROWNUM includes soft deleted rows.</td></tr>
<tr><td>FILEID FDISK FPATH FNAME FTYPE</td><td>String</td><td>FILEID returns the full name of the file this row belongs to. Useful in particular for multiple tables represented by several files. The other special columns can be used to retrieve only one part of the full name.</td></tr>
<tr><td>TABID</td><td>String</td><td>The name of the table this row belongs to. Useful for TBL tables.</td></tr>
<tr><td>PARTID</td><td>String</td><td>The name of the partition this row belongs to. Specific to partitioned tables.</td></tr>
<tr><td>SERVID</td><td>String</td><td>The name of the federated server or server host used by a MYSQL table. “ODBC” for an ODBC table, "JDBC" for a JDBC table and “Current” for all other tables.</td></tr>
</tbody></table>

<strong>Note:</strong> CONNECT does not currently support auto incremented columns. However,
a `ROWID` special column will do the job of a column auto incremented by 1.

