# Table Elimination in MariaDB

The first thing the MariaDB optimizer does is to merge the <code class="highlight fixed" style="white-space:pre-wrap">VIEW</code>
definition into the query to obtain:

```sql
select ACRAT_rating
from
  ac_anchor
  left join ac_name on ac_anchor.AC_ID=ac_name.AC_ID
  left join ac_dob on ac_anchor.AC_ID=ac_dob.AC_ID
  left join ac_rating on (ac_anchor.AC_ID=ac_rating.AC_ID and
                          ac_rating.ACRAT_fromdate = 
                            (select max(sub.ACRAT_fromdate)
                             from ac_rating sub where sub.AC_ID = ac_rating.AC_ID))
where
 ACNAM_name='Gary Oldman'
```

It's important to realize that the obtained query has a useless part:

- <code class="highlight fixed" style="white-space:pre-wrap">left join ac_dob on ac_dob.AC_ID=...</code> will produce exactly
  one matching record:
<ul start="1"><li><code class="highlight fixed" style="white-space:pre-wrap">primary key(ac_dob.AC_ID)</code> guarantees that there will be
   at most one match for any value of <code class="highlight fixed" style="white-space:pre-wrap">ac_anchor.AC_ID</code>,
</li><li>and if there won't be a match, <code class="highlight fixed" style="white-space:pre-wrap">LEFT JOIN</code> will generate a
   NULL-complemented “row”
</li></ul>
- and we don't care what the matching record is, as table
  <code class="highlight fixed" style="white-space:pre-wrap">ac_dob</code> is not used anywhere else in the query.

This means that the <code class="highlight fixed" style="white-space:pre-wrap">left join ac_dob on ...</code> part can be
removed from the query and this is what Table Elimination module does.  The
detection logic is rather smart, for example it would be able to remove the
<code class="highlight fixed" style="white-space:pre-wrap">left join ac_rating on ...</code> part as well, together with the
subquery (in the above example it won't be removed because ac_rating used in
the selection list of the query). The Table Elimination module can also handle
nested outer joins and multi-table outer joins.

## See Also

- This page is based on the following blog post about table elimination:
  [http://s.petrunia.net/blog/?p=58](http://s.petrunia.net/blog/?p=58)