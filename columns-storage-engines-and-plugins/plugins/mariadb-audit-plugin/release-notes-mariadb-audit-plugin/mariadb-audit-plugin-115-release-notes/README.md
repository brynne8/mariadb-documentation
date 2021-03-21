# MariaDB Audit Plugin 1.1.5 Release Notes

<strong>Release date:</strong> 25 Feb 2014

This is a <strong><em>[Stable](/kb/en/release-criteria/) (GA)</em></strong> release. In general this means that there are no known serious bugs, except for those marked as feature requests, that no bugs were fixed since last release that caused a notable code changes, and that we believe the code is ready for general usage (based on bug inflow).

## Important Notices

- MariaDB will include the [Audit Plugin](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin) by default from versions 5.5.37 and 10.0.10. At the time of writing these versions haven't yet been released.
- The MariaDB Audit Plugin works for MariaDB, MySQL and Percona Server.

## Download

If you want to download the MariaDB Audit Plugin separately from the MariaDB server, it is available at [SkySQL's site](http://www.skysql.com/downloads/mariadb-audit-plugin).

## Main Improvements

- This version includes only one bug fix, but an important one without which crashes can occur with version 1.1.4 of the Audit Plugin. See below for more details and look at [mariadb-audit-plugin-114-release-notes](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/release-notes-mariadb-audit-plugin/mariadb-audit-plugin-114-release-notes) for other recent changes in the Audit Plugin.

## Bug Fixes

- [MDEV-5722](https://jira.mariadb.org/browse/MDEV-5722) MariaDB audit plugin crashes when MariaDB Audit Plugin variables are set in my.cnf.

## MariaDB Audit Plugin 1.1.5 Changelog

The issue number links will take you to the corresponding issue in MariaDB project tracking.

- Revision 3914, 2014-02-24
<ul start="1"><li>[MDEV-5722](https://jira.mariadb.org/browse/MDEV-5722) MariaDB audit plugin crashes when variables set in .my.cnf. The 'thd' parameter is NULL when variables are changed in server_audit_init(). That we should take into account.</li></ul>