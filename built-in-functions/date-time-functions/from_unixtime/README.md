# FROM_UNIXTIME

## Syntax

```sql
FROM_UNIXTIME(unix_timestamp), FROM_UNIXTIME(unix_timestamp,format)
```

## Description

Returns a representation of the unix_timestamp argument as a value in
'YYYY-MM-DD HH:MM:SS' or YYYYMMDDHHMMSS.uuuuuu format, depending on
whether the function is used in a string or numeric context. The value
is expressed in the current [time zone](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones). unix_timestamp is an internal
timestamp value such as is produced by the [UNIX_TIMESTAMP()](/built-in-functions/date-time-functions/unix_timestamp) function.

If format is given, the result is formatted according to the format
string, which is used the same way as listed in the entry for the
[DATE_FORMAT()](/built-in-functions/date-time-functions/date_format) function.

Timestamps in MariaDB have a maximum value of 2147483647, equivalent to 2038-01-19 05:14:07. This is due to the underlying 32-bit limitation. Using the function on a timestamp beyond this will result in NULL being returned. Use [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime) as a storage type if you require dates beyond this.

The options that can be used by FROM_UNIXTIME(), as well as [DATE_FORMAT()](/built-in-functions/date-time-functions/date_format) and [STR_TO_DATE()](/built-in-functions/date-time-functions/str_to_date), are:

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
<tr><td><code>%w</code></td><td>Day of the week. 0 = Sunday, 1 = Saturday.</td></tr>
<tr><td><code>%X</code></td><td>Year with 4 digits when first day of the week is Sunday. Used with %V.</td></tr>
<tr><td><code>%x</code></td><td>Year with 4 digits when first day of the week is Sunday. Used with %v.</td></tr>
<tr><td><code>%Y</code></td><td>Year with 4 digits.</td></tr>
<tr><td><code>%y</code></td><td>Year with 2 digits.</td></tr>
<tr><td><code>%#</code></td><td>For <a href="/kb/en/str_to_date/">str_to_date</a>(), skip all numbers.</td></tr>
<tr><td><code>%.</code></td><td>For <a href="/kb/en/str_to_date/">str_to_date</a>(), skip all punctation characters.</td></tr>
<tr><td><code>%@</code></td><td>For <a href="/kb/en/str_to_date/">str_to_date</a>(), skip all alpha characters.</td></tr>
<tr><td><code>%%</code></td><td>A literal <code>%</code> character.</td></tr>
</tbody></table>

## Performance Considerations

If your [session time zone](/kb/en/server-system-variables/#time_zone) is set to `SYSTEM` (the default), `FROM_UNIXTIME()` will call the OS function to convert the data using the system time zone. At least on Linux, the corresponding function (`localtime_r`) uses a global mutex inside glibc that can cause contention under high concurrent load.

Set your time zone to a named time zone to avoid this issue. See [mysql time zone tables](/kb/en/time-zones/#mysql-time-zone-tables) for details on how to do this.

## Examples

```sql
SELECT FROM_UNIXTIME(1196440219);
+---------------------------+
| FROM_UNIXTIME(1196440219) |
+---------------------------+
| 2007-11-30 11:30:19       |
+---------------------------+

SELECT FROM_UNIXTIME(1196440219) + 0;
+-------------------------------+
| FROM_UNIXTIME(1196440219) + 0 |
+-------------------------------+
|         20071130113019.000000 |
+-------------------------------+

SELECT FROM_UNIXTIME(UNIX_TIMESTAMP(), '%Y %D %M %h:%i:%s %x');
+---------------------------------------------------------+
| FROM_UNIXTIME(UNIX_TIMESTAMP(), '%Y %D %M %h:%i:%s %x') |
+---------------------------------------------------------+
| 2010 27th March 01:03:47 2010                           |
+---------------------------------------------------------+
```

## See Also

- [UNIX_TIMESTAMP()](/built-in-functions/date-time-functions/unix_timestamp)
- [DATE_FORMAT()](/built-in-functions/date-time-functions/date_format)
- [STR_TO_DATE()](/built-in-functions/date-time-functions/str_to_date)