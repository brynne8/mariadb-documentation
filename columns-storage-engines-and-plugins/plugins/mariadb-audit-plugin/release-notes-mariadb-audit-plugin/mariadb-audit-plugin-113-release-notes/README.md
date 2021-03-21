# MariaDB Audit Plugin 1.1.3 Release Notes

<strong>Release date:</strong> 7 Nov 2013

This is a <strong><em>[Stable](/kb/en/release-criteria/) (GA)</em></strong> release. In general this means that there are no known serious bugs, except for those marked as feature requests, that no bugs were fixed since last release that caused a notable code changes, and that we believe the code is ready for general usage (based on bug inflow).

## Bug Fixes

- [MDEV-5243](https://jira.mariadb.org/browse/MDEV-5243) Version 1.1.2 for MySQL not providing facility correctly
- [MDEV-5145](https://jira.mariadb.org/browse/MDEV-5145) Windows build fixed.
- [MDEV-5145](https://jira.mariadb.org/browse/MDEV-5145) Client errors disabled for Percona/Mysql.
- [MDEV-4472](https://jira.mariadb.org/browse/MDEV-4472) Wrong filename doens't stop the logging.
- [MDEV-4472](https://jira.mariadb.org/browse/MDEV-4472) Error message fixed. Empty values for server_audit_file_path treated as default value.
- [MDEV-4472](https://jira.mariadb.org/browse/MDEV-4472) Now it's possible to specify a catalog as server_audit_file_path. In this case the log name will be [server_audit_file_path]/server_audit.log

## Added platform and database server support

- The MariaDB Audit Plugin is now supported on Windows 64-bit and Windows 32-bit.
- Now the Audit Plugin can be used with Percona Server + Galera, i.e. the Percona XtraDB Cluster.

## MariaDB Audit Plugin 1.1.3 Changelog

The revision number links will take you to the revision's page on Launchpad. On Launchpad you can view more details of the revision and view diffs of the code modified in that revision.

- Revision 3905, 2013-11-05
<ul start="1"><li>[MDEV-5243](https://jira.mariadb.org/browse/MDEV-5243) MariaDB Audit Plugin 1.1.2 for MySQL not providing facility correctly.
</li></ul>
- Revision 3904, 2013-11-03
<ul start="1"><li>[MDEV-5145](https://jira.mariadb.org/browse/MDEV-5145) Audit plugin crashes on PXC when selecting file as output type. Windows build fixed.
</li></ul>
- Revision 3903, 2013-10-28
<ul start="1"><li>[MDEV-5145](https://jira.mariadb.org/browse/MDEV-5145) Audit plugin crashes on PXC when selecting file as output type. Client errors disabled for Percona/Mysql.
</li></ul>
- Revision 3902, 2013-10-26
<ul start="1"><li>Merging into 5.5-noga-hf.
</li></ul>
- Revision 3901, 2013-09-27
<ul start="1"><li>[MDEV-4472](https://jira.mariadb.org/browse/MDEV-4472) Audit plugin. Fixed so the wrong filename doens't stop the logging.
</li></ul>
- Revision 3900, 2013-09-27
<ul start="1"><li>[MDEV-4472](https://jira.mariadb.org/browse/MDEV-4472) Audit plugin. Error message fixed. Empty values for server_audit_file_path treated as default value.
</li></ul>
- Revision 3899, 2013-09-27
<ul start="1"><li>[MDEV-4472](https://jira.mariadb.org/browse/MDEV-4472) Audit plugin. Now it's possible to specify a catalog as server_audit_file_path. In this case the log name will be [server_audit_file_path]/server_audit.log
</li></ul>