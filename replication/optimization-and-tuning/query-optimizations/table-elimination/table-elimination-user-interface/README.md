# Table Elimination User Interface

One can check that table elimination is working by looking at the
output of <code class="highlight fixed" style="white-space:pre-wrap">EXPLAIN [EXTENDED]</code> and not finding there the
tables that were eliminated:

```sql
MySQL [test]> explain select ACRAT_rating from actors where ACNAM_name=’Gary Oldman’;
+----+--------------------+-----------+--------+---------------+---------+---------+----------------------+------+-------------+
| id | select_type        | table     | type   | possible_keys | key     | key_len | ref                  | rows | Extra       |
+----+--------------------+-----------+--------+---------------+---------+---------+----------------------+------+-------------+
|  1 | PRIMARY            | ac_anchor | index  | PRIMARY       | PRIMARY | 4       | NULL                 |    2 | Using index |
|  1 | PRIMARY            | ac_name   | eq_ref | PRIMARY       | PRIMARY | 4       | test.ac_anchor.AC_ID |    1 | Using where |
|  1 | PRIMARY            | ac_rating | ref    | PRIMARY       | PRIMARY | 4       | test.ac_anchor.AC_ID |    1 |             |
|  3 | DEPENDENT SUBQUERY | sub       | ref    | PRIMARY       | PRIMARY | 4       | test.ac_rating.AC_ID |    1 | Using index |
+----+--------------------+-----------+--------+---------------+---------+---------+----------------------+------+-------------+
4 rows in set (0.01 sec)
```

Note that <code class="highlight fixed" style="white-space:pre-wrap">ac_dob</code> table is not in the output. Now let's try
getting birthdate instead:

```sql
MySQL [test]> explain select ACDOB_birthdate from actors where ACNAM_name=’Gary Oldman’;
+----+-------------+-----------+--------+---------------+---------+---------+----------------------+------+-------------+
| id | select_type | table     | type   | possible_keys | key     | key_len | ref                  | rows | Extra       |
+----+-------------+-----------+--------+---------------+---------+---------+----------------------+------+-------------+
|  1 | PRIMARY     | ac_anchor | index  | PRIMARY       | PRIMARY | 4       | NULL                 |    2 | Using index |
|  1 | PRIMARY     | ac_name   | eq_ref | PRIMARY       | PRIMARY | 4       | test.ac_anchor.AC_ID |    1 | Using where |
|  1 | PRIMARY     | ac_dob    | eq_ref | PRIMARY       | PRIMARY | 4       | test.ac_anchor.AC_ID |    1 |             |
+----+-------------+-----------+--------+---------------+---------+---------+----------------------+------+-------------+
3 rows in set (0.01 sec)
```

The <code class="highlight fixed" style="white-space:pre-wrap">ac_dob</code> table is there while <code class="highlight fixed" style="white-space:pre-wrap">ac_rating</code>
and the subquery are gone. Now, if we just want to check the name of the actor:

```sql
MySQL [test]> explain select count(*) from actors where ACNAM_name=’Gary Oldman’;
+----+-------------+-----------+--------+---------------+---------+---------+----------------------+------+-------------+
| id | select_type | table     | type   | possible_keys | key     | key_len | ref                  | rows | Extra       |
+----+-------------+-----------+--------+---------------+---------+---------+----------------------+------+-------------+
|  1 | PRIMARY     | ac_anchor | index  | PRIMARY       | PRIMARY | 4       | NULL                 |    2 | Using index |
|  1 | PRIMARY     | ac_name   | eq_ref | PRIMARY       | PRIMARY | 4       | test.ac_anchor.AC_ID |    1 | Using where |
+----+-------------+-----------+--------+---------------+---------+---------+----------------------+------+-------------+
2 rows in set (0.01 sec)
```

In this case it will eliminate both the <code class="highlight fixed" style="white-space:pre-wrap">ac_dob</code> and
<code class="highlight fixed" style="white-space:pre-wrap">ac_rating</code> tables.

Removing tables from a query does not make the query slower, and it does not
cut off any optimization opportunities, so table elimination is unconditional
and there are no plans on having any kind of query hints for it.

For debugging purposes there is a <code class="highlight fixed" style="white-space:pre-wrap">table_elimination=on|off</code>
switch in debug builds of the server.

## See Also

- This page is based on the following blog post about table elimination:
  [http://s.petrunia.net/blog/?p=58](http://s.petrunia.net/blog/?p=58)