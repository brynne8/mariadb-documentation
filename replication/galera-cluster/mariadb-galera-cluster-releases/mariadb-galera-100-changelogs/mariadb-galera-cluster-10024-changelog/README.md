# MariaDB Galera Cluster 10.0.24 Changelog

The most recent [MariaDB Galera Cluster 10.0](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 10.0.38](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10038-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/10.0)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/10.0.24)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10024-release-notes/)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10024-changelog/)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 26 Feb 2016

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10024-release-notes/).
For changes made in MariaDB, see the [MariaDB 10.0.24 Changelog](/kb/en/mariadb-10024-changelog/).

The revision number links will take you to the revision's page on Github. On
Github you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #01897db](https://github.com/MariaDB/server/commit/01897db)
<span class="cstm-style datetime">2016-02-24 17:40:12 -0500</span>
<ul start="1"><li>Skip galera_sync_wait_show.test on non-debug builds.
</li></ul>
- [Revision #f67d6fc](https://github.com/MariaDB/server/commit/f67d6fc)
<span class="cstm-style datetime">2015-12-04 15:09:08 +0530</span>
<ul start="1"><li>- PXC#480: xtrabackup-v2 SST fails with multiple log_bin directives in my.cnf
</li></ul>
- [Revision #0cf66e4](https://github.com/MariaDB/server/commit/0cf66e4)
<span class="cstm-style datetime">2015-10-22 14:56:29 +0530</span>
<ul start="1"><li>- PXC#460: wsrep_sst_auth don't work in Percona-XtraDB-Cluster-56-5.6.25-25.12.1.el7
</li></ul>
- [Revision #0fd9d5a](https://github.com/MariaDB/server/commit/0fd9d5a)
<span class="cstm-style datetime">2016-02-23 21:24:00 -0500</span>
<ul start="1"><li>Update WSREP_PATCH_REVNO.
</li></ul>
- [Revision #1b0d811](https://github.com/MariaDB/server/commit/1b0d811)
<span class="cstm-style datetime">2016-02-23 21:08:42 -0500</span>
<ul start="1"><li>Merge branch '5.5-galera' into 10.0-galera
</li></ul>
- [Revision #0d58323](https://github.com/MariaDB/server/commit/0d58323)
<span class="cstm-style datetime">2016-02-23 20:53:29 -0500</span>
<ul start="1"><li>Merge tag 'mariadb-10.0.24' into 10.0-galera
</li></ul>
- [Revision #276d65b](https://github.com/MariaDB/server/commit/276d65b)
<span class="cstm-style datetime">2016-02-23 20:33:21 -0500</span>
<ul start="1"><li>Fix for test failures.
</li></ul>
- [Revision #b9c42d7](https://github.com/MariaDB/server/commit/b9c42d7)
<span class="cstm-style datetime">2016-01-11 11:57:22 +0200</span>
<ul start="1"><li>Bump WSREP_PATCH_VERSION in cmake/wsrep.cmake to 13
</li></ul>
- [Revision #15118d3](https://github.com/MariaDB/server/commit/15118d3)
<span class="cstm-style datetime">2016-02-23 00:30:47 -0500</span>
<ul start="1"><li>refs codership/mysql-wsrep#237: Add sync point for mtr test.
</li></ul>
- [Revision #b633dbd](https://github.com/MariaDB/server/commit/b633dbd)
<span class="cstm-style datetime">2015-12-23 09:44:32 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#237 - test for FLUSH TABLES hang in slave node
</li></ul>
- [Revision #32df0b1](https://github.com/MariaDB/server/commit/32df0b1)
<span class="cstm-style datetime">2015-12-02 23:20:10 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#233 - avoiding the race condition, by not grabbing thd-&gt;LOCK_wsrep_thd for   accessing thd-&gt;wsrep_exec_mode. The caller is same thread and exec mode   can only be changed by self.
</li></ul>
- [Revision #90e5e2f](https://github.com/MariaDB/server/commit/90e5e2f)
<span class="cstm-style datetime">2015-12-02 23:16:25 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#233 - added mtr test case for this issue - not a perfect one, depends on some sleeps instead of checking   if sync points are met
</li></ul>
- [Revision #2cdcde9](https://github.com/MariaDB/server/commit/2cdcde9)
<span class="cstm-style datetime">2016-02-23 00:19:41 -0500</span>
<ul start="1"><li>Merge sync point from previous commit to XtraDB.
</li></ul>
- [Revision #18f160d](https://github.com/MariaDB/server/commit/18f160d)
<span class="cstm-style datetime">2015-12-02 22:57:46 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#233 - added dbug sync points for further mtr test for this issue
</li></ul>
- [Revision #bf9572b](https://github.com/MariaDB/server/commit/bf9572b)
<span class="cstm-style datetime">2015-11-25 03:36:26 -0800</span>
<ul start="1"><li>refs codership/mysql-wsrep#228 - a test for wsrep_sync_wait and SHOW
</li></ul>
- [Revision #1e14db1](https://github.com/MariaDB/server/commit/1e14db1)
<span class="cstm-style datetime">2015-11-16 11:57:38 +0100</span>
<ul start="1"><li>refs codership/mysql-wsrep#228
</li></ul>
- [Revision #5ebf6ce](https://github.com/MariaDB/server/commit/5ebf6ce)
<span class="cstm-style datetime">2015-11-16 04:06:38 -0800</span>
<ul start="1"><li>Bump WSREP_PATCH_VERSION in cmake/wsrep.cmake to 12
</li></ul>
- [Revision #2b7a5d9](https://github.com/MariaDB/server/commit/2b7a5d9)
<span class="cstm-style datetime">2015-11-16 03:00:27 -0800</span>
<ul start="1"><li>Galera MTR Tests: adjust the galera.galera_defaults test for the new MTR default value for repl.causal_read_timeout
</li></ul>
- [Revision #8504330](https://github.com/MariaDB/server/commit/8504330)
<span class="cstm-style datetime">2015-11-13 04:03:39 -0800</span>
<ul start="1"><li>Galera MTR Tests: misc test stability fixes
</li></ul>
- [Revision #c665934](https://github.com/MariaDB/server/commit/c665934)
<span class="cstm-style datetime">2015-11-03 14:16:08 +0100</span>
<ul start="1"><li>refs codership/mysql-wsrep#201
</li></ul>
- [Revision #c05d85f](https://github.com/MariaDB/server/commit/c05d85f)
<span class="cstm-style datetime">2016-02-22 22:35:48 -0500</span>
<ul start="1"><li>Refs codership/mysql-wsrep#198 : Fix test case
</li></ul>
- [Revision #e9d805b](https://github.com/MariaDB/server/commit/e9d805b)
<span class="cstm-style datetime">2015-10-23 00:01:16 -0700</span>
<ul start="1"><li>Refs codership/mysql-wsrep#198 . MTR test case
</li></ul>
- [Revision #d45f0c1](https://github.com/MariaDB/server/commit/d45f0c1)
<span class="cstm-style datetime">2016-02-22 22:30:14 -0500</span>
<ul start="1"><li>refs codership/mysql-wsrep#198: Revert test changes from previous commit
</li></ul>
- [Revision #ea0b183](https://github.com/MariaDB/server/commit/ea0b183)
<span class="cstm-style datetime">2015-10-23 09:38:33 +0300</span>
<ul start="1"><li>refs codership/mysql-wsrep#198 Removed code duplication, PXC specifics
</li></ul>
- [Revision #235bebe](https://github.com/MariaDB/server/commit/235bebe)
<span class="cstm-style datetime">2015-10-22 17:30:20 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#201
</li></ul>
- [Revision #17ac959](https://github.com/MariaDB/server/commit/17ac959)
<span class="cstm-style datetime">2016-02-22 22:07:59 -0500</span>
<ul start="1"><li>Bug#1421360: Add Percona Server specific FLUSH statements.
</li></ul>
- [Revision #5d4fb15](https://github.com/MariaDB/server/commit/5d4fb15)
<span class="cstm-style datetime">2016-02-22 22:05:16 -0500</span>
<ul start="1"><li>Fix for compilation failure.
</li></ul>
- [Revision #7d89deb](https://github.com/MariaDB/server/commit/7d89deb)
<span class="cstm-style datetime">2015-10-22 14:59:53 +0300</span>
<ul start="1"><li>refs codership/mysql-wsrep#198 fixed merge issues
</li></ul>
- [Revision #0ecc4fe](https://github.com/MariaDB/server/commit/0ecc4fe)
<span class="cstm-style datetime">2015-07-07 14:20:22 +0530</span>
<ul start="1"><li>Bug#1421360: Add Percona Server specific FLUSH statements.
</li></ul>
- [Revision #1077eef](https://github.com/MariaDB/server/commit/1077eef)
<span class="cstm-style datetime">2015-07-16 05:24:13 -0700</span>
<ul start="1"><li>PXC-391: Avoid Total Order Isolation (TOI) for LOCAL sql admin commands.
</li></ul>
- [Revision #5be449d](https://github.com/MariaDB/server/commit/5be449d)
<span class="cstm-style datetime">2015-10-21 01:25:32 -0700</span>
<ul start="1"><li>Galera MTR Tests: attempt to work around codership/QA#179 in galera_as_slave_nonprim.test
</li></ul>
- [Revision #d794f05](https://github.com/MariaDB/server/commit/d794f05)
<span class="cstm-style datetime">2015-10-21 01:15:52 -0700</span>
<ul start="1"><li>Galera MTR Tests: stability fix for galera_gcs_fragment.test (TCP port was output to the .result file)
</li></ul>
- [Revision #ace86a2](https://github.com/MariaDB/server/commit/ace86a2)
<span class="cstm-style datetime">2015-10-20 17:54:14 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#201
</li></ul>
- [Revision #c1ea057](https://github.com/MariaDB/server/commit/c1ea057)
<span class="cstm-style datetime">2016-02-22 16:51:45 -0500</span>
<ul start="1"><li>refs codership/mysql-wsrep#184
</li></ul>
- [Revision #251c53a](https://github.com/MariaDB/server/commit/251c53a)
<span class="cstm-style datetime">2015-10-19 11:17:13 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#184
</li></ul>
- [Revision #5ad30e8](https://github.com/MariaDB/server/commit/5ad30e8)
<span class="cstm-style datetime">2015-10-16 15:57:22 +0300</span>
<ul start="1"><li>MTR test for checking correctness of fragmentation over CCs
</li></ul>
- [Revision #cf43620](https://github.com/MariaDB/server/commit/cf43620)
<span class="cstm-style datetime">2015-10-16 11:51:11 +0200</span>
<ul start="1"><li>refs codership/galera#308
</li></ul>
- [Revision #8c89e84](https://github.com/MariaDB/server/commit/8c89e84)
<span class="cstm-style datetime">2015-10-16 10:22:30 +0200</span>
<ul start="1"><li>refs codership/galera#308
</li></ul>
- [Revision #2c56142](https://github.com/MariaDB/server/commit/2c56142)
<span class="cstm-style datetime">2016-02-22 16:36:05 -0500</span>
<ul start="1"><li>refs codership/mysql-wsrep#184
</li></ul>
- [Revision #1d21676](https://github.com/MariaDB/server/commit/1d21676)
<span class="cstm-style datetime">2015-10-15 15:13:29 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#184
</li></ul>
- [Revision #267d429](https://github.com/MariaDB/server/commit/267d429)
<span class="cstm-style datetime">2015-10-05 11:01:04 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#31
</li></ul>
- [Revision #c0dac42](https://github.com/MariaDB/server/commit/c0dac42)
<span class="cstm-style datetime">2015-10-05 09:42:03 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#31
</li></ul>
- [Revision #0ec457b](https://github.com/MariaDB/server/commit/0ec457b)
<span class="cstm-style datetime">2015-10-02 10:16:55 +0200</span>
<ul start="1"><li>refs codership/galera#308
</li></ul>
- [Revision #00b058a](https://github.com/MariaDB/server/commit/00b058a)
<span class="cstm-style datetime">2015-10-01 17:05:48 +0300</span>
<ul start="1"><li>refs codership/mysql-wsrep#202 Added schema info into wsrep messages
</li></ul>
- [Revision #7ce84cf](https://github.com/MariaDB/server/commit/7ce84cf)
<span class="cstm-style datetime">2015-09-29 23:29:54 -0700</span>
<ul start="1"><li>Galera MTR Tests: stability fixes
</li></ul>
- [Revision #2f870f5](https://github.com/MariaDB/server/commit/2f870f5)
<span class="cstm-style datetime">2015-09-15 13:20:55 +0300</span>
<ul start="1"><li>Restore original value of wsrep_on after waiting for sync point.
</li></ul>
- [Revision #d01328d](https://github.com/MariaDB/server/commit/d01328d)
<span class="cstm-style datetime">2015-09-13 18:57:20 +0300</span>
<ul start="1"><li>Helpers to deal with galera dbug sync points.
</li></ul>
- [Revision #17b5cb6](https://github.com/MariaDB/server/commit/17b5cb6)
<span class="cstm-style datetime">2016-02-17 22:56:38 -0500</span>
<ul start="1"><li>codership/mysql-wsrep#247: Fix test case
</li></ul>
- [Revision #a6d93b2](https://github.com/MariaDB/server/commit/a6d93b2)
<span class="cstm-style datetime">2016-02-16 23:42:42 -0800</span>
<ul start="1"><li>Galera MTR Tests: MW-246 codership/mysql-wsrep#247 Stability fix for galera.mysql-wsrep#247.test
</li></ul>
- [Revision #2438afb](https://github.com/MariaDB/server/commit/2438afb)
<span class="cstm-style datetime">2016-02-16 03:12:58 -0800</span>
<ul start="1"><li>Galera MTR tests: MW-246 codership/mysql-wsrep#247 Additional tests around RSU and wsrep_desync
</li></ul>
- [Revision #13627d4](https://github.com/MariaDB/server/commit/13627d4)
<span class="cstm-style datetime">2016-02-16 11:55:03 +0200</span>
<ul start="1"><li>refs MW-246 - created mtr test for testing explicit desyncing with RSU mode DDL
</li></ul>
- [Revision #4bdf025](https://github.com/MariaDB/server/commit/4bdf025)
<span class="cstm-style datetime">2016-02-15 23:33:55 +0200</span>
<ul start="1"><li>refs MW-246 - skipping desync and resync before and after DDL execution in RSU mode, if wsrep_desync is set upfront
</li></ul>
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
- [Revision #0c7dffe](https://github.com/MariaDB/server/commit/0c7dffe)
<span class="cstm-style datetime">2015-10-22 17:30:20 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#201
</li></ul>
- [Revision #8a71fde](https://github.com/MariaDB/server/commit/8a71fde)
<span class="cstm-style datetime">2015-10-20 17:54:14 +0200</span>
<ul start="1"><li>refs codership/mysql-wsrep#201
</li></ul>