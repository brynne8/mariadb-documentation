# STR_TO_DATE

## Syntax

```sql
STR_TO_DATE(str,format)
```

## Description

This is the inverse of the [DATE_FORMAT](/built-in-functions/date-time-functions/date_format/)() function. It takes
a string `str` and a format string `format`. `STR_TO_DATE()` returns a
`DATETIME` value if the format string contains both date and time parts, or a
`DATE` or `TIME` value if the string contains only date or time parts.

The date, time, or datetime values contained in `str` should be given in the format indicated by format. If str contains an illegal date, time, or datetime value, `STR_TO_DATE()` returns `NULL`. An illegal value also produces a warning.

The options that can be used by STR_TO_DATE(), as well as its inverse [DATE_FORMAT()](/built-in-functions/date-time-functions/date_format/) and the [FROM_UNIXTIME()](/built-in-functions/date-time-functions/from_unixtime/) function, are:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>%a</code></td><td>Short weekday name in current locale (Variable <a href="/kb/en/server-system-variables/#lc_time_names">lc_time_names</a>).</td></tr>
<tr><td><code>%b</code></td><td>Short form month name in current locale. For locale en_US this is one of: Jan,Feb,Mar,Apr,May,Jun,Jul,Aug,Sep,Oct,Nov or Dec.</td></tr>
<tr><td><code>%c</code></td><td>Month with 1 or 2 digits.</td></tr>
<tr><td><code>%D</code></td><td>Day with English suffix 'th', 'nd', 'st' or 'rd''. (1st, 2nd, 3rd...).</td></tr>
<tr><td><code>%d</code></td><td>Day with 2 digits.</td></tr>
<tr><td><code>%e</code></td><td>Day with 1 or 2 digits.</td></tr>
<tr><td><code>%f</code></td><td>Sub seconds 6 digits.</td></tr>
<tr><td><code>%H</code></td><td>Hour with 2 digits between 00-23.</td></tr>
<tr><td><code>%h</code></td><td>Hour with 2 digits between 01-12.</td></tr>
<tr><td><code>%I</code></td><td>Hour with 2 digits between 01-12.</td></tr>
<tr><td><code>%i</code></td><td>Minute with 2 digits.</td></tr>
<tr><td><code>%j</code></td><td>Day of the year (001-366)</td></tr>
<tr><td><code>%k</code></td><td>Hour with 1 digits between 0-23.</td></tr>
<tr><td><code>%l</code></td><td>Hour with 1 digits between 1-12.</td></tr>
<tr><td><code>%M</code></td><td>Full month name in current locale (Variable <a href="/kb/en/server-system-variables/#lc_time_names">lc_time_names</a>).</td></tr>
<tr><td><code>%m</code></td><td>Month with 2 digits.</td></tr>
<tr><td><code>%p</code></td><td>AM/PM according to current locale (Variable <a href="/kb/en/server-system-variables/#lc_time_names">lc_time_names</a>).</td></tr>
<tr><td><code>%r</code></td><td>Time in 12 hour format, followed by AM/PM. Short for '%I:%i:%S %p'.</td></tr>
<tr><td><code>%S</code></td><td>Seconds with 2 digits.</td></tr>
<tr><td><code>%s</code></td><td>Seconds with 2 digits.</td></tr>
<tr><td><code>%T</code></td><td>Time in 24 hour format. Short for '%H:%i:%S'.</td></tr>
<tr><td><code>%U</code></td><td>Week number (00-53), when first day of the week is Sunday.</td></tr>
<tr><td><code>%u</code></td><td>Week number (00-53), when first day of the week is Monday.</td></tr>
<tr><td><code>%V</code></td><td>Week number (01-53), when first day of the week is Sunday. Used with %X.</td></tr>
<tr><td><code>%v</code></td><td>Week number (01-53), when first day of the week is Monday. Used with %x.</td></tr>
<tr><td><code>%W</code></td><td>Full weekday name in current locale (Variable <a href="/kb/en/server-system-variables/#lc_time_names">lc_time_names</a>).</td></tr>
<tr><td><code>%w</code></td><td>Day of the week. 0 = Sunday, 6 = Saturday.</td></tr>
<tr><td><code>%X</code></td><td>Year with 4 digits when first day of the week is Sunday. Used with %V.</td></tr>
<tr><td><code>%x</code></td><td>Year with 4 digits when first day of the week is Monday. Used with %v.</td></tr>
<tr><td><code>%Y</code></td><td>Year with 4 digits.</td></tr>
<tr><td><code>%y</code></td><td>Year with 2 digits.</td></tr>
<tr><td><code>%#</code></td><td>For <a href="/kb/en/str_to_date/">str_to_date</a>(), skip all numbers.</td></tr>
<tr><td><code>%.</code></td><td>For <a href="/kb/en/str_to_date/">str_to_date</a>(), skip all punctation characters.</td></tr>
<tr><td><code>%@</code></td><td>For <a href="/kb/en/str_to_date/">str_to_date</a>(), skip all alpha characters.</td></tr>
<tr><td><code>%%</code></td><td>A literal <code>%</code> character.</td></tr>
</tbody></table>

## Examples

```sql
SELECT STR_TO_DATE('Wednesday, June 2, 2014', '%W, %M %e, %Y');
+---------------------------------------------------------+
| STR_TO_DATE('Wednesday, June 2, 2014', '%W, %M %e, %Y') |
+---------------------------------------------------------+
| 2014-06-02                                              |
+---------------------------------------------------------+


SELECT STR_TO_DATE('Wednesday23423, June 2, 2014', '%W, %M %e, %Y');
+--------------------------------------------------------------+
| STR_TO_DATE('Wednesday23423, June 2, 2014', '%W, %M %e, %Y') |
+--------------------------------------------------------------+
| NULL                                                         |
+--------------------------------------------------------------+
1 row in set, 1 warning (0.00 sec)

SHOW WARNINGS;
+---------+------+-----------------------------------------------------------------------------------+
| Level   | Code | Message                                                                           |
+---------+------+-----------------------------------------------------------------------------------+
| Warning | 1411 | Incorrect datetime value: 'Wednesday23423, June 2, 2014' for function str_to_date |
+---------+------+-----------------------------------------------------------------------------------+

SELECT STR_TO_DATE('Wednesday23423, June 2, 2014', '%W%#, %M %e, %Y');
+----------------------------------------------------------------+
| STR_TO_DATE('Wednesday23423, June 2, 2014', '%W%#, %M %e, %Y') |
+----------------------------------------------------------------+
| 2014-06-02                                                     |
+----------------------------------------------------------------+
```

## See Also

- [DATE_FORMAT()](/built-in-functions/date-time-functions/date_format/)
- [FROM_UNIXTIME()](/built-in-functions/date-time-functions/from_unixtime/)