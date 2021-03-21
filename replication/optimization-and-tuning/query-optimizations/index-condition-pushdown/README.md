# Index Condition Pushdown

Index Condition Pushdown is an optimization that is applied for access methods that access table data through indexes: `range`, `ref`, `eq_ref`, `ref_or_null`, and [Batched Key Access](/kb/en/block-based-join-algorithms/#batch-key-access-join).

The idea is to check part of the WHERE condition that refers to index fields (we call it <em>Pushed Index Condition</em>) as soon as we've accessed the index. If the <em>Pushed Index Condition</em> is not satisfied, we won't need to read the whole table record.

##### MariaDB starting with [5.3.3](/kb/en/mariadb-533-release-notes/)

Starting in [MariaDB 5.3.3](/kb/en/mariadb-533-release-notes/), Index Condition Pushdown is <strong>on</strong> by default. To disable it, set its optimizer_switch flag like so:

```sql
SET optimizer_switch='index_condition_pushdown=off'
```

When Index Condition Pushdown is used, EXPLAIN will show "Using index condition":

```sql
MariaDB [test]> explain select * from tbl where key_col1 between 10 and 11 and key_col2 like '%foo%';
+----+-------------+-------+-------+---------------+----------+---------+------+------+-----------------------+
| id | select_type | table | type  | possible_keys | key      | key_len | ref  | rows | Extra                 |
+----+-------------+-------+-------+---------------+----------+---------+------+------+-----------------------+
|  1 | SIMPLE      | tbl   | range | key_col1      | key_col1 | 5       | NULL |    2 | Using index condition |
+----+-------------+-------+-------+---------------+----------+---------+------+------+-----------------------+
1 row in set (0.01 sec)
```

## The idea behind index condition pushdown

In disk-based storage engines, making an index lookup is done in two steps, like shown on the picture:

<img src="/kb/en/index-condition-pushdown/+image/index-access-2phases" alt="index-access-2phases" title="index-access-2phases">

Index Condition Pushdown optimization tries to cut down the number of full record reads by checking whether index records satisfy part of the WHERE condition that can be checked for them:

<img src="/kb/en/index-condition-pushdown/+image/index-access-with-icp" alt="index-access-with-icp" title="index-access-with-icp">

How much speed will be gained depends on
- How many records will be filtered out 
- How expensive it was to read them

The former depends on the query and the dataset. The latter is generally bigger when table records are on disk and/or are big, especially when they have [blobs](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/).

## Example speedup

I used DBT-3 benchmark data, with scale factor=1. Since the benchmark defines very few indexes, we've added a multi-column index (index condition pushdown is usually useful with multi-column indexes: the first component(s) is what index access is done for, the subsequent have columns that we read and check conditions on).

```sql
alter table lineitem add index s_r (l_shipdate, l_receiptdate);
```

The query was to find big (l_quantity &gt; 40) orders that were made in January 1993 that took more than 25 days to ship:

```sql
select count(*) from lineitem
where
  l_shipdate between '1993-01-01' and '1993-02-01' and
  datediff(l_receiptdate,l_shipdate) > 25 and
  l_quantity > 40;
```

EXPLAIN without Index Condition Pushdown:

```sql
-+----------+-------+----------------------+-----+---------+------+--------+-------------+
 | table    | type | possible_keys         | key | key_len | ref | rows    | Extra       |
-+----------+-------+----------------------+-----+---------+------+--------+-------------+
 | lineitem | range | s_r                  | s_r | 4       | NULL | 152064 | Using where |
-+----------+-------+----------------------+-----+---------+------+--------+-------------+
```

with Index Condition Pushdown:

```sql
-+-----------+-------+---------------+-----+---------+------+--------+------------------------------------+
 | table     | type | possible_keys | key | key_len | ref | rows     | Extra                              |
-+-----------+-------+---------------+-----+---------+------+--------+------------------------------------+
 | lineitem | range | s_r            | s_r | 4       | NULL | 152064 | Using index condition; Using where |
-+-----------+-------+---------------+-----+---------+------+--------+------------------------------------+
```

The speedup was:

- Cold buffer pool: from 5 min down to 1 min
- Hot buffer pool: from 0.19 sec down to 0.07 sec

## Status variables

There are two server status variables:

<table><tbody><tr><th>Variable name</th><th>Meaning</th></tr>
<tr><td><a href="/kb/en/server-status-variables/#handler_icp_attempts">Handler_icp_attempts</a></td><td>Number of times pushed index condition was checked.</td></tr>
<tr><td><a href="/kb/en/server-status-variables/#handler_icp_match">Handler_icp_match</a></td><td>Number of times the condition was matched.</td></tr>
</tbody></table>

That way, the value <code class="fixed" style="white-space:pre-wrap">Handler_icp_attempts - Handler_icp_match</code> shows the number records that the server did not have to read because of Index Condition Pushdown.

## See Also

- [What is MariaDB 5.3](/kb/en/what-is-mariadb-53/)
- [Index Condition Pushdown](http://dev.mysql.com/doc/refman/5.6/en/index-condition-pushdown-optimization.html) in MySQL 5.6 manual (MariaDB's and MySQL 5.6's Index Condition Pushdown implementations have the same ancestry so are very similar to one another).