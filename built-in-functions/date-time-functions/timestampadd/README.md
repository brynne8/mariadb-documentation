# TIMESTAMPADD

## Syntax

```sql
TIMESTAMPADD(unit,interval,datetime_expr)
```

## Description

Adds the integer expression interval to the date or datetime
expression datetime_expr. The unit for interval is given by the unit
argument, which should be one of the following values: MICROSECOND, SECOND, MINUTE, HOUR, DAY, WEEK, MONTH, QUARTER, or YEAR.

The unit value may be specified using one of keywords as shown, or
with a prefix of SQL_TSI_. For example, DAY and SQL_TSI_DAY both are
legal.

Before [MariaDB 5.5](/kb/en/what-is-mariadb-55/), FRAC_SECOND was permitted as a synonym for MICROSECOND.

## Examples

```sql
SELECT TIMESTAMPADD(MINUTE,1,'2003-01-02');
+-------------------------------------+
| TIMESTAMPADD(MINUTE,1,'2003-01-02') |
+-------------------------------------+
| 2003-01-02 00:01:00                 |
+-------------------------------------+

SELECT TIMESTAMPADD(WEEK,1,'2003-01-02');
+-----------------------------------+
| TIMESTAMPADD(WEEK,1,'2003-01-02') |
+-----------------------------------+
| 2003-01-09                        |
+-----------------------------------+
```