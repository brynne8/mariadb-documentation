# mysql_tzinfo_to_sql

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-tzinfo-to-sql` is a symlink to `mysql_tzinfo_to_sql`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mysql_tzinfo_to_sql` is the symlink, and `mariadb-tzinfo-to-sql` the binary name.

`mysql_tzinfo_to_sql` is a utility used to load [time zones](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones) on systems that have a zoneinfo database to load the time zone tables ([time_zone](/kb/en/mysqltime_zone-table/), [time_zone_leap_second](/kb/en/mysqltime_zone_leap_second-table/), [time_zone_name](/kb/en/mysqltime_zone_name-table/), [time_zone_transition](/kb/en/mysqltime_zone_transition-table/) and [time_zone_transition_type](/kb/en/mysqltime_zone_transition_type-table/)) into the mysql database.

Most Linux, Mac OS X, FreeBSD and Solaris systems will have a zoneinfo database - Windows does not. The database is commonly found in the /usr/share/zoneinfo directory, or, on Solaris, the /usr/share/lib/zoneinfo directory.

## Usage

`mysql_tzinfo_to_sql` can be called in several ways. The output is usually passed straight to the [mysql client](/clients-utilities/mysql-client) for direct loading in the mysql database.

```sql
shell> mysql_tzinfo_to_sql timezone_dir
shell> mysql_tzinfo_to_sql timezone_file timezone_name
shell> mysql_tzinfo_to_sql --leap timezone_file
```

## Examples

Most commonly, the whole directory is passed:

```sql
shell>mysql_tzinfo_to_sql /usr/share/zoneinfo | mysql -u root mysql
```

Load a single time zone file, `timezone_file`, corresponding to the time zone called `timezone_name`.

```sql
shell> mysql_tzinfo_to_sql timezone_file timezone_name | mysql -u root mysql
```

A separate command for each time zone and time zone file the server needs is required.

To account for leap seconds, use:

```sql
shell> mysql_tzinfo_to_sql --leap timezone_file | mysql -u root mysql
```

After populating the time zone tables, you should usually restart the server so that the new time zone data is correctly loaded.