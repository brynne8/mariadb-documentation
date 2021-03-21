# MONTHNAME

## Syntax

```sql
MONTHNAME(date)
```

## Description

Returns the full name of the month for date. The language used for the name is controlled by the value of the [lc_time_names](/kb/en/server-system-variables/#lc_time_names) system variable. See [server locale](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/server-locale/) for more on the supported locales.

## Examples

```sql
SELECT MONTHNAME('2019-02-03');
+-------------------------+
| MONTHNAME('2019-02-03') |
+-------------------------+
| February                |
+-------------------------+
```

Changing the locale:

```sql
SET lc_time_names = 'fr_CA';

SELECT MONTHNAME('2019-05-21');
+-------------------------+
| MONTHNAME('2019-05-21') |
+-------------------------+
| mai                     |
+-------------------------+
```