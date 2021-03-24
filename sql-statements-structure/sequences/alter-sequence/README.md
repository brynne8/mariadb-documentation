# ALTER SEQUENCE

##### MariaDB starting with [10.3.1](/kb/en/mariadb-1031-release-notes/)

`ALTER SEQUENCE` was introduced in [MariaDB 10.3](/kb/en/what-is-mariadb-103/).

## Syntax

```sql
ALTER SEQUENCE [IF EXISTS] sequence_name
[ INCREMENT [ BY | = ] increment ]
[ MINVALUE [=] minvalue | NO MINVALUE | NOMINVALUE ]
[ MAXVALUE [=] maxvalue | NO MAXVALUE | NOMAXVALUE ]
[ START [ WITH | = ] start ] [ CACHE [=] cache ] [ [ NO ] CYCLE ]
[ RESTART [[WITH | =] restart]
```

`ALTER SEQUENCE` allows one to change any values for a `SEQUENCE` created with [CREATE SEQUENCE](/sql-statements-structure/sequences/create-sequence/).

The options for `ALTER SEQUENCE` can be given in any order.

## Description

`ALTER SEQUENCE` changes the parameters of an existing sequence generator. Any parameters not specifically set in the `ALTER SEQUENCE` command retain their prior settings.

`ALTER SEQUENCE` requires the [ALTER privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/).

### Arguments to `ALTER SEQUENCE`

The following options may be used:

<table><tbody><tr><th>Option</th><th>Default value</th><th>Description</th></tr>
<tr><td><code>INCREMENT</code></td><td>1</td><td>Increment to use for values. May be negative.</td></tr>
<tr><td><code>MINVALUE</code></td><td>1 if <code>INCREMENT</code> &gt; 0 and -9223372036854775807 if <code>INCREMENT</code> &lt; 0</td><td>Minimum value for the sequence.</td></tr>
<tr><td><code>MAXVALUE</code></td><td>9223372036854775806 if <code>INCREMENT</code> &gt; 0 and -1 if <code>INCREMENT</code> &lt; 0</td><td>Max value for sequence.</td></tr>
<tr><td><code>START</code></td><td><code>MINVALUE</code> if <code>INCREMENT</code> &gt; 0 and <code>MAX_VALUE</code> if <code>INCREMENT</code>&lt; 0</td><td>First value that the sequence will generate.</td></tr>
<tr><td><code>CACHE</code></td><td>1000</td><td>Number of values that should be cached. 0 if no <code>CACHE</code>.  The underlying table will be updated first time a new sequence number is generated and each time the cache runs out.</td></tr>
<tr><td><code>CYCLE</code></td><td>0 (= <code>NO CYCLE</code>)</td><td>1 if the sequence should start again from <code>MINVALUE</code># after it has run out of values.</td></tr>
<tr><td><code>RESTART</code></td><td><code>START</code> if <code>restart</code> value not is given</td><td>&nbsp;If  <code>RESTART</code> option is used, <code>NEXT VALUE</code> will return the restart value.</td></tr>
</tbody></table>

The optional clause `RESTART [ WITH restart ]` sets the next value for the sequence. This is equivalent to calling the [SETVAL()](/sql-statements-structure/sequences/sequence-functions/setval/) function with the `is_used` argument as 0. The specified value will be returned by the next call of nextval.  Using `RESTART` with no restart value is
equivalent to supplying the start value that was recorded by
[CREATE SEQUENCE](/sql-statements-structure/sequences/create-sequence/) or last set by `ALTER SEQUENCE START WITH`.

`ALTER SEQUENCE` will not allow you to change the sequence so that it's inconsistent. For example:

```sql
CREATE SEQUENCE s1;
ALTER SEQUENCE s1 MINVALUE 10;
ERROR 4061 (HY000): Sequence 'test.t1' values are conflicting

ALTER SEQUENCE s1 MINVALUE 10 RESTART 10;
ERROR 4061 (HY000): Sequence 'test.t1' values are conflicting

ALTER SEQUENCE s1 MINVALUE 10 START 10 RESTART 10;
```

### INSERT

To allow `SEQUENCE` objects to be backed up by old tools, like [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/), one can use `SELECT` to read the current state of a `SEQUENCE` object and use an `INSERT` to update the `SEQUENCE` object.  `INSERT` is only allowed if all fields are specified:

```sql
CREATE SEQUENCE s1;
INSERT INTO s1 VALUES(1000,10,2000,1005,1,1000,0,0);
SELECT * FROM s1;

+------------+-----------+-----------+-------+-----------+-------+-------+-------+
| next_value | min_value | max_value | start | increment | cache | cycle | round |
+------------+-----------+-----------+-------+-----------+-------+-------+-------+
|       1000 |        10 |      2000 |  1005 |         1 |  1000 |     0 |     0 |
+------------+-----------+-----------+-------+-----------+-------+-------+-------+

SHOW CREATE SEQUENCE s1;
+-------+--------------------------------------------------------------------------------------------------------------+
| Table | Create Table                                                                                                 |
+-------+--------------------------------------------------------------------------------------------------------------+
| s1    | CREATE SEQUENCE `s1` start with 1005 minvalue 10 maxvalue 2000 increment by 1 cache 1000 nocycle ENGINE=Aria |
+-------+--------------------------------------------------------------------------------------------------------------+
```

### Notes

`ALTER SEQUENCE` will instantly affect all future `SEQUENCE` operations.  This is in contrast to some other databases where the changes requested by `ALTER SEQUENCE` will not be seen until the sequence cache has run out.

`ALTER SEQUENCE` will take a full table lock of the sequence object during
its (brief) operation. This ensures that `ALTER SEQUENCE` is replicated
correctly.  If you only want to set the next sequence value to a
higher value than current, then you should use [SETVAL()](/sql-statements-structure/sequences/sequence-functions/setval/)
instead, as this is not blocking.

If you want to change storage engine, sequence comment or rename the sequence, you can use [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/) for this.

## See Also

- [Sequence Overview](/sql-statements-structure/sequences/sequence-overview/)
- [CREATE SEQUENCE](/sql-statements-structure/sequences/create-sequence/)
- [DROP SEQUENCE](/sql-statements-structure/sequences/drop-sequence/)
- [NEXT VALUE FOR](/sql-statements-structure/sequences/sequence-functions/next-value-for-sequence_name/)
- [PREVIOUS VALUE FOR](/sql-statements-structure/sequences/sequence-functions/previous-value-for-sequence_name/)
- [SETVAL()](/sql-statements-structure/sequences/sequence-functions/setval/).  Set next value for the sequence.
- [AUTO INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/)
- [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table/)