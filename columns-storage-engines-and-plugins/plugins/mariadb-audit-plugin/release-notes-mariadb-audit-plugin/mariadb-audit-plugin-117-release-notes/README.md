# MariaDB Audit Plugin 1.1.7 Release Notes

<strong>Release date:</strong> 1 May 2014

This is a <strong><em>[Stable](/kb/en/release-criteria/) (GA)</em></strong> release. In general this means that there are no known serious bugs, except for those marked as feature requests, that no bugs were fixed since last release that caused a notable code changes, and that we believe the code is ready for general usage (based on bug inflow).

## Important Notices

- The [Audit Plugin](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/) is included by default from versions 5.5.37 and 10.0.10.
- The MariaDB Audit Plugin works for MariaDB, MySQL and Percona Server. For MySQL and Percona Server it has to be downloaded separately from [SkySQL's site](http://www.skysql.com/downloads/mariadb-audit-plugin)

## Download

If you want to download the MariaDB Audit Plugin separately from the MariaDB server, it is available at [SkySQL's site](http://www.skysql.com/downloads/mariadb-audit-plugin).

## Bug Fixes

- [MDEV-6124](https://jira.mariadb.org/browse/MDEV-6124) Audit plugin fails with the Percona-Server 5.6.