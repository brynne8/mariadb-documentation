# MariaDB Galera Cluster 5.5.48 Changelog

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.48)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5548-release-notes/)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5548-changelog/)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 23 Feb 2016

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5548-release-notes/).

The revision number links will take you to the revision's page on Github. On
Github you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #3042d65](https://github.com/MariaDB/server/commit/3042d65)
<span class="cstm-style datetime">2016-02-17 15:50:01 -0500</span>
<ul start="1"><li>[MDEV-9577](https://jira.mariadb.org/browse/MDEV-9577): sys_vars.ignore_db_dirs_basic fails under Valgrind
</li></ul>
- [Revision #0225962](https://github.com/MariaDB/server/commit/0225962)
<span class="cstm-style datetime">2016-02-16 17:14:11 -0500</span>
<ul start="1"><li>Update global_suppressions for galera suite to include new warning.
</li></ul>
- [Revision #d23bd26](https://github.com/MariaDB/server/commit/d23bd26)
<span class="cstm-style datetime">2016-02-13 18:28:36 -0500</span>
<ul start="1"><li>Merge tag 'mariadb-5.5.48' into 5.5-galera
</li></ul>
- [Revision #b83de11](https://github.com/MariaDB/server/commit/b83de11)
<span class="cstm-style datetime">2016-02-10 18:04:08 -0500</span>
<ul start="1"><li>Update WSREP_PATCH_REVNO.
</li></ul>
- [Revision #a6d0903](https://github.com/MariaDB/server/commit/a6d0903)
<span class="cstm-style datetime">2016-01-11 12:03:35 +0200</span>
<ul start="1"><li>Bump WSREP_PATCH_VERSION in cmake/wsrep.cmake to 14
</li></ul>
- [Revision #403a8bf](https://github.com/MariaDB/server/commit/403a8bf)
<span class="cstm-style datetime">2015-11-16 04:07:08 -0800</span>
<ul start="1"><li>Bump WSREP_PATCH_VERSION in cmake/wsrep.cmake to 13
</li></ul>
- [Revision #1ce821b](https://github.com/MariaDB/server/commit/1ce821b)
<span class="cstm-style datetime">2015-11-12 10:33:04 +0200</span>
<ul start="1"><li>Refs codership/mysql-wsrep#221     - disabling certain IB atomic builtins, which caused complete hangs
</li></ul>
- [Revision #bd1d2b9](https://github.com/MariaDB/server/commit/bd1d2b9)
<span class="cstm-style datetime">2015-11-06 10:50:21 +0100</span>
<ul start="1"><li>refs codership/mysql-wsrep#201
</li></ul>
- [Revision #8a93a7c](https://github.com/MariaDB/server/commit/8a93a7c)
<span class="cstm-style datetime">2015-11-04 16:19:48 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#226 Limit binlog recovery to found wsrep position
</li></ul>
- [Revision #652e4c1](https://github.com/MariaDB/server/commit/652e4c1)
<span class="cstm-style datetime">2016-02-10 17:29:28 -0500</span>
<ul start="1"><li>Fix for a build failure.
</li></ul>
- [Revision #484bbd3](https://github.com/MariaDB/server/commit/484bbd3)
<span class="cstm-style datetime">2015-11-04 09:36:01 +0100</span>
<ul start="1"><li>refs codership/mysql-wsrep#201
</li></ul>
- [Revision #a9a08b1](https://github.com/MariaDB/server/commit/a9a08b1)
<span class="cstm-style datetime">2016-02-10 10:03:47 +0400</span>
<ul start="1"><li>[MDEV-9371](https://jira.mariadb.org/browse/MDEV-9371) select insert('a',2,1,'b') doesn't return expected 'a'
</li></ul>
- [Revision #0c7dffe](https://github.com/MariaDB/server/commit/0c7dffe)
<span class="cstm-style datetime">2015-10-22 17:30:20 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#201
</li></ul>
- [Revision #8a71fde](https://github.com/MariaDB/server/commit/8a71fde)
<span class="cstm-style datetime">2015-10-20 17:54:14 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#201
</li></ul>
- [Revision #3c5c04b](https://github.com/MariaDB/server/commit/3c5c04b)
<span class="cstm-style datetime">2016-02-10 03:49:11 +0200</span>
<ul start="1"><li>[MDEV-7122](https://jira.mariadb.org/browse/MDEV-7122): Assertion `0' failed in subselect_hash_sj_engine::init
</li></ul>
- [Revision #6b614c6](https://github.com/MariaDB/server/commit/6b614c6)
<span class="cstm-style datetime">2016-02-09 13:50:48 +0100</span>
<ul start="1"><li>[MDEV-7765](https://jira.mariadb.org/browse/MDEV-7765): Crash (Assertion `!table || (!table-&gt;write_set || bitmap_is_set(table-&gt;write_set, field_index) || bitmap_is_set(table-&gt;vcol_set, field_index))' fails) on using function over not created table
</li></ul>
- [Revision #775cccc](https://github.com/MariaDB/server/commit/775cccc)
<span class="cstm-style datetime">2016-02-08 22:53:40 +0200</span>
<ul start="1"><li>[MDEV-7122](https://jira.mariadb.org/browse/MDEV-7122): Assertion `0' failed in subselect_hash_sj_engine::init
</li></ul>
- [Revision #01628ce](https://github.com/MariaDB/server/commit/01628ce)
<span class="cstm-style datetime">2016-02-09 14:08:36 +0100</span>
<ul start="1"><li>Merge branch 'bb-5.5-serg' into 5.5
</li></ul>
- [Revision #afce541](https://github.com/MariaDB/server/commit/afce541)
<span class="cstm-style datetime">2016-02-09 14:06:45 +0100</span>
<ul start="1"><li>Merge branch 'merge-xtradb-5.5' into 5.5
</li></ul>
- [Revision #5d478f5](https://github.com/MariaDB/server/commit/5d478f5)
<span class="cstm-style datetime">2016-02-08 20:07:38 +0100</span>
<ul start="1"><li>Bug#19817021
</li></ul>
- [Revision #6703e5b](https://github.com/MariaDB/server/commit/6703e5b)
<span class="cstm-style datetime">2016-02-08 20:07:09 +0100</span>
<ul start="1"><li>Bug#20691429 ASSERTION `CHILD_L' FAILED IN STORAGE/MYISAMMRG/HA_MYISAMMRG.CC:631
</li></ul>
- [Revision #dece4bc](https://github.com/MariaDB/server/commit/dece4bc)
<span class="cstm-style datetime">2016-02-09 11:28:44 +0100</span>
<ul start="1"><li>cleanup: make assert more readable
</li></ul>
- [Revision #63d3ccd](https://github.com/MariaDB/server/commit/63d3ccd)
<span class="cstm-style datetime">2016-02-08 20:04:39 +0100</span>
<ul start="1"><li>Bug#21205695	DROP TABLE MAY CAUSE SLAVES TO BREAK
</li></ul>
- [Revision #f3444df](https://github.com/MariaDB/server/commit/f3444df)
<span class="cstm-style datetime">2016-02-09 11:27:40 +0100</span>
<ul start="1"><li>Merge branch 'mysql/5.5' into 5.5
</li></ul>
- [Revision #ea0c3fc](https://github.com/MariaDB/server/commit/ea0c3fc)
<span class="cstm-style datetime">2016-02-09 05:17:41 +0400</span>
<ul start="1"><li>[MDEV-9438](https://jira.mariadb.org/browse/MDEV-9438) backport feedback-http-proxy to 5.5 and 10.0.         The http-proxy option to the FEEDBACK plugin backported.
</li></ul>
- [Revision #b17a435](https://github.com/MariaDB/server/commit/b17a435)
<span class="cstm-style datetime">2016-02-09 02:31:47 +0300</span>
<ul start="1"><li>[MDEV-6859](https://jira.mariadb.org/browse/MDEV-6859): scalar subqueries in a comparison produced unexpected result
</li></ul>
- [Revision #3cfd36b](https://github.com/MariaDB/server/commit/3cfd36b)
<span class="cstm-style datetime">2016-02-09 00:13:25 +0100</span>
<ul start="1"><li>5.5.47-37.7
</li></ul>
- [Revision #d443d70](https://github.com/MariaDB/server/commit/d443d70)
<span class="cstm-style datetime">2016-02-09 01:46:53 +0300</span>
<ul start="1"><li>[MDEV-7823](https://jira.mariadb.org/browse/MDEV-7823): Server crashes in next_depth_first_tab on nested IN clauses with SQ inside
</li></ul>
- [Revision #c4cb240](https://github.com/MariaDB/server/commit/c4cb240)
<span class="cstm-style datetime">2016-02-06 22:41:58 +0100</span>
<ul start="1"><li>[MDEV-9024](https://jira.mariadb.org/browse/MDEV-9024) Build fails with VS2015
</li></ul>
- [Revision #1e361f2](https://github.com/MariaDB/server/commit/1e361f2)
<span class="cstm-style datetime">2016-02-06 13:57:59 +0100</span>
<ul start="1"><li>[MDEV-4664](https://jira.mariadb.org/browse/MDEV-4664) mysql_upgrade crashes if root's password contains an apostrophe/single quotation mark
</li></ul>
- [Revision #9e4e412](https://github.com/MariaDB/server/commit/9e4e412)
<span class="cstm-style datetime">2016-02-06 13:56:37 +0100</span>
<ul start="1"><li>unit test for dynstr_append_os_quoted()
</li></ul>
- [Revision #41021c0](https://github.com/MariaDB/server/commit/41021c0)
<span class="cstm-style datetime">2016-02-03 17:15:22 +0100</span>
<ul start="1"><li>[MDEV-9462](https://jira.mariadb.org/browse/MDEV-9462): Out of memory using explain on 2 empty tables
</li></ul>
- [Revision #ad94790](https://github.com/MariaDB/server/commit/ad94790)
<span class="cstm-style datetime">2016-02-04 14:47:46 +0100</span>
<ul start="1"><li>[MDEV-9453](https://jira.mariadb.org/browse/MDEV-9453) mysql_upgrade.exe error when mysql is migrated to mariadb
</li></ul>
- [Revision #0a76ad5](https://github.com/MariaDB/server/commit/0a76ad5)
<span class="cstm-style datetime">2016-02-04 12:51:57 +0100</span>
<ul start="1"><li>[MDEV-9175](https://jira.mariadb.org/browse/MDEV-9175) Query parser tansforms MICROSECOND into SECOND_FRAC, which does not work
</li></ul>
- [Revision #a90da6e](https://github.com/MariaDB/server/commit/a90da6e)
<span class="cstm-style datetime">2016-02-05 14:04:24 +0100</span>
<ul start="1"><li>[MDEV-9314](https://jira.mariadb.org/browse/MDEV-9314) fatal build error: viosslfactories.c:58:5: error: dereferencing pointer to incomplete type ‘DH {aka struct dh_st}
</li></ul>
- [Revision #db5f743](https://github.com/MariaDB/server/commit/db5f743)
<span class="cstm-style datetime">2016-02-06 12:37:46 +0200</span>
<ul start="1"><li>Merge pull request #148 from grooverdan/5.5-rpl_reporting-cppcheck-va_end
</li></ul>
- [Revision #6ecf6d8](https://github.com/MariaDB/server/commit/6ecf6d8)
<span class="cstm-style datetime">2016-02-05 17:46:01 +0100</span>
<ul start="1"><li>[MDEV-7827](https://jira.mariadb.org/browse/MDEV-7827): Assertion `!table || (!table-&gt;read_set || bitmap_is_set(table-&gt;read_set, field_index))' failed in Field_long::val_str on EXPLAIN EXTENDED
</li></ul>
- [Revision #9f3b53f](https://github.com/MariaDB/server/commit/9f3b53f)
<span class="cstm-style datetime">2015-12-14 19:16:29 +0100</span>
<ul start="1"><li>[MDEV-9093](https://jira.mariadb.org/browse/MDEV-9093) Persistent computed column is not updated when update query contains join
</li></ul>
- [Revision #a3d843d](https://github.com/MariaDB/server/commit/a3d843d)
<span class="cstm-style datetime">2016-02-03 15:52:26 +0200</span>
<ul start="1"><li>Fix function visibility as it is used on row0mysql.c in Windows.
</li></ul>
- [Revision #f66d016](https://github.com/MariaDB/server/commit/f66d016)
<span class="cstm-style datetime">2016-02-03 11:32:51 +0200</span>
<ul start="1"><li>[MDEV-9471](https://jira.mariadb.org/browse/MDEV-9471): Server crashes or returns an error while trying to alter partitioning on a table moved from Windows to Linux
</li></ul>
- [Revision #603c096](https://github.com/MariaDB/server/commit/603c096)
<span class="cstm-style datetime">2016-02-03 00:43:00 +0100</span>
<ul start="1"><li>[MDEV-9466](https://jira.mariadb.org/browse/MDEV-9466) : Exception handler on Windows does not output any text, if mysqld runs as service
</li></ul>
- [Revision #0e84d54](https://github.com/MariaDB/server/commit/0e84d54)
<span class="cstm-style datetime">2016-02-01 16:27:12 +0100</span>
<ul start="1"><li>Merge [MDEV-9112](https://jira.mariadb.org/browse/MDEV-9112) into 5.5
</li></ul>
- [Revision #8cf1f50](https://github.com/MariaDB/server/commit/8cf1f50)
<span class="cstm-style datetime">2016-02-01 16:10:49 +0100</span>
<ul start="1"><li>[MDEV-9112](https://jira.mariadb.org/browse/MDEV-9112): Non-blocking client API missing on non-x86 platforms
</li></ul>
- [Revision #d0c5efc](https://github.com/MariaDB/server/commit/d0c5efc)
<span class="cstm-style datetime">2016-01-29 23:53:44 +0200</span>
<ul start="1"><li>If one compiled with too long MYSQL_SERVER_SUFFIX this caused a memory overrun that caused some test to fail.
</li></ul>
- [Revision #a1ddf01](https://github.com/MariaDB/server/commit/a1ddf01)
<span class="cstm-style datetime">2016-01-29 23:52:15 +0200</span>
<ul start="1"><li>my_decimal didn't compile properly with debug
</li></ul>
- [Revision #3e5724f](https://github.com/MariaDB/server/commit/3e5724f)
<span class="cstm-style datetime">2016-01-19 14:47:41 +1100</span>
<ul start="1"><li>Add va_end to make cppcheck happy
</li></ul>
- [Revision #9c9d10b](https://github.com/MariaDB/server/commit/9c9d10b)
<span class="cstm-style datetime">2016-01-15 09:50:27 +0400</span>
<ul start="1"><li>[MDEV-9106](https://jira.mariadb.org/browse/MDEV-9106) Audit plugin not working with MySQL 5.7.         fixing Windows crash.
</li></ul>
- [Revision #fe4823d](https://github.com/MariaDB/server/commit/fe4823d)
<span class="cstm-style datetime">2016-01-13 18:02:44 +0400</span>
<ul start="1"><li>[MDEV-9106](https://jira.mariadb.org/browse/MDEV-9106) Audit plugin doesnt run with MySQL 5.7.    updata thread_pool_server_audit test result.
</li></ul>
- [Revision #cdc9aa5](https://github.com/MariaDB/server/commit/cdc9aa5)
<span class="cstm-style datetime">2016-01-13 15:24:33 +0400</span>
<ul start="1"><li>[MDEV-9106](https://jira.mariadb.org/browse/MDEV-9106) Audit Plugin doesn't run with MySQL 5.7.     [MariaDB 5.5](/kb/en/what-is-mariadb-55/) built in debug gets unhappy with mutexes.     Although everything is correct, some DBUG_ASSERT can happen.     So this patch keeps safe_mutex silent.
</li></ul>
- [Revision #c955253](https://github.com/MariaDB/server/commit/c955253)
<span class="cstm-style datetime">2016-01-12 16:29:02 +0400</span>
<ul start="1"><li>[MDEV-9106](https://jira.mariadb.org/browse/MDEV-9106) Audit plugin compiled with MariaDB can't install on MySQL 5.7.         The audit API was seriously changed in MySQL 5.7.         so we had to adapt the plugin's code to that.
</li></ul>
- [Revision #5f48b61](https://github.com/MariaDB/server/commit/5f48b61)
<span class="cstm-style datetime">2016-01-07 14:45:40 +0100</span>
<ul start="1"><li>[MDEV-9298](https://jira.mariadb.org/browse/MDEV-9298) : Build failure when linking libmysql.
</li></ul>
- [Revision #ff24820](https://github.com/MariaDB/server/commit/ff24820)
<span class="cstm-style datetime">2015-12-30 19:39:31 +0100</span>
<ul start="1"><li>Fix process  handle leak in buildbot. GenerateConsoleCtrlEvent sent to non-existing process will add a process handle to this non-existing process to console host process conhost.exe
</li></ul>
- [Revision #61d3621](https://github.com/MariaDB/server/commit/61d3621)
<span class="cstm-style datetime">2015-12-29 18:40:41 +0400</span>
<ul start="1"><li>Moving Field_blob::store_length() back from protected to public, as it's needed for Cassandra in 10.0.
</li></ul>
- [Revision #e1b9be5](https://github.com/MariaDB/server/commit/e1b9be5)
<span class="cstm-style datetime">2015-12-29 14:17:31 +0400</span>
<ul start="1"><li>[MDEV-9319](https://jira.mariadb.org/browse/MDEV-9319) ALTER from a bigger to a smaller blob type truncates too much data
</li></ul>
- [Revision #e126baa](https://github.com/MariaDB/server/commit/e126baa)
<span class="cstm-style datetime">2015-12-21 10:19:02 +0100</span>
<ul start="1"><li>[MDEV-9249](https://jira.mariadb.org/browse/MDEV-9249) MariaDB un-buildable on linux64: fails @ "error: ‘ERR_remove_state’ was not declared in this scope" when linking against OpenSSL 1.0.2e
</li></ul>
- [Revision #591e74c](https://github.com/MariaDB/server/commit/591e74c)
<span class="cstm-style datetime">2015-06-20 16:59:22 +0800</span>
<ul start="1"><li>[MDEV-7526](https://jira.mariadb.org/browse/MDEV-7526): TokuDB doesn't build on OS X
</li></ul>
- [Revision #e386523](https://github.com/MariaDB/server/commit/e386523)
<span class="cstm-style datetime">2015-12-19 13:53:43 +0200</span>
<ul start="1"><li>[MDEV-7526](https://jira.mariadb.org/browse/MDEV-7526): TokuDB doesn't build on OS X
</li></ul>
- [Revision #f39b9e0](https://github.com/MariaDB/server/commit/f39b9e0)
<span class="cstm-style datetime">2015-12-19 13:52:27 +0200</span>
<ul start="1"><li>[MDEV-7526](https://jira.mariadb.org/browse/MDEV-7526): TokuDB doesn't build on OS X
</li></ul>
- [Revision #6414959](https://github.com/MariaDB/server/commit/6414959)
<span class="cstm-style datetime">2015-12-19 13:31:44 +0200</span>
<ul start="1"><li>[MDEV-7526](https://jira.mariadb.org/browse/MDEV-7526): TokuDB doesn't build on OS X
</li></ul>
- [Revision #f89c9fc](https://github.com/MariaDB/server/commit/f89c9fc)
<span class="cstm-style datetime">2015-12-19 13:25:55 +0200</span>
<ul start="1"><li>[MDEV-7526](https://jira.mariadb.org/browse/MDEV-7526): TokuDB doesn't build on OS X
</li></ul>
- [Revision #0ed4744](https://github.com/MariaDB/server/commit/0ed4744)
<span class="cstm-style datetime">2015-12-11 17:03:55 +0100</span>
<ul start="1"><li>fix main.mysqldump test on windows
</li></ul>
- [Revision #ca28d90](https://github.com/MariaDB/server/commit/ca28d90)
<span class="cstm-style datetime">2015-12-09 17:54:55 +0100</span>
<ul start="1"><li>[MDEV-7655](https://jira.mariadb.org/browse/MDEV-7655) SHOW CREATE TABLE returns invalid DDL when using virtual columns along with a table collation
</li></ul>
- [Revision #f560c1b](https://github.com/MariaDB/server/commit/f560c1b)
<span class="cstm-style datetime">2015-12-10 10:32:11 +0100</span>
<ul start="1"><li>revert 5e9a50efc37c233f1e2a3616f8bcb36315aba4c2
</li></ul>
- [Revision #265e833](https://github.com/MariaDB/server/commit/265e833)
<span class="cstm-style datetime">2015-12-09 21:22:37 +0100</span>
<ul start="1"><li>revert 415faa122b9c683661dafac82fff414fa6864151
</li></ul>
- [Revision #c19972f](https://github.com/MariaDB/server/commit/c19972f)
<span class="cstm-style datetime">2015-12-11 14:33:41 +0200</span>
<ul start="1"><li>[MDEV-9251](https://jira.mariadb.org/browse/MDEV-9251): Fix MySQL Bug#20755615: InnoDB compares column names case sensitively, while according to Storage Engine API column names should be compared case insensitively. This can cause FRM and InnoDB data dictionary to go out of sync.
</li></ul>
- [Revision #fa25921](https://github.com/MariaDB/server/commit/fa25921)
<span class="cstm-style datetime">2015-12-10 11:22:53 +0100</span>
<ul start="1"><li>[MDEV-8407](https://jira.mariadb.org/browse/MDEV-8407) Numeric errors, server crash with COLUMN_JSON() on DECIMAL with precision &gt; 40</li></ul>