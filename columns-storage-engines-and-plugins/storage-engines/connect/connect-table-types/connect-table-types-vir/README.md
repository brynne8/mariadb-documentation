# CONNECT Table Types - VIR

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The VIR virtual type for the CONNECT handler was introduced in [MariaDB 10.0.15](/kb/en/mariadb-10015-release-notes/).

## VIR Type

A VIR table is a virtual table having only Special or Virtual columns. Its only property is its “size”, or cardinality, meaning the number of virtual rows it contains. It is created using the syntax:

```sql
CREATE TABLE name [coldef] ENGINE=CONNECT TABLE_TYPE=VIR
[BLOCK_SIZE=n];
```

The optional `BLOCK_SIZE` option gives the size of the table, defaulting to 1 if not specified. When its columns are not specified, it is almost equivalent to a [SEQUENCE](/kb/en/sequence/) table “seq_1_to_Size”.

### Displaying constants or expressions

Many DBMS use a no-column one-line table to do this, often call “dual”. MySQL and MariaDB use
syntax where no table is specified. With CONNECT, you can achieve the same purpose with a virtual
table, with the noticeable advantage of being able to display several lines. For example:

```sql
create table virt engine=connect table_type=VIR block_size=10;
select concat('The square root of ', n, ' is') what,
round(sqrt(n),16) value from virt;
```

This will return:

<table><tbody><tr><th>what</th><th>value</th></tr>
<tr><td>The square root of 1 is</td><td>1.0000000000000000</td></tr>
<tr><td>The square root of 2 is</td><td>1.4142135623730951</td></tr>
<tr><td>The square root of 3 is</td><td>1.7320508075688772</td></tr>
<tr><td>The square root of 4 is</td><td>2.0000000000000000</td></tr>
<tr><td>The square root of 5 is</td><td>2.2360679774997898</td></tr>
<tr><td>The square root of 6 is</td><td>2.4494897427831779</td></tr>
<tr><td>The square root of 7 is</td><td>2.6457513110645907</td></tr>
<tr><td>The square root of 8 is</td><td>2.8284271247461903</td></tr>
<tr><td>The square root of 9 is</td><td>3.0000000000000000</td></tr>
<tr><td>The square root of 10 is</td><td>3.1622776601683795</td></tr>
</tbody></table>

What happened here? First of all, unlike Oracle “dual” tableS that have no columns, a MariaDB table must have at least one column. By default, CONNECT creates VIR tables with one special column. This can be seen with the SHOW CREATE TABLE statement:

```sql
CREATE TABLE `virt` (
`n` int(11) NOT NULL `SPECIAL`=ROWID,
PRIMARY KEY (`n`)
) ENGINE=CONNECT DEFAULT CHARSET=latin1 `TABLE_TYPE`='VIR'
`BLOCK_SIZE`=10
```

This special column is called “n” and its value is the row number starting from 1. It is purely a virtual
table and no data file exists corresponding to it and to its index.
It is possible to specify the columns of a VIR table but they must be CONNECT special columns or
virtual columns. For instance:

```sql
create table virt2 (
n int key not null special=rowid,
sig1 bigint as ((n*(n+1))/2) virtual,
sig2 bigint as(((2*n+1)*(n+1)*n)/6) virtual)
engine=connect table_type=VIR block_size=10000000;
select * from virt2 limit 995, 5;
```

This table shows the sum and the sum of the square of the n first integers:

<table><tbody><tr><th>n</th><th>sig1</th><th>sig2</th></tr>
<tr><td>996</td><td>496506</td><td>329845486</td></tr>
<tr><td>997</td><td>497503</td><td>330839495</td></tr>
<tr><td>998</td><td>498501</td><td>331835499</td></tr>
<tr><td>999</td><td>499500</td><td>332833500</td></tr>
<tr><td>1000</td><td>500500</td><td>333833500</td></tr>
</tbody></table>

Note that the size of the table can be made very big as there no physical data. However, the result
should be limited in the queries. For instance:

```sql
select * from virt2 where n = 1664510;
```

Such a query could last very long if the rowid column were not indexed. Note that by default,
CONNECT declares the “n” column as a primary key. Actually, VIR tables can be indexed but only on
the ROWID (or ROWNUM) columns of the table. This is a virtual index for which no data is stored.

### Generating a Table filled with constant values

An interesting use of virtual tables, which often cannot be achieved with a table of any other type, is to
generate a table containing constant values.
This is easily done with a virtual table. Let us define the table FILLER as:

```sql
create table filler engine=connect table_type=VIR block_size=5000000;
```

Here we choose a size larger than the biggest table we want to generate. Later if we need a table pre-
filled with default and/or null values, we can do for example:

```sql
create table tp (
id int(6) key not null,
name char(16) not null,
salary float(8,2));
insert into tp select n, 'unknown', NULL from filler where n <= 10000;
```

This will generate a table having 10000 rows that can be updated later when needed. Note that a [SEQUENCE](/kb/en/sequence/) table could have been used here instead of FILLING .

### VIR tables vs. SEQUENCE tables

With just its default column, a VIR table is almost equivalent to a [SEQUENCE](/kb/en/sequence/) table. The syntax used is the main difference, for instance:

```sql
select * from seq_100_to_150_step_10;
```

can be obtained with a VIR table (of size &gt;= 15) by:

```sql
select n*10 from vir where n between 10 and 15;
```

Therefore, the main difference is to be able to define the columns of VIR tables. Unfortunately, there are currently many limitations to virtual columns that hopefully should be removed in the future.