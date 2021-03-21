# Temporal Tables

MariaDB supports temporal data tables in the form of system-versioning tables (allowing you to query and operate on historic data), application-time periods (allow you to query and operate on a temporal range of data), and bitemporal tables (which combine both system-versioning and application-time periods).

- [System-Versioned Tables](/sql-statements-structure/temporal-tables/system-versioned-tables/) — System-versioned tables record the history of all changes to table data.
- [Application-Time Periods](/sql-statements-structure/temporal-tables/application-time-periods/) — Application-time period tables, defined by a range between two temporal columns.
- [Bitemporal Tables](/sql-statements-structure/temporal-tables/bitemporal-tables/) — Bitemporal tables use versioning both at the system and application-time period levels.