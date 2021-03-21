# Time Zones

MariaDB keeps track of several time zone settings.

## Setting the Time Zone

The <a undefined>time_zone</a> system variable is the primary way to set the time zone. It can be specified in one of the following formats:

- The default value is `SYSTEM`, which indicates that the system time zone defined in the <a undefined>system_time_zone</a> system variable will be used. See [System Time Zone](#system-time-zone) below for more information.
- An offset from [Coordinated Universal Time (UTC)](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/coordinated-universal-time), such as `+5:00` or `-9:00`, can also be used.
- If the time zone tables in the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables) database were loaded, then a named time zone, such as `America/New_York`, `Africa/Johannesburg`, or `Europe/Helsinki`, is also permissible. See [mysql Time Zone Tables](#mysql-time-zone-tables) below for more information.

There are two time zone settings that can be set within MariaDB--the global server time zone, and the time zone for your current session. There is also a third time zone setting which may be relevant--the system time zone.

### Global Server Time Zone

The global server time zone can be changed at server startup by setting the `--default-time-zone` option either on the command-line or in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariadb]
...
default_time_zone = 'America/New_York'
```

The global server time zone can also be changed dynamically by setting the <a undefined>time_zone</a> system variable as a user account that has the <a undefined>SUPER</a> privilege. For example:

```sql
SET GLOBAL time_zone = 'America/New_York';
```

The current global server time zone can be viewed by looking at the global value of the <a undefined>time_zone</a> system variable. For example:

```sql
SHOW GLOBAL VARIABLES LIKE 'time_zone';
+---------------+--------+
| Variable_name | Value  |
+---------------+--------+
| time_zone     | SYSTEM |
+---------------+--------+
```

### Session Time Zone

Each session that connects to the server will also have its own time zone. This time zone is initially inherited from the global value of the <a undefined>time_zone</a> system variable, which sets the session value of the same variable.

A session's time zone can be changed dynamically by setting the <a undefined>time_zone</a> system variable. For example:

```sql
SET time_zone = 'America/New_York';
```

The current session time zone can be viewed by looking at the session value of the <a undefined>time_zone</a> system variable. For example:

```sql
SHOW SESSION VARIABLES LIKE 'time_zone';
+---------------+--------+
| Variable_name | Value  |
+---------------+--------+
| time_zone     | SYSTEM |
+---------------+--------+
```

### System Time Zone

The system time zone is determined when the server starts, and it sets the value of the <a undefined>system_time_zone</a> system variable. The system time zone is usually read from the operating system's environment. You can change the system time zone in several different ways, such as:

- If you are starting the server with [mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe), then you can set the system time zone with the `--timezone` option either on the command-line or in the `[mysqld_safe]` [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mysqld_safe]
timezone='America/New_York'
```

- If you are using a Unix-like operating system, then you can set the system time zone by setting the `TZ` [environment variable](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-environment-variables) in your shell before starting the server. For example:

```sql
$ export TZ='America/New_York'
$ service mysql start
```

- On some Linux operating systems, you can change the default time zone for the whole system by making the <a undefined>/etc/localtime</a> symbolic link point to the desired time zone. For example:

```sql
$ sudo rm /etc/localtime
$ sudo ln -s /usr/share/zoneinfo/America/New_York /etc/localtime
```

- On some Debian-based Linux operating systems, you can change the default time zone for the whole system by executing the following:

```sql
sudo dpkg-reconfigure tzdata
```

- On Linux operating systems that use [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd), you can change the default time zone for the whole system by using the <a undefined>timedatectl</a> utility. For example:

```sql
sudo timedatectl set-timezone America/New_York
```

## Time Zone Effects

### Time Zone Effects on Functions

Some functions are affected by the time zone settings. These include:

- [NOW()](/built-in-functions/date-time-functions/now)
- [SYSDATE()](/built-in-functions/date-time-functions/sysdate)
- [CURDATE()](/built-in-functions/date-time-functions/curdate)
- [CURTIME()](/built-in-functions/date-time-functions/curtime)
- [UNIX_TIMESTAMP()](/built-in-functions/date-time-functions/unix_timestamp)

Some functions are not affected. These include:

- [UTC_DATE()](/built-in-functions/date-time-functions/utc_date)
- [UTC_TIME()](/built-in-functions/date-time-functions/utc_time)
- [UTC_TIMESTAMP()](/built-in-functions/date-time-functions/utc_timestamp)

### Time Zone Effects on Data Types

Some data types are affected by the time zone settings.

- [TIMESTAMP](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/timestamp) - See [TIMESTAMP: Time Zones](/kb/en/timestamp/#time-zones) for information on how this data type is effected by time zones.
- [DATETIME](/columns-storage-engines-and-plugins/data-types/date-and-time-data-types/datetime) - See [DATETIME: Time Zones](/kb/en/datetime/#time-zones) for information on how this data type is effected by time zones.

## mysql Time Zone Tables

The [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables) database contains a number of time zone tables:

- <a undefined>time_zone</a>
- <a undefined>time_zone_leap_second</a>
- <a undefined>time_zone_name</a>
- <a undefined>time_zone_transition</a>
- <a undefined>time_zone_transition_type</a>

By default, these time zone tables in the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables) database are created, but not populated.

If you are using a Unix-like operating system, then you can populate these tables using the [mysql_tzinfo_to_sql](/clients-utilities/mysql_tzinfo_to_sql) utility, which uses the <a undefined>zoneinfo</a> data available on Linux, Mac OS X, FreeBSD and Solaris.

If you are using Windows, then you will need to import pre-populated time zone tables. Some are available at [MySQL's documentation](http://dev.mysql.com/downloads/timezones.html).

Time zone data needs to be updated on occasion. When that happens, the time zone tables may need to be reloaded.

## See Also

- [LinuxJedi in Spacetime: Properly Handling Time and Date](https://www.youtube.com/watch?v=IV8q_mbZzEo) (video)