# GET_FORMAT

## Syntax

```sql
GET_FORMAT({DATE|DATETIME|TIME}, {'EUR'|'USA'|'JIS'|'ISO'|'INTERNAL'})
```

## Description

Returns a format string. This function is useful in combination with
the [DATE_FORMAT()](/built-in-functions/date-time-functions/date_format) and the [STR_TO_DATE()](/built-in-functions/date-time-functions/str_to_date) functions.

Possible result formats are:

<table><tbody><tr><th>Function Call</th><th>Result Format</th></tr>
<tr><td>GET_FORMAT(DATE,'EUR')</td><td>'%d.%m.%Y'</td></tr>
<tr><td>GET_FORMAT(DATE,'USA')</td><td>'%m.%d.%Y'</td></tr>
<tr><td>GET_FORMAT(DATE,'JIS')</td><td>'%Y-%m-%d'</td></tr>
<tr><td>GET_FORMAT(DATE,'ISO')</td><td>'%Y-%m-%d'</td></tr>
<tr><td>GET_FORMAT(DATE,'INTERNAL')</td><td>'%Y%m%d'</td></tr>
<tr><td>GET_FORMAT(DATETIME,'EUR')</td><td>'%Y-%m-%d %H.%i.%s'</td></tr>
<tr><td>GET_FORMAT(DATETIME,'USA')</td><td>'%Y-%m-%d %H.%i.%s'</td></tr>
<tr><td>GET_FORMAT(DATETIME,'JIS')</td><td>'%Y-%m-%d %H:%i:%s'</td></tr>
<tr><td>GET_FORMAT(DATETIME,'ISO')</td><td>'%Y-%m-%d %H:%i:%s'</td></tr>
<tr><td>GET_FORMAT(DATETIME,'INTERNAL')</td><td>'%Y%m%d%H%i%s'</td></tr>
<tr><td>GET_FORMAT(TIME,'EUR')</td><td>'%H.%i.%s'</td></tr>
<tr><td>GET_FORMAT(TIME,'USA')</td><td>'%h:%i:%s %p'</td></tr>
<tr><td>GET_FORMAT(TIME,'JIS')</td><td>'%H:%i:%s'</td></tr>
<tr><td>GET_FORMAT(TIME,'ISO')</td><td>'%H:%i:%s'</td></tr>
<tr><td>GET_FORMAT(TIME,'INTERNAL')</td><td>'%H%i%s'</td></tr>
</tbody></table>

## Examples

Obtaining the string matching to the standard European date format:

```sql
SELECT GET_FORMAT(DATE, 'EUR');
+-------------------------+
| GET_FORMAT(DATE, 'EUR') |
+-------------------------+
| %d.%m.%Y                |
+-------------------------+
```

Using the same string to format a date:

```sql
SELECT DATE_FORMAT('2003-10-03',GET_FORMAT(DATE,'EUR'));
+--------------------------------------------------+
| DATE_FORMAT('2003-10-03',GET_FORMAT(DATE,'EUR')) |
+--------------------------------------------------+
| 03.10.2003                                       |
+--------------------------------------------------+

SELECT STR_TO_DATE('10.31.2003',GET_FORMAT(DATE,'USA'));
+--------------------------------------------------+
| STR_TO_DATE('10.31.2003',GET_FORMAT(DATE,'USA')) |
+--------------------------------------------------+
| 2003-10-31                                       |
+--------------------------------------------------+
```