# MariaDB Galera Cluster 5.5.43 Changelog

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.43)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5543-release-notes/)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5543-changelog/)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 15 May 2015

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5543-release-notes/).

The revision number links will take you to the revision's page on Github. On
Github you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #014fe12](https://github.com/MariaDB/server/commit/014fe12)
<span class="cstm-style datetime">2015-05-12 14:19:30 -0400</span>
<ul start="1"><li>Fix for debug build failure
</li></ul>
- [Revision #18fee8b](https://github.com/MariaDB/server/commit/18fee8b)
<span class="cstm-style datetime">2015-05-12 17:05:41 -0400</span>
<ul start="1"><li>Revert last commit.
</li></ul>
- [Revision #048039e](https://github.com/MariaDB/server/commit/048039e)
<span class="cstm-style datetime">2015-05-11 15:42:20 -0400</span>
<ul start="1"><li>Fix for build failure.
</li></ul>
- [Revision #e1868fd](https://github.com/MariaDB/server/commit/e1868fd)
<span class="cstm-style datetime">2015-05-07 22:18:34 +0200</span>
<ul start="1"><li>[MDEV-8115](https://jira.mariadb.org/browse/MDEV-8115) mysql_upgrade crashes the server with REPAIR VIEW
</li></ul>
- [Revision #d256200](https://github.com/MariaDB/server/commit/d256200)
<span class="cstm-style datetime">2015-05-04 13:50:52 -0400</span>
<ul start="1"><li>Merge tag 'mariadb-5.5.43' into 5.5-galera
</li></ul>
- [Revision #42f99d0](https://github.com/MariaDB/server/commit/42f99d0)
<span class="cstm-style datetime">2015-03-18 21:17:31 +0200</span>
<ul start="1"><li>codership/mysql-wsrep#67 - total order isolation for FLUSH
</li></ul>
- [Revision #f3efc63](https://github.com/MariaDB/server/commit/f3efc63)
<span class="cstm-style datetime">2015-02-10 18:27:21 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#60 explicit braces around empty body
</li></ul>
- [Revision #70d6236](https://github.com/MariaDB/server/commit/70d6236)
<span class="cstm-style datetime">2015-02-10 00:47:02 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#55 - To avoid compiler warming:
</li></ul>
- [Revision #f6b1e0f](https://github.com/MariaDB/server/commit/f6b1e0f)
<span class="cstm-style datetime">2015-02-05 14:38:03 +0200</span>
<ul start="1"><li>refs #55 fixed debug build compilation errors
</li></ul>
- [Revision #b02d736](https://github.com/MariaDB/server/commit/b02d736)
<span class="cstm-style datetime">2014-12-10 23:29:28 +0200</span>
<ul start="1"><li>Refs #25 - made sure signals that may be set to ignored in mysqld were set to default in the child process.
</li></ul>
- [Revision #c72ed05](https://github.com/MariaDB/server/commit/c72ed05)
<span class="cstm-style datetime">2014-12-10 01:45:50 +0200</span>
<ul start="1"><li>This commit
<ul start="1"><li>improves MySQL client version check making it no less than required as opposed to exactly as required
</li><li>adds event table copying to ensure same results as with rsync SST.
</li></ul>
</li></ul>
- [Revision #822c005](https://github.com/MariaDB/server/commit/822c005)
<span class="cstm-style datetime">2014-12-09 22:13:14 +0200</span>
<ul start="1"><li>Going more MTR-friendly - for SST prefer to use mysql client, mysqldump and my_print_defaults from the local build tree.
</li></ul>
- [Revision #2a6e123](https://github.com/MariaDB/server/commit/2a6e123)
<span class="cstm-style datetime">2015-03-30 17:58:41 -0400</span>
<ul start="1"><li>refs #7 - handling lock queue granting in BF-BF conflict situation
</li></ul>
- [Revision #68fe3a3](https://github.com/MariaDB/server/commit/68fe3a3)
<span class="cstm-style datetime">2014-11-16 16:26:36 +0200</span>
<ul start="1"><li>refs #7 - handling lock queue granting in BF-BF conflict situation
</li></ul>
- [Revision #51744b3](https://github.com/MariaDB/server/commit/51744b3)
<span class="cstm-style datetime">2014-10-31 15:55:18 +0800</span>
<ul start="1"><li>Refs #8: [5.5] preserve gvwstate.dat for pc recovery feature
</li></ul>
- [Revision #4c87f72](https://github.com/MariaDB/server/commit/4c87f72)
<span class="cstm-style datetime">2015-04-29 16:24:52 +0200</span>
<ul start="1"><li>Merge branch '5.5' into bb-5.5-serg
</li></ul>
- [Revision #a4477d2](https://github.com/MariaDB/server/commit/a4477d2)
<span class="cstm-style datetime">2015-04-29 14:14:45 +0300</span>
<ul start="1"><li>Fix failing test cases for [MDEV-7912](https://jira.mariadb.org/browse/MDEV-7912) patch
</li></ul>
- [Revision #f632b51](https://github.com/MariaDB/server/commit/f632b51)
<span class="cstm-style datetime">2015-04-28 21:27:43 +0200</span>
<ul start="1"><li>[MDEV-7987](https://jira.mariadb.org/browse/MDEV-7987) Fatal error: Please read "Security" section of the manual to find out how to run mysqld as root!
</li></ul>
- [Revision #6f17e23](https://github.com/MariaDB/server/commit/6f17e23)
<span class="cstm-style datetime">2015-04-28 21:24:32 +0200</span>
<ul start="1"><li>post-merge fixes
</li></ul>
- [Revision #f9c02d7](https://github.com/MariaDB/server/commit/f9c02d7)
<span class="cstm-style datetime">2015-04-28 21:11:49 +0200</span>
<ul start="1"><li>Merge branch 'openquery/[MDEV-6916](https://jira.mariadb.org/browse/MDEV-6916)-maria-5.5-check_view-r4408' into 5.5
</li></ul>
- [Revision #fbab068](https://github.com/MariaDB/server/commit/fbab068)
<span class="cstm-style datetime">2015-04-28 13:57:21 +0200</span>
<ul start="1"><li>post-merge changes, fixes, and tests
</li></ul>
- [Revision #67a3ddf](https://github.com/MariaDB/server/commit/67a3ddf)
<span class="cstm-style datetime">2015-04-28 13:54:37 +0200</span>
<ul start="1"><li>Merge branch 'merge-xtradb-5.5' into 5.5
</li></ul>
- [Revision #40e9560](https://github.com/MariaDB/server/commit/40e9560)
<span class="cstm-style datetime">2015-04-28 13:42:58 +0200</span>
<ul start="1"><li>percona-server-5.5.42-37.1.tar.gz
</li></ul>
- [Revision #c581ae0](https://github.com/MariaDB/server/commit/c581ae0)
<span class="cstm-style datetime">2015-04-28 13:37:54 +0200</span>
<ul start="1"><li>Null-merge branch 'merge-xtradb-5.5' into 5.5
</li></ul>
- [Revision #a5fa434](https://github.com/MariaDB/server/commit/a5fa434)
<span class="cstm-style datetime">2015-04-28 15:31:49 +0500</span>
<ul start="1"><li>[MDEV-7779](https://jira.mariadb.org/browse/MDEV-7779) View definition changes upon creation.
<ul start="1"><li>Fixed by using POINT instead of ST_POINT in the item.
</li><li>Later need to fix that with proper ST_POINT implementation
</li></ul>
</li></ul>
- [Revision #4c174fc](https://github.com/MariaDB/server/commit/4c174fc)
<span class="cstm-style datetime">2015-04-28 15:28:29 +0300</span>
<ul start="1"><li>[MDEV-8020](https://jira.mariadb.org/browse/MDEV-8020): innodb.innodb-[MDEV-7055](https://jira.mariadb.org/browse/MDEV-7055) produces valgrind warnings in buildbot
</li></ul>
- [Revision #ac2b92c](https://github.com/MariaDB/server/commit/ac2b92c)
<span class="cstm-style datetime">2015-04-28 15:09:04 +0300</span>
<ul start="1"><li>[MDEV-7912](https://jira.mariadb.org/browse/MDEV-7912) multitable delete with wrongly set sort_buffer_size crashes in merge_buffers
</li></ul>
- [Revision #fd39c56](https://github.com/MariaDB/server/commit/fd39c56)
<span class="cstm-style datetime">2015-04-27 23:37:51 +0200</span>
<ul start="1"><li>move to storage/xtradb/
</li></ul>
- [Revision #0f12ada](https://github.com/MariaDB/server/commit/0f12ada)
<span class="cstm-style datetime">2015-04-27 21:04:06 +0200</span>
<ul start="1"><li>Merge remote-tracking branch 'mysql/5.5' into 5.5
</li></ul>
- [Revision #e4df6e5](https://github.com/MariaDB/server/commit/e4df6e5)
<span class="cstm-style datetime">2015-04-27 16:19:54 +0200</span>
<ul start="1"><li>Merge commit 'tokudb-engine/tokudb-7.5.6' into 5.5
</li></ul>
- [Revision #2f446f2](https://github.com/MariaDB/server/commit/2f446f2)
<span class="cstm-style datetime">2015-04-27 16:04:39 +0200</span>
<ul start="1"><li>Merge commit 'tokudb-ft-index/tokudb-7.5.6' into 5.5
</li></ul>
- [Revision #939a233](https://github.com/MariaDB/server/commit/939a233)
<span class="cstm-style datetime">2015-04-27 15:56:39 +0200</span>
<ul start="1"><li>Merge remote-tracking branch 'openquery/[MDEV-8060](https://jira.mariadb.org/browse/MDEV-8060)-shm-path' into 5.5
</li></ul>
- [Revision #245cc73](https://github.com/MariaDB/server/commit/245cc73)
<span class="cstm-style datetime">2015-04-27 12:47:39 +0200</span>
<ul start="1"><li>[MDEV-7434](https://jira.mariadb.org/browse/MDEV-7434) XtraDB does not build on Solaris
</li></ul>
- [Revision #e26b207](https://github.com/MariaDB/server/commit/e26b207)
<span class="cstm-style datetime">2015-04-26 16:27:41 +0200</span>
<ul start="1"><li>[MDEV-7938](https://jira.mariadb.org/browse/MDEV-7938) MariaDB Crashes Suddenly while writing binlogs
</li></ul>
- [Revision #053143e](https://github.com/MariaDB/server/commit/053143e)
<span class="cstm-style datetime">2015-04-25 21:56:46 +0200</span>
<ul start="1"><li>[MDEV-7883](https://jira.mariadb.org/browse/MDEV-7883) Segmentation failure when running mysqladmin -u root -p
</li></ul>
- [Revision #18215dd](https://github.com/MariaDB/server/commit/18215dd)
<span class="cstm-style datetime">2015-04-25 17:22:46 +0200</span>
<ul start="1"><li>[MDEV-7859](https://jira.mariadb.org/browse/MDEV-7859) SSL hostname verification fails for long subject names
</li></ul>
- [Revision #9fd65db](https://github.com/MariaDB/server/commit/9fd65db)
<span class="cstm-style datetime">2015-04-25 00:19:20 +0200</span>
<ul start="1"><li>[MDEV-7585](https://jira.mariadb.org/browse/MDEV-7585) Assertion `thd-&gt;is_error() || kill_errno || thd-&gt;killed == ABORT_QUERY' failed in ha_rows filesort
</li></ul>
- [Revision #8e78160](https://github.com/MariaDB/server/commit/8e78160)
<span class="cstm-style datetime">2015-04-24 21:41:00 +0200</span>
<ul start="1"><li>[MDEV-6870](https://jira.mariadb.org/browse/MDEV-6870) Not possible to use FIFO file as a general_log file
</li></ul>
- [Revision #c05d431](https://github.com/MariaDB/server/commit/c05d431)
<span class="cstm-style datetime">2015-04-24 21:03:43 +0200</span>
<ul start="1"><li>bug: crash when sync() or close() of a log file fails on shutdown
</li></ul>
- [Revision #8f499c3](https://github.com/MariaDB/server/commit/8f499c3)
<span class="cstm-style datetime">2015-04-24 21:02:37 +0200</span>
<ul start="1"><li>bug: debug assert crash when seek on log file fails
</li></ul>
- [Revision #5fd0088](https://github.com/MariaDB/server/commit/5fd0088)
<span class="cstm-style datetime">2015-04-27 15:31:12 +0200</span>
<ul start="1"><li>[MDEV-8058](https://jira.mariadb.org/browse/MDEV-8058): funcs_1.innodb_views and funcs_1.memory_views fail
</li></ul>
- [Revision #574227c](https://github.com/MariaDB/server/commit/574227c)
<span class="cstm-style datetime">2015-04-27 21:15:23 +1000</span>
<ul start="1"><li>/run/shm is the general replacement for /dev/shm in newer distros
</li></ul>
- [Revision #f832021](https://github.com/MariaDB/server/commit/f832021)
<span class="cstm-style datetime">2015-04-23 08:26:57 +0200</span>
<ul start="1"><li>[MDEV-7126](https://jira.mariadb.org/browse/MDEV-7126) replication slave - deadlock in terminate_slave_thread with stop slave and show variables of replication filters and show global status
</li></ul>
- [Revision #2d6c0a5](https://github.com/MariaDB/server/commit/2d6c0a5)
<span class="cstm-style datetime">2015-04-24 13:44:22 +0200</span>
<ul start="1"><li>Merge pull request #39 from openquery/[MDEV-7977](https://jira.mariadb.org/browse/MDEV-7977)-mutex-unlock_LOCK_log-in-MYSQL_BIN_LOG_write_incident
</li></ul>
- [Revision #44d1e85](https://github.com/MariaDB/server/commit/44d1e85)
<span class="cstm-style datetime">2015-04-24 11:00:34 +0400</span>
<ul start="1"><li>[MDEV-7649](https://jira.mariadb.org/browse/MDEV-7649) wrong result when comparing utf8 column with an invalid literal
</li></ul>
- [Revision #f9b2704](https://github.com/MariaDB/server/commit/f9b2704)
<span class="cstm-style datetime">2015-04-23 23:06:14 +0300</span>
<ul start="1"><li>Testcase for: [MDEV-7893](https://jira.mariadb.org/browse/MDEV-7893) table_elimination works wrong ...
</li></ul>
- [Revision #2010971](https://github.com/MariaDB/server/commit/2010971)
<span class="cstm-style datetime">2015-04-14 23:18:54 +0200</span>
<ul start="1"><li>[MDEV-6892](https://jira.mariadb.org/browse/MDEV-6892): WHERE does not apply
</li></ul>
- [Revision #581b49d](https://github.com/MariaDB/server/commit/581b49d)
<span class="cstm-style datetime">2015-04-22 18:13:30 -0400</span>
<ul start="1"><li>[MDEV-7995](https://jira.mariadb.org/browse/MDEV-7995) : DMLs not getting replicated with log-bin=OFF &amp; binlog-format != ROW
</li></ul>
- [Revision #8cbaafd](https://github.com/MariaDB/server/commit/8cbaafd)
<span class="cstm-style datetime">2015-04-22 10:14:11 +0200</span>
<ul start="1"><li>[MDEV-8018](https://jira.mariadb.org/browse/MDEV-8018): main.multi_update fails with --ps-protocol
</li></ul>
- [Revision #e428c80](https://github.com/MariaDB/server/commit/e428c80)
<span class="cstm-style datetime">2015-04-21 15:41:01 +0300</span>
<ul start="1"><li>[MDEV-7911](https://jira.mariadb.org/browse/MDEV-7911): crash in Item_cond::eval_not_null_tables
</li></ul>
- [Revision #f1f8adf](https://github.com/MariaDB/server/commit/f1f8adf)
<span class="cstm-style datetime">2015-04-20 05:02:10 +0200</span>
<ul start="1"><li>tokuftdump: Install to ${INSTALL_BINDIR} instead of bin
</li></ul>
- [Revision #4cfb7f9](https://github.com/MariaDB/server/commit/4cfb7f9)
<span class="cstm-style datetime">2015-04-19 15:49:35 +0300</span>
<ul start="1"><li>Increase the version number
</li></ul>
- [Revision #1115a59](https://github.com/MariaDB/server/commit/1115a59)
<span class="cstm-style datetime">2015-04-15 19:14:20 +0300</span>
<ul start="1"><li>Merge pull request #41 from MariaDB/5.5-[MDEV-7820](https://jira.mariadb.org/browse/MDEV-7820)
</li></ul>
- [Revision #eb47b22](https://github.com/MariaDB/server/commit/eb47b22)
<span class="cstm-style datetime">2015-04-15 16:23:43 +0300</span>
<ul start="1"><li>[MDEV-7820](https://jira.mariadb.org/browse/MDEV-7820) Server crashes in in my_strcasecmp_utf8 on subquery in ORDER BY clause of GROUP_CONCAT
</li></ul>
- [Revision #59d847b](https://github.com/MariaDB/server/commit/59d847b)
<span class="cstm-style datetime">2015-04-15 12:08:37 +0400</span>
<ul start="1"><li>[MDEV-7814](https://jira.mariadb.org/browse/MDEV-7814) Assertion `args[0]-&gt;fixed' fails in Item_func_conv_charset::Item_func_conv_charset Removing a wrong assertion.
</li></ul>
- [Revision #b9a7586](https://github.com/MariaDB/server/commit/b9a7586)
<span class="cstm-style datetime">2015-03-05 16:34:13 +0100</span>
<ul start="1"><li>[MDEV-7613](https://jira.mariadb.org/browse/MDEV-7613): [MariaDB 5.5.40](/kb/en/mariadb-5540-release-notes/) server crash on update table left join with a view
</li></ul>
- [Revision #83ce352](https://github.com/MariaDB/server/commit/83ce352)
<span class="cstm-style datetime">2015-04-14 13:26:55 +1000</span>
<ul start="1"><li>quote table name in mysql_check:is_view. increment version too
</li></ul>
- [Revision #4987080](https://github.com/MariaDB/server/commit/4987080)
<span class="cstm-style datetime">2015-04-14 13:26:22 +1000</span>
<ul start="1"><li>Don't run upgrade-views if not mysql or --upgrade-system-tables
</li></ul>
- [Revision #97e0aea](https://github.com/MariaDB/server/commit/97e0aea)
<span class="cstm-style datetime">2015-04-14 12:43:50 +1000</span>
<ul start="1"><li>mysqlcheck fix-view-algorithm -&gt; upgrade-views
</li></ul>
- [Revision #808608c](https://github.com/MariaDB/server/commit/808608c)
<span class="cstm-style datetime">2015-04-14 11:26:13 +1000</span>
<ul start="1"><li>corrected mysql_upgrade to always list output for every phase
</li></ul>
- [Revision #c584058](https://github.com/MariaDB/server/commit/c584058)
<span class="cstm-style datetime">2015-04-14 11:01:31 +1000</span>
<ul start="1"><li>Update tests for mysql_upgrade_view
</li></ul>
- [Revision #76c18f7](https://github.com/MariaDB/server/commit/76c18f7)
<span class="cstm-style datetime">2015-04-13 23:25:23 +1000</span>
<ul start="1"><li>sql_print_information corrected
</li></ul>
- [Revision #622891c](https://github.com/MariaDB/server/commit/622891c)
<span class="cstm-style datetime">2015-04-13 22:58:45 +1000</span>
<ul start="1"><li>mariadb_fix_view to allow fixing of view-&gt;mariadb_version
</li></ul>
- [Revision #8a827d5](https://github.com/MariaDB/server/commit/8a827d5)
<span class="cstm-style datetime">2015-04-13 22:39:37 +1000</span>
<ul start="1"><li>avoid calling runctiosn in DBUG_RETURN
</li></ul>
- [Revision #29721d7](https://github.com/MariaDB/server/commit/29721d7)
<span class="cstm-style datetime">2015-04-13 22:31:44 +1000</span>
<ul start="1"><li>mariadb_fix_view need only check view-&gt;mariadb_version
</li></ul>
- [Revision #7229b19](https://github.com/MariaDB/server/commit/7229b19)
<span class="cstm-style datetime">2015-04-13 22:28:12 +1000</span>
<ul start="1"><li>remove include sql_view.h from sql_table.cc - unneeded
</li></ul>
- [Revision #fc277cd](https://github.com/MariaDB/server/commit/fc277cd)
<span class="cstm-style datetime">2015-04-13 22:17:57 +1000</span>
<ul start="1"><li>Add --fix-tables option to mysql-check
</li></ul>
- [Revision #28b1731](https://github.com/MariaDB/server/commit/28b1731)
<span class="cstm-style datetime">2015-04-13 21:12:23 +1000</span>
<ul start="1"><li>Allow REPAIR NO_WRITE_TO_BINLOG as per serg's review
</li></ul>
- [Revision #f91dafc](https://github.com/MariaDB/server/commit/f91dafc)
<span class="cstm-style datetime">2015-04-13 20:52:19 +1000</span>
<ul start="1"><li>correct phase numbering in test results
</li></ul>
- [Revision #eaa3da8](https://github.com/MariaDB/server/commit/eaa3da8)
<span class="cstm-style datetime">2015-04-13 20:41:49 +1000</span>
<ul start="1"><li>Add mysql-test/std_data/mysql_upgrade/* for [MDEV-6916](https://jira.mariadb.org/browse/MDEV-6916)
</li></ul>
- [Revision #4409e04](https://github.com/MariaDB/server/commit/4409e04)
<span class="cstm-style datetime">2015-04-12 21:40:07 +1000</span>
<ul start="1"><li>correct server side error messages
</li></ul>
- [Revision #9b067a3](https://github.com/MariaDB/server/commit/9b067a3)
<span class="cstm-style datetime">2015-04-12 21:05:01 +1000</span>
<ul start="1"><li>Corrections to mysqlcheck
</li></ul>
- [Revision #96e277a](https://github.com/MariaDB/server/commit/96e277a)
<span class="cstm-style datetime">2015-04-12 20:42:13 +1000</span>
<ul start="1"><li>mysql_upgrade to pass binlog option to mysqlcheck
</li></ul>
- [Revision #c8dbef2](https://github.com/MariaDB/server/commit/c8dbef2)
<span class="cstm-style datetime">2015-04-12 20:41:28 +1000</span>
<ul start="1"><li>[MDEV-6916](https://jira.mariadb.org/browse/MDEV-6916) REPAIR VIEW / mysql migration
</li></ul>
- [Revision #e5191dd](https://github.com/MariaDB/server/commit/e5191dd)
<span class="cstm-style datetime">2015-04-12 17:26:50 +1000</span>
<ul start="1"><li>mysql-upgrade -&gt; fix-view-algorithm as mysqlcheck option
</li></ul>
- [Revision #25872e2](https://github.com/MariaDB/server/commit/25872e2)
<span class="cstm-style datetime">2015-04-12 17:21:02 +1000</span>
<ul start="1"><li>Correct phase count on mysql_upgrade
</li></ul>
- [Revision #ebd3c6c](https://github.com/MariaDB/server/commit/ebd3c6c)
<span class="cstm-style datetime">2015-04-12 17:05:02 +1000</span>
<ul start="1"><li>Remove mysql-upgrade / skip-mysql-upgrade options from mysql-upgrade.c
</li></ul>
- [Revision #87f5bae](https://github.com/MariaDB/server/commit/87f5bae)
<span class="cstm-style datetime">2015-04-12 16:50:16 +1000</span>
<ul start="1"><li>Get my_getop to parse opt_mysql_upgrade in mysqlcheck
</li></ul>
- [Revision #70960e7](https://github.com/MariaDB/server/commit/70960e7)
<span class="cstm-style datetime">2015-04-12 15:56:21 +1000</span>
<ul start="1"><li>[MDEV-6916](https://jira.mariadb.org/browse/MDEV-6916): Upgrade from MySQL to MariaDB breaks already created views
</li></ul>
- [Revision #85660d7](https://github.com/MariaDB/server/commit/85660d7)
<span class="cstm-style datetime">2015-04-11 18:13:08 +1000</span>
<ul start="1"><li>[MDEV-7977](https://jira.mariadb.org/browse/MDEV-7977) MYSQL_BIN_LOG::write_incident failing to release LOCK_log
</li></ul>
- [Revision #cc84ac3](https://github.com/MariaDB/server/commit/cc84ac3)
<span class="cstm-style datetime">2015-03-31 13:10:43 +0500</span>
<ul start="1"><li>[MDEV-7596](https://jira.mariadb.org/browse/MDEV-7596) audit plugin - record full query / document line length / make buffer configurable.
<ul start="1"><li>The serve_audit_query_log_limit variable implemented.
</li><li>Also QUERY_DCL filter added.
</li></ul>
</li></ul>
- [Revision #995f622](https://github.com/MariaDB/server/commit/995f622)
<span class="cstm-style datetime">2015-03-30 00:49:16 +0300</span>
<ul start="1"><li>[MDEV-7858](https://jira.mariadb.org/browse/MDEV-7858): main.subselect_sj2_jcl6 fails in buildbot
</li></ul>
- [Revision #d7445ea](https://github.com/MariaDB/server/commit/d7445ea)
<span class="cstm-style datetime">2015-03-27 20:35:37 -0400</span>
<ul start="1"><li>[MDEV-7194](https://jira.mariadb.org/browse/MDEV-7194): galera fails to replicate DDL queries when using binlog_checksum
</li></ul>
- [Revision #6a20454](https://github.com/MariaDB/server/commit/6a20454)
<span class="cstm-style datetime">2015-03-24 16:41:04 -0400</span>
<ul start="1"><li>[MDEV-7798](https://jira.mariadb.org/browse/MDEV-7798): mysql.server init script can't stop mysqld when WSREP is turned off
</li></ul>
- [Revision #86f46a3d](https://github.com/MariaDB/server/commit/86f46a3d)
<span class="cstm-style datetime">2015-03-23 09:49:32 +0200</span>
<ul start="1"><li>[MDEV-7301](https://jira.mariadb.org/browse/MDEV-7301): Unknown column quoted with backticks in HAVING clause when using function.
</li></ul>
- [Revision #9253064](https://github.com/MariaDB/server/commit/9253064)
<span class="cstm-style datetime">2015-03-10 12:34:17 +0200</span>
<ul start="1"><li>[MDEV-7682](https://jira.mariadb.org/browse/MDEV-7682) Incorrect use of SPATIAL KEY for query plan
</li></ul>
- [Revision #5e20df2](https://github.com/MariaDB/server/commit/5e20df2)
<span class="cstm-style datetime">2015-03-19 19:46:08 +0400</span>
<ul start="1"><li>[MDEV-7641](https://jira.mariadb.org/browse/MDEV-7641) Server crash on set global server_audit_incl_users=null.
</li></ul>
- [Revision #c020d36](https://github.com/MariaDB/server/commit/c020d36)
<span class="cstm-style datetime">2015-03-17 13:26:33 +0300</span>
<ul start="1"><li>[MDEV-7474](https://jira.mariadb.org/browse/MDEV-7474): Semi-Join's DuplicateWeedout strategy skipped ...
</li></ul>
- [Revision #5a3bf84](https://github.com/MariaDB/server/commit/5a3bf84)
<span class="cstm-style datetime">2015-03-12 18:53:31 +0200</span>
<ul start="1"><li>[MDEV-7692](https://jira.mariadb.org/browse/MDEV-7692) MariaDB - mysql-test - SUITE:percona - percona.innodb_sys_index 'xtradb' fails - @@version_comment
</li></ul>
- [Revision #7a6cad5](https://github.com/MariaDB/server/commit/7a6cad5)
<span class="cstm-style datetime">2015-03-11 12:36:00 -0400</span>
<ul start="1"><li>Backport fix for [MDEV-7673](https://jira.mariadb.org/browse/MDEV-7673), [MDEV-7203](https://jira.mariadb.org/browse/MDEV-7203) and [MDEV-7192](https://jira.mariadb.org/browse/MDEV-7192) from 10.0-galera
</li></ul>
- [Revision #07ff90e](https://github.com/MariaDB/server/commit/07ff90e)
<span class="cstm-style datetime">2015-03-09 22:55:54 -0400</span>
<ul start="1"><li>Reduce gcache size to cut down disk usage
</li></ul>
- [Revision #34f37aa](https://github.com/MariaDB/server/commit/34f37aa)
<span class="cstm-style datetime">2015-03-02 19:18:10 +0200</span>
<ul start="1"><li>[MDEV-7643](https://jira.mariadb.org/browse/MDEV-7643) MTR creates nested links when tests are run with --mem
</li></ul>
- [Revision #17a3779](https://github.com/MariaDB/server/commit/17a3779)
<span class="cstm-style datetime">2015-03-06 18:13:06 +0100</span>
<ul start="1"><li>after innodb/xtradb merge: use the correct visibility for internal functions
</li></ul>
- [Revision #d7d1907](https://github.com/MariaDB/server/commit/d7d1907)
<span class="cstm-style datetime">2015-03-06 17:03:46 +0100</span>
<ul start="1"><li>[MDEV-6838](https://jira.mariadb.org/browse/MDEV-6838) Using too big key for internal temp tables
</li></ul>
- [Revision #12d87c3](https://github.com/MariaDB/server/commit/12d87c3)
<span class="cstm-style datetime">2015-03-06 11:15:55 +0100</span>
<ul start="1"><li>[MDEV-7659](https://jira.mariadb.org/browse/MDEV-7659) buildbot may leave stale mysqld
</li></ul>
- [Revision #206b111](https://github.com/MariaDB/server/commit/206b111)
<span class="cstm-style datetime">2015-03-06 11:19:23 +0200</span>
<ul start="1"><li>[MDEV-7672](https://jira.mariadb.org/browse/MDEV-7672): Crash creating an InnoDB table with foreign keys
</li></ul>
- [Revision #f66fbe8](https://github.com/MariaDB/server/commit/f66fbe8)
<span class="cstm-style datetime">2015-03-05 12:05:59 +0200</span>
<ul start="1"><li>[MDEV-7578](https://jira.mariadb.org/browse/MDEV-7578) :Slave is 10x slower to execute set of statements compared to master when using RBR
</li></ul>
- [Revision #45b6edb](https://github.com/MariaDB/server/commit/45b6edb)
<span class="cstm-style datetime">2015-02-28 23:44:55 +0200</span>
<ul start="1"><li>[MDEV-6838](https://jira.mariadb.org/browse/MDEV-6838): Using too big key for internal temp tables
</li></ul>
- [Revision #fa87fc7](https://github.com/MariaDB/server/commit/fa87fc7)
<span class="cstm-style datetime">2015-02-27 18:28:40 +0100</span>
<ul start="1"><li>update tokudb version after merge
</li></ul>
- [Revision #b5d6aa5](https://github.com/MariaDB/server/commit/b5d6aa5)
<span class="cstm-style datetime">2015-02-23 13:27:51 +0100</span>
<ul start="1"><li>[MDEV-7310](https://jira.mariadb.org/browse/MDEV-7310): last_commit_pos_offset set to wrong value after binlog rotate in group commit
</li></ul>