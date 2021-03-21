# Using Buffer UPDATE Algorithm

This article explains the [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) statement's <em>Using Buffer</em> algorithm.

Take the following table and query:

<table><tbody><tr><th>Name</th><th>Salary</th></tr>
<tr><td>Babatunde</td><td>1000</td></tr>
<tr><td>Jolana</td><td>1050</td></tr>
<tr><td>Pankaja</td><td>1300</td></tr>
</tbody></table>

```sql
UPDATE employees SET salary = salary+100 WHERE salary < 2000;
```

Suppose the <em>employees</em> table has an index on the <em>salary</em> column, and the optimizer decides to use a range scan on that index.

The optimizer starts a range scan on the <em>salary</em> index. We find the first record <em>Babatunde, 1000</em>. If we do an on-the-fly update, we immediately instruct the storage engine to change this record to be <em>Babatunde, 1000+100=1100</em>.

Then we proceed to search for the next record, and find <em>Jolana, 1050</em>. We instruct the storage engine to update it to be <em>Jolana, 1050+100=1150</em>.

Then we proceed to search for the next record ... and what happens next depends on the storage engine. In some storage engines, data changes are visible immediately, so we will find find the <em>Babatunde, 1100</em> record that we wrote at the first step, modifying it again, giving Babatunde an undeserved raise. Then we will see Babatunde again and again, looping continually.

In order to prevent such situations, the optimizer checks whether the UPDATE statement is going to change key values for the keys it is using. In that case, it will use a different algorithm:

1 Scan everyone with "salary&lt;2000", remembering the rowids of the rows in a buffer.
2 Read the buffer and apply the updates.

This way, each row will be updated only once.

The `Using buffer` [EXPLAIN](/sql-statements-structure/sql-statements/administrative-sql-statements/analyze-and-explain-statements/explain/) output indicates that the buffer as described above will be used. The algorithm has always been in use, but has only been made visible in the `EXPLAIN` output since [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/).