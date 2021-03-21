# CONNECT XCOL Table Type

`XCOL` tables are based on another table or view, like <a undefined>PROXY</a> tables. This type can be
used when the object table has a column that contains a list of values.

Suppose we have a <em>'children'</em> table that can be displayed as:

<table><tbody><tr><th>name</th><th>childlist</th></tr>
<tr><td>Sophie</td><td>Vivian, Antony</td></tr>
<tr><td>Lisbeth</td><td>Lucy,Charles,Diana</td></tr>
<tr><td>Corinne</td><td></td></tr>
<tr><td>Claude</td><td>Marc</td></tr>
<tr><td>Janet</td><td>Arthur, Sandra, Peter, John</td></tr>
</tbody></table>

We can have a different view on these data, where each child will be associated
with his/her mother by creating an `XCOL` table by:

```sql
CREATE TABLE xchild (
  mother char(12) NOT NULL,
  child char(12) DEFAULT NULL flag=2
) ENGINE=CONNECT table_type=XCOL tabname='chlist'
option_list='colname=child';
```

The `COLNAME` option specifies the name of the column receiving the list
items.  This will return from:

```sql
select * from xchild;
```

The requested view:

<table><tbody><tr><th>mother</th><th>child</th></tr>
<tr><td>Sophia</td><td>Vivian</td></tr>
<tr><td>Sophia</td><td>Antony</td></tr>
<tr><td>Lisbeth</td><td>Lucy</td></tr>
<tr><td>Lisbeth</td><td>Charles</td></tr>
<tr><td>Lisbeth</td><td>Diana</td></tr>
<tr><td>Corinne</td><td>NULL</td></tr>
<tr><td>Claude</td><td>Marc</td></tr>
<tr><td>Janet</td><td>Arthur</td></tr>
<tr><td>Janet</td><td>Sandra</td></tr>
<tr><td>Janet</td><td>Peter</td></tr>
<tr><td>Janet</td><td>John</td></tr>
</tbody></table>

Several things should be noted here:

- When the original <em>children</em> field is void, what happens depends on the NULL specification of the "multiple" column. If it is nullable, like here, a void string will generate a NULL value. However, if the column is not nullable, no row will be generated at all.
- Blanks after the separator are ignored.
- No copy of the original data was done. Both tables use the same source data.
- Specifying the column definitions in the `CREATE TABLE` statement is optional.

The "multiple" column <em>child</em> can be used as any other column. For instance:

```sql
select * from xchild where substr(child,1,1) = 'A';
```

This will return:

<table><tbody><tr><th>Mother</th><th>Child</th></tr>
<tr><td>Sophia</td><td>Antony</td></tr>
<tr><td>Janet</td><td>Arthur</td></tr>
</tbody></table>

If a query does not involve the "multiple" column, no row multiplication will
be done. For instance:

```sql
select mother from xchild;
```

This will just return all the mothers:

<table><tbody><tr><th>mother</th></tr>
<tr><td>Sophia</td></tr>
<tr><td>Lisbeth</td></tr>
<tr><td>Corinne</td></tr>
<tr><td>Claude</td></tr>
<tr><td>Janet</td></tr>
</tbody></table>

The same occurs with other types of select statements, for instance:

```sql
select count(*) from xchild;      -- returns 5
select count(child) from xchild;  -- returns 10
select count(mother) from xchild; -- returns 5
```

Grouping also gives different result:

```sql
select mother, count(*) from xchild group by mother;
```

Replies:

<table><tbody><tr><th>mother</th><th>count(*)</th></tr>
<tr><td>Claude</td><td>1</td></tr>
<tr><td>Corinne</td><td>1</td></tr>
<tr><td>Janet</td><td>1</td></tr>
<tr><td>Lisbeth</td><td>1</td></tr>
<tr><td>Sophia</td><td>1</td></tr>
</tbody></table>

While the query:

```sql
select mother, count(child) from xchild group by mother;
```

Gives the more interesting result:

<table><tbody><tr><th>mother</th><th>count(child)</th></tr>
<tr><td>Claude</td><td>1</td></tr>
<tr><td>Corinne</td><td>0</td></tr>
<tr><td>Janet</td><td>4</td></tr>
<tr><td>Lisbeth</td><td>3</td></tr>
<tr><td>Sophia</td><td>2</td></tr>
</tbody></table>

Some more options are available for this table type:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td>Sep_char</td><td>The separator character used in the "multiple" column, defaults to the comma.</td></tr>
<tr><td>Mult</td><td>Indicates the max number of multiple items. It is used to internally calculate the max size of the table and defaults to 10. (To be specified in <code>OPTION_LIST</code>).</td></tr>
</tbody></table>

## Using Special Columns with XCOL

Special columns can be used in XCOL tables. The mostly useful one is ROWNUM that gives the rank of the value in the list of values. For instance:

```sql
CREATE TABLE xchild2 (
rank int NOT NULL SPECIAL=ROWID,
mother char(12) NOT NULL,
child char(12) NOT NULL flag=2
) ENGINE=CONNECT table_type=XCOL tabname='chlist' option_list='colname=child';
```

This table will be displayed as:

<table><tbody><tr><th>rank</th><th>mother</th><th>child</th></tr>
<tr><td>1</td><td>Sophia</td><td>Vivian</td></tr>
<tr><td>2</td><td>Sophia</td><td>Antony</td></tr>
<tr><td>1</td><td>Lisbeth</td><td>Lucy</td></tr>
<tr><td>2</td><td>Lisbeth</td><td>Charles</td></tr>
<tr><td>3</td><td>Lisbeth</td><td>Diana</td></tr>
<tr><td>1</td><td>Claude</td><td>Marc</td></tr>
<tr><td>1</td><td>Janet</td><td>Arthur</td></tr>
<tr><td>2</td><td>Janet</td><td>Sandra</td></tr>
<tr><td>3</td><td>Janet</td><td>Peter</td></tr>
<tr><td>4</td><td>Janet</td><td>John</td></tr>
</tbody></table>

To list only the first child of each mother you can do:

```sql
SELECT mother, child FROM xchild2 where rank = 1 ;
```

returning:

<table><tbody><tr><th>mother</th><th>child</th></tr>
<tr><td>Sophia</td><td>Vivian</td></tr>
<tr><td>Lisbeth</td><td>Lucy</td></tr>
<tr><td>Claude</td><td>Marc</td></tr>
<tr><td>Janet</td><td>Arthur</td></tr>
</tbody></table>

However, note the following pitfall: trying to get the names of all mothers having more than 2 children cannot be done by:

```sql
SELECT mother FROM xchild2 where rank > 2;
```

This is because with no row multiplication being done, the rank value is always 1. The correct way to obtain this result is longer but cannot use the ROWNUM column:

```sql
SELECT mother FROM xchild2 group by mother having count(child) > 2;
```

## XCOL tables based on specified views

Instead of specifying a source table name via the TABNAME option, it is possible to retrieve data from a
“view” whose definition is given in a new option SRCDEF . For instance:

```sql
create table xsvars engine=connect table_type=XCOL
srcdef='show variables like "optimizer_switch"'
option_list='Colname=Value';
```

Then, for instance:

```sql
select value from xsvars limit 10;
```

This will display something like:

<table><tbody><tr><th>value</th></tr>
<tr><td>index_merge=on</td></tr>
<tr><td>index_merge_union=on</td></tr>
<tr><td>index_merge_sort_union=on</td></tr>
<tr><td>index_merge_intersection=on</td></tr>
<tr><td>index_merge_sort_intersection=off</td></tr>
<tr><td>engine_condition_pushdown=off</td></tr>
<tr><td>index_condition_pushdown=on</td></tr>
<tr><td>derived_merge=on</td></tr>
<tr><td>derived_with_keys=on</td></tr>
<tr><td>firstmatch=on</td></tr>
</tbody></table>

Note: All XCOL tables are read only.