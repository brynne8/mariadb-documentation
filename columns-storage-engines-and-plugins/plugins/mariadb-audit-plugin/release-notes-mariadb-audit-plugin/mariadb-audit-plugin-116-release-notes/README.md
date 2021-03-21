# MariaDB Audit Plugin 1.1.6 Release Notes

<strong>Release date:</strong> 27 Mar 2014

This is a <strong><em>[Stable](/kb/en/release-criteria/) (GA)</em></strong> release. In general this means that there are no known serious bugs, except for those marked as feature requests, that no bugs were fixed since last release that caused a notable code changes, and that we believe the code is ready for general usage (based on bug inflow).

## Important Notices

- MariaDB will include the [Audit Plugin](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/) by default from versions 5.5.37 and 10.0.10. At the time of writing these versions haven't yet been released.
- The MariaDB Audit Plugin works for MariaDB, MySQL and Percona Server.

## Download

If you want to download the MariaDB Audit Plugin separately from the MariaDB server, it is available at [SkySQL's site](http://www.skysql.com/downloads/mariadb-audit-plugin).

## Bug Fixes

- [MDEV-5681](https://jira.mariadb.org/browse/MDEV-5681) audit log will not rotate when the file size exceeds global variable setting. The variables [server_audit_file_rotate_now](/kb/en/server_audit-system-variables/#server_audit_file_rotate_now) and [server_audit_file_rotations](/kb/en/server_audit-system-variables/#server_audit_file_rotations) are now handled and one doesn't need to stop/start logging to make them effective.
- [MDEV-5862](https://jira.mariadb.org/browse/MDEV-5862) server_audit test fails in buildbot on Mac (labrador). The RTLD_DEFAULT value on Labrador machine is not NULL, so the dlsym() commands in the server_audit just fail to bind the necessary functions. Fixed by using RTLD_DEFAULT explicitly.