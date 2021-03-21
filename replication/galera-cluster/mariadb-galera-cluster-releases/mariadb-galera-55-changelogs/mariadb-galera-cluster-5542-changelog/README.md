# MariaDB Galera Cluster 5.5.42 Changelog

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.42)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5542-release-notes/)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5542-changelog/)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 11 Mar 2015

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5542-release-notes/).

The revision number links will take you to the revision's page on Github. On
Github you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #6c19f51](https://github.com/MariaDB/server/commit/6c19f51)
<span class="cstm-style datetime">2015-03-06 11:19:23 +0200</span>
<ul start="1"><li>[MDEV-7672](https://jira.mariadb.org/browse/MDEV-7672): Crash creating an InnoDB table with foreign keys
</li></ul>
- [Revision #e52a58a](https://github.com/MariaDB/server/commit/e52a58a)
<span class="cstm-style datetime">2015-03-04 19:49:17 -0500</span>
<ul start="1"><li>Update galera package name
</li></ul>
- [Revision #4fb2f66](https://github.com/MariaDB/server/commit/4fb2f66)
<span class="cstm-style datetime">2015-02-24 21:55:22 -0500</span>
<ul start="1"><li>Changes in wsrep_guess_ip()
</li></ul>
- [Revision #c6e62ac](https://github.com/MariaDB/server/commit/c6e62ac)
<span class="cstm-style datetime">2015-02-20 17:21:15 -0500</span>
<ul start="1"><li>Fix for build failures on Power8
</li></ul>
- [Revision #1e6f46d](https://github.com/MariaDB/server/commit/1e6f46d)
<span class="cstm-style datetime">2015-02-19 17:28:18 -0500</span>
<ul start="1"><li>cleanup: remove unused THD::COND_wsrep_thd
</li></ul>
- [Revision #fcc6e12](https://github.com/MariaDB/server/commit/fcc6e12)
<span class="cstm-style datetime">2015-02-18 19:02:00 -0500</span>
<ul start="1"><li>[MDEV-7544](https://jira.mariadb.org/browse/MDEV-7544): Update global_suppressions to include warning related to "gvwstate.dat"
</li></ul>
- [Revision #fdd6c11](https://github.com/MariaDB/server/commit/fdd6c11)
<span class="cstm-style datetime">2015-02-13 12:57:11 +0100</span>
<ul start="1"><li>[MDEV-7419](https://jira.mariadb.org/browse/MDEV-7419) Function cli_safe_read not exported
</li></ul>
- [Revision #13927f8](https://github.com/MariaDB/server/commit/13927f8)
<span class="cstm-style datetime">2015-02-11 18:32:40 +0100</span>
<ul start="1"><li>percona-server-5.5.41-37.0
</li></ul>
- [Revision #d996dc2](https://github.com/MariaDB/server/commit/d996dc2)
<span class="cstm-style datetime">2015-02-11 15:02:15 +0100</span>
<ul start="1"><li>[MDEV-7290](https://jira.mariadb.org/browse/MDEV-7290) please update MSI installer to include HeidiSQL 9.1
</li></ul>
- [Revision #63108dc](https://github.com/MariaDB/server/commit/63108dc)
<span class="cstm-style datetime">2015-02-10 12:26:21 +0100</span>
<ul start="1"><li>Fix the tree to work in git. Backport corresponing 10.1 changes.
</li></ul>
- [Revision #7588424](https://github.com/MariaDB/server/commit/7588424)
<span class="cstm-style datetime">2015-02-10 10:19:42 +0100</span>
<ul start="1"><li>restore a cross-compiling bit that was lost in a merge
</li></ul>
- [Revision #a34fd50](https://github.com/MariaDB/server/commit/a34fd50)
<span class="cstm-style datetime">2015-02-09 20:53:36 +0100</span>
<ul start="1"><li>[MDEV-7478](https://jira.mariadb.org/browse/MDEV-7478) log-basename unpredictable behavior in standalone mode
</li></ul>
- [Revision #f007f82](https://github.com/MariaDB/server/commit/f007f82)
<span class="cstm-style datetime">2015-02-09 20:53:28 +0100</span>
<ul start="1"><li>[MDEV-7351](https://jira.mariadb.org/browse/MDEV-7351) 5.5 build fails on Ubuntu Utopic in buildbot
</li></ul>
- [Revision #c233d6e](https://github.com/MariaDB/server/commit/c233d6e)
<span class="cstm-style datetime">2015-02-11 01:26:50 +0100</span>
<ul start="1"><li>[MDEV-7260](https://jira.mariadb.org/browse/MDEV-7260): Crash in get_best_combination when executing multi-table UPDATE with nested views
</li></ul>
- [Revision #cfb7d5d](https://github.com/MariaDB/server/commit/cfb7d5d)
<span class="cstm-style datetime">2015-02-10 16:16:31 +0400</span>
<ul start="1"><li>[MDEV-7516](https://jira.mariadb.org/browse/MDEV-7516) Assertion `!cur_p-&gt;event' failed in Gcalc_scan_iterator::arrange_event(int, int).     When the distance in ST_BUFFER is too far negative the coordinates can run out of the operational     area. We should just return an empty geometry in this case.
</li></ul>
- [Revision #552f1b3](https://github.com/MariaDB/server/commit/552f1b3)
<span class="cstm-style datetime">2015-02-10 14:17:23 +0200</span>
<ul start="1"><li>Fix test failures on innodb-[MDEV-7055](https://jira.mariadb.org/browse/MDEV-7055) and innodb-[MDEV-7513](https://jira.mariadb.org/browse/MDEV-7513).
</li></ul>
- [Revision #ada0743](https://github.com/MariaDB/server/commit/ada0743)
<span class="cstm-style datetime">2015-02-10 08:08:59 +0200</span>
<ul start="1"><li>Fix test failure on innodb-[MDEV-7055](https://jira.mariadb.org/browse/MDEV-7055).
</li></ul>
- [Revision #44a9e3f](https://github.com/MariaDB/server/commit/44a9e3f)
<span class="cstm-style datetime">2015-02-09 16:14:27 +0200</span>
<ul start="1"><li>[MDEV-7139](https://jira.mariadb.org/browse/MDEV-7139): Sporadic failure in innodb.innodb_corrupt_bit on P8
</li></ul>
- [Revision #919f40e](https://github.com/MariaDB/server/commit/919f40e)
<span class="cstm-style datetime">2015-02-08 22:38:19 +0400</span>
<ul start="1"><li>Audit plugin v1.2.0.
</li></ul>
- [Revision #0d73bc1](https://github.com/MariaDB/server/commit/0d73bc1)
<span class="cstm-style datetime">2015-02-08 15:47:00 +0300</span>
<ul start="1"><li>[MDEV-7519](https://jira.mariadb.org/browse/MDEV-7519) debian / ubuntu packaging creation of plugin table (if not exists)
</li></ul>
- [Revision #b9d616c](https://github.com/MariaDB/server/commit/b9d616c)
<span class="cstm-style datetime">2015-02-06 15:49:45 +0400</span>
<ul start="1"><li>[MDEV-7435](https://jira.mariadb.org/browse/MDEV-7435) Windows debug: Run-Time Check Failure #3 - The variable 'unused' is being used without being initialized. Fixed as it's done in 10.0.
</li></ul>
- [Revision #5c6eb52](https://github.com/MariaDB/server/commit/5c6eb52)
<span class="cstm-style datetime">2015-02-04 16:50:29 +0200</span>
<ul start="1"><li>Fix test failure.
</li></ul>
- [Revision #8cc9751](https://github.com/MariaDB/server/commit/8cc9751)
<span class="cstm-style datetime">2015-02-04 14:40:46 +0200</span>
<ul start="1"><li>[MDEV-7538](https://jira.mariadb.org/browse/MDEV-7538): Wrong constraint (TINYINT or MEDIUMINT and INT) causes server crash
</li></ul>
- [Revision #422ffe9](https://github.com/MariaDB/server/commit/422ffe9)
<span class="cstm-style datetime">2015-02-04 11:12:46 +0200</span>
<ul start="1"><li>InnoDB and XtraDB produce different output on [MDEV-7513](https://jira.mariadb.org/browse/MDEV-7513).
</li></ul>
- [Revision #f320915](https://github.com/MariaDB/server/commit/f320915)
<span class="cstm-style datetime">2015-02-04 10:50:16 +0200</span>
<ul start="1"><li>[MDEV-7055](https://jira.mariadb.org/browse/MDEV-7055): MySQL#74664 - InnoDB: Failing assertion: len &lt;= col-&gt;len || col-&gt;mtype == 5 || (col-&gt;len == 0 &amp;&amp; col-&gt;mtype == 1) in file rem0rec.cc line 845
</li></ul>
- [Revision #7afbf33](https://github.com/MariaDB/server/commit/7afbf33)
<span class="cstm-style datetime">2015-02-04 09:29:54 +0200</span>
<ul start="1"><li>[MDEV-7513](https://jira.mariadb.org/browse/MDEV-7513): ib_warn_row_too_big dereferences null thd
</li></ul>
- [Revision #5f63c9c](https://github.com/MariaDB/server/commit/5f63c9c)
<span class="cstm-style datetime">2015-01-29 14:34:31 +0100</span>
<ul start="1"><li>recreate expired certificates for SSL tests
</li></ul>
- [Revision #9033aa0](https://github.com/MariaDB/server/commit/9033aa0)
<span class="cstm-style datetime">2015-01-28 11:49:55 +0100</span>
<ul start="1"><li>[MDEV-6128](https://jira.mariadb.org/browse/MDEV-6128):[PATCH] mysqlcheck wrongly escapes '.' in table names
</li></ul>
- [Revision #d8ee54c](https://github.com/MariaDB/server/commit/d8ee54c)
<span class="cstm-style datetime">2015-01-27 20:02:43 -0500</span>
<ul start="1"><li>Add cmake check for getifaddrs.
</li></ul>
- [Revision #9b7e380](https://github.com/MariaDB/server/commit/9b7e380)
<span class="cstm-style datetime">2015-01-27 16:22:29 -0500</span>
<ul start="1"><li>[MDEV-7476](https://jira.mariadb.org/browse/MDEV-7476): Allow SELECT to succeed even when node is not ready
</li></ul>
- [Revision #5b6f637](https://github.com/MariaDB/server/commit/5b6f637)
<span class="cstm-style datetime">2015-01-27 14:17:40 -0500</span>
<ul start="1"><li>[MDEV-7322](https://jira.mariadb.org/browse/MDEV-7322): Option to allow setting the binlog_format with Galera
</li></ul>
- [Revision #248c662](https://github.com/MariaDB/server/commit/248c662)
<span class="cstm-style datetime">2015-01-26 22:43:46 -0500</span>
<ul start="1"><li>Minor test modifications.
</li></ul>
- [Revision #f9e7f82](https://github.com/MariaDB/server/commit/f9e7f82)
<span class="cstm-style datetime">2015-01-26 11:44:39 -0500</span>
<ul start="1"><li>Backported changes done in wsrep_guess_ip() from 10.1.
</li></ul>
- [Revision #fffc9f5](https://github.com/MariaDB/server/commit/fffc9f5)
<span class="cstm-style datetime">2015-01-24 18:39:21 -0500</span>
<ul start="1"><li>[MDEV-7374](https://jira.mariadb.org/browse/MDEV-7374) : Losing connection to MySQL while running ALTER TABLE
</li></ul>
- [Revision #cb9c116](https://github.com/MariaDB/server/commit/cb9c116)
<span class="cstm-style datetime">2015-01-23 09:13:21 +0100</span>
<ul start="1"><li>update tokudb version after merge
</li></ul>
- [Revision #8bc712e](https://github.com/MariaDB/server/commit/8bc712e)
<span class="cstm-style datetime">2015-01-19 17:31:59 +0100</span>
<ul start="1"><li>[MDEV-6671](https://jira.mariadb.org/browse/MDEV-6671) mysql_server_end breaks OpenSSL
</li></ul>
- [Revision #3212aaa](https://github.com/MariaDB/server/commit/3212aaa)
<span class="cstm-style datetime">2015-01-19 17:18:24 +0100</span>
<ul start="1"><li>[MDEV-6220](https://jira.mariadb.org/browse/MDEV-6220) mysqldump will not backup database with --flush-logs parameter and log_error my.cnf parameter defined
</li></ul>
- [Revision #a18eb83](https://github.com/MariaDB/server/commit/a18eb83)
<span class="cstm-style datetime">2015-01-19 16:41:37 +0100</span>
<ul start="1"><li>[MDEV-7226](https://jira.mariadb.org/browse/MDEV-7226) sql-bench test-table-elimination does not execute
</li></ul>
- [Revision #595cf63](https://github.com/MariaDB/server/commit/595cf63)
<span class="cstm-style datetime">2015-01-19 16:29:18 +0100</span>
<ul start="1"><li>[MDEV-7475](https://jira.mariadb.org/browse/MDEV-7475) Wrong implementation of checking PLUGIN_VAR_SET condition
</li></ul>
- [Revision #5d0d6cb](https://github.com/MariaDB/server/commit/5d0d6cb)
<span class="cstm-style datetime">2015-01-19 16:28:58 +0100</span>
<ul start="1"><li>[MDEV-7294](https://jira.mariadb.org/browse/MDEV-7294) MTR does not use /dev/shm with a out-of-source build
</li></ul>
- [Revision #bb93d46](https://github.com/MariaDB/server/commit/bb93d46)
<span class="cstm-style datetime">2015-01-16 13:52:30 -0500</span>
<ul start="1"><li>Test changes (backported from 10.1).
</li></ul>
- [Revision #3f118a7](https://github.com/MariaDB/server/commit/3f118a7)
<span class="cstm-style datetime">2015-01-16 18:13:02 +0100</span>
<ul start="1"><li>[MDEV-6347](https://jira.mariadb.org/browse/MDEV-6347) Build RHEL7 packages
</li></ul>
- [Revision #2fc0b22](https://github.com/MariaDB/server/commit/2fc0b22)
<span class="cstm-style datetime">2015-01-16 17:54:00 +0100</span>
<ul start="1"><li>restore an incorrectly merged line
</li></ul>
- [Revision #ca6b86f](https://github.com/MariaDB/server/commit/ca6b86f)
<span class="cstm-style datetime">2015-01-14 17:50:38 +0400</span>
<ul start="1"><li>[MDEV-7448](https://jira.mariadb.org/browse/MDEV-7448) - mtr may leave stale mysqld
</li></ul>
- [Revision #d9d9940](https://github.com/MariaDB/server/commit/d9d9940)
<span class="cstm-style datetime">2015-01-14 18:24:23 -0500</span>
<ul start="1"><li>[MDEV-7368](https://jira.mariadb.org/browse/MDEV-7368) : SLES: Failed to start mysql.service: Unit             mysql.service failed to load
</li></ul>
- [Revision #5900333](https://github.com/MariaDB/server/commit/5900333)
<span class="cstm-style datetime">2015-01-14 12:10:13 +0100</span>
<ul start="1"><li>[MDEV-7404](https://jira.mariadb.org/browse/MDEV-7404) REPAIR multiple tables crash in MDL_ticket::has_stronger_or_equal_type
</li></ul>
- [Revision #e53b41a](https://github.com/MariaDB/server/commit/e53b41a)
<span class="cstm-style datetime">2015-01-13 19:28:03 +0100</span>
<ul start="1"><li>cleanup
</li></ul>
- [Revision #7f9f313](https://github.com/MariaDB/server/commit/7f9f313)
<span class="cstm-style datetime">2015-01-13 19:27:28 +0100</span>
<ul start="1"><li>[MDEV-7333](https://jira.mariadb.org/browse/MDEV-7333) "'show table status like 'table_name'" on tokudb table lead to MariaDB crash
</li></ul>
- [Revision #33b4fab](https://github.com/MariaDB/server/commit/33b4fab)
<span class="cstm-style datetime">2015-01-13 13:10:07 -0500</span>
<ul start="1"><li>[MDEV-6771](https://jira.mariadb.org/browse/MDEV-6771) : Incorrect Size for Transfer Reported to pv
</li></ul>
- [Revision #2ab4968](https://github.com/MariaDB/server/commit/2ab4968)
<span class="cstm-style datetime">2015-01-10 14:07:46 +0100</span>
<ul start="1"><li>[MDEV-7410](https://jira.mariadb.org/browse/MDEV-7410) Temporary table name conflict between sessions
</li></ul>
- [Revision #743a28e](https://github.com/MariaDB/server/commit/743a28e)
<span class="cstm-style datetime">2015-01-07 17:22:53 -0500</span>
<ul start="1"><li>[MDEV-7129](https://jira.mariadb.org/browse/MDEV-7129) : Galera duplicate error on autoincrement field primary key
</li></ul>
- [Revision #0064952](https://github.com/MariaDB/server/commit/0064952)
<span class="cstm-style datetime">2015-01-06 16:32:41 +0100</span>
<ul start="1"><li>[MDEV-7189](https://jira.mariadb.org/browse/MDEV-7189): main.processlist fails sporadically in buildbot
</li></ul>
- [Revision #068416d](https://github.com/MariaDB/server/commit/068416d)
<span class="cstm-style datetime">2015-01-02 09:50:51 -0500</span>
<ul start="1"><li>DB-785 add a txn api to check if a txn is prepared
</li></ul>
- [Revision #5fafc3c](https://github.com/MariaDB/server/commit/5fafc3c)
<span class="cstm-style datetime">2014-12-28 13:24:53 +0200</span>
<ul start="1"><li>[MDEV-7369](https://jira.mariadb.org/browse/MDEV-7369): MariaDB build fails when XTRADB_STORAGE_ENGINE enabled
</li></ul>
- [Revision #8051205](https://github.com/MariaDB/server/commit/8051205)
<span class="cstm-style datetime">2014-12-23 21:21:23 +0400</span>
<ul start="1"><li>Increased the version number
</li></ul>
- [Revision #3818bbb](https://github.com/MariaDB/server/commit/3818bbb)
<span class="cstm-style datetime">2014-12-21 19:23:28 +0100</span>
<ul start="1"><li>Adding mariadb-version on the view creation to view frm. ([MDEV-6916](https://jira.mariadb.org/browse/MDEV-6916) followup)
</li></ul>
- [Revision #260727a](https://github.com/MariaDB/server/commit/260727a)
<span class="cstm-style datetime">2014-12-19 23:42:22 +0400</span>
<ul start="1"><li>Fixed yet another compiler warning.
</li></ul>
- [Revision #094640c](https://github.com/MariaDB/server/commit/094640c)
<span class="cstm-style datetime">2014-12-19 23:17:59 +0400</span>
<ul start="1"><li>Fixed a couple of compiler warnings.
</li></ul>