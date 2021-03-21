# Coordinated Universal Time

UTC stands for Coordinated Universal Time, and is the world standard for regulating time.

MariaDB stores values internally in UTC, converting them to the required time zone as required.

In general terms it is equivalent to Greenwich Mean Time (GMT), but UTC is used in technical contexts as it is precisely defined at the subsecond level.

[Time zones](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/internationalization-and-localization/time-zones) are offset relative to UTC. For example, time in Tonga is UTC + 13, so 03h00 UTC will be 16h00 in Tonga.

## See Also

- [UTC_DATE](/built-in-functions/date-time-functions/utc_date)
- [UTC_TIME](/built-in-functions/date-time-functions/utc_time)
- [UTC_TIMESTAMP](/built-in-functions/date-time-functions/utc_timestamp)