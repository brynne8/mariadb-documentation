# MariaDB Galera Cluster 10.0.30 Changelog

The most recent [MariaDB Galera Cluster 10.0](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 10.0.38](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10038-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/10.0)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/10.0.30)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10030-release-notes/)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10030-changelog/)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 22 Mar 2017

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10030-release-notes/).
For changes made in MariaDB, see the [MariaDB 10.0.30 Changelog](/kb/en/mariadb-10030-changelog/).

The revision number links will take you to the revision's page on Github. On
Github you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #656d0f10e5](https://github.com/MariaDB/server/commit/656d0f10e5)
<span class="cstm-style datetime">2017-03-21 14:23:45 +0530</span>
<ul start="1"><li>Fix galera_admin test
</li></ul>
- [Revision #b22026ddfd](https://github.com/MariaDB/server/commit/b22026ddfd)
<span class="cstm-style datetime">2017-03-21 10:00:02 +0200</span>
<ul start="1"><li>Fix failure on galera_toi_drop_database test.
</li></ul>
- [Revision #0759c9baf5](https://github.com/MariaDB/server/commit/0759c9baf5)
<span class="cstm-style datetime">2017-03-21 12:14:19 +0530</span>
<ul start="1"><li>Fix mysqlhotcopy test failures
</li></ul>
- [Revision #0b51dee0e3](https://github.com/MariaDB/server/commit/0b51dee0e3)
<span class="cstm-style datetime">2017-03-20 18:52:18 +0530</span>
<ul start="1"><li>Change VERSION no to 30
</li></ul>
- <span class="cstm-style datetime">[Revision #9cf499724f](https://github.com/MariaDB/server/commit/9cf499724f) 2017-03-20 18:11:56 +0530 - Merge branch '10.0' into bb-10.0-galera</span>
- [Revision #4c35dce296](https://github.com/MariaDB/server/commit/4c35dce296)
<span class="cstm-style datetime">2017-03-18 22:50:14 +0200</span>
<ul start="1"><li>Clean up the test mentioned in [MDEV-12052](https://jira.mariadb.org/browse/MDEV-12052).
</li></ul>
- [Revision #8971286a3c](https://github.com/MariaDB/server/commit/8971286a3c)
<span class="cstm-style datetime">2017-03-16 14:03:17 +0100</span>
<ul start="1"><li>compiler warning
</li></ul>
- [Revision #2d0c579a86](https://github.com/MariaDB/server/commit/2d0c579a86)
<span class="cstm-style datetime">2017-03-06 16:25:01 +0200</span>
<ul start="1"><li>Wait for slave threads to start during startup
</li></ul>
- [Revision #e7f55fde88](https://github.com/MariaDB/server/commit/e7f55fde88)
<span class="cstm-style datetime">2017-03-06 16:02:50 +0200</span>
<ul start="1"><li>Removed wrong assert
</li></ul>
- [Revision #2c2bd8c155](https://github.com/MariaDB/server/commit/2c2bd8c155)
<span class="cstm-style datetime">2017-03-15 11:46:54 +0100</span>
<ul start="1"><li>[MDEV-12261](https://jira.mariadb.org/browse/MDEV-12261) build failure without P_S
</li></ul>
- [Revision #06f1f1aa6e](https://github.com/MariaDB/server/commit/06f1f1aa6e)
<span class="cstm-style datetime">2017-03-14 00:24:06 +0200</span>
<ul start="1"><li>Make ELOOP be considered a File Not Found error when it comes from handlerton
</li></ul>
- [Revision #032678ad18](https://github.com/MariaDB/server/commit/032678ad18)
<span class="cstm-style datetime">2017-03-10 18:33:38 +0200</span>
<ul start="1"><li>[MDEV-12091](https://jira.mariadb.org/browse/MDEV-12091) Shutdown fails to wait for rollback of recovered transactions to finish
</li></ul>
- [Revision #1d47bd61d5](https://github.com/MariaDB/server/commit/1d47bd61d5)
<span class="cstm-style datetime">2017-03-09 16:52:57 +0200</span>
<ul start="1"><li>Remove leftover merge conflict marker
</li></ul>
- [Revision #1b2b209519](https://github.com/MariaDB/server/commit/1b2b209519)
<span class="cstm-style datetime">2017-03-09 11:28:07 +0200</span>
<ul start="1"><li>Use correct integer format with printf-like functions.
</li></ul>
- [Revision #8805fe0d5c](https://github.com/MariaDB/server/commit/8805fe0d5c)
<span class="cstm-style datetime">2017-03-09 11:27:24 +0200</span>
<ul start="1"><li>Use %pure-parser instead of the deprecated %pure_parser.
</li></ul>
- [Revision #2158de8865](https://github.com/MariaDB/server/commit/2158de8865)
<span class="cstm-style datetime">2017-03-09 11:26:36 +0200</span>
<ul start="1"><li>Remove unused variables.
</li></ul>
- [Revision #9fe92a9770](https://github.com/MariaDB/server/commit/9fe92a9770)
<span class="cstm-style datetime">2017-03-08 11:13:34 -0500</span>
<ul start="1"><li>bump the VERSION
</li></ul>
- [Revision #e1e04c3d68](https://github.com/MariaDB/server/commit/e1e04c3d68)
<span class="cstm-style datetime">2017-03-08 14:40:02 +0200</span>
<ul start="1"><li>Correct a merge error.
</li></ul>
- <span class="cstm-style datetime">[Revision #fc0a6dd57f](https://github.com/MariaDB/server/commit/fc0a6dd57f) 2017-03-08 12:21:13 +0200 - Merge branch '5.5' into 10.0</span>
- [Revision #f65c9f825d](https://github.com/MariaDB/server/commit/f65c9f825d)
<span class="cstm-style datetime">2017-03-07 15:52:17 +0200</span>
<ul start="1"><li>mysql_client_test_nonblock fails when compiled with clang
</li></ul>
- [Revision #74fe0e03d5](https://github.com/MariaDB/server/commit/74fe0e03d5)
<span class="cstm-style datetime">2017-03-08 11:46:34 +0200</span>
<ul start="1"><li>Remove unused declarations.
</li></ul>
- <span class="cstm-style datetime">[Revision #47396ddea9](https://github.com/MariaDB/server/commit/47396ddea9) 2017-03-08 11:40:43 +0200 - Merge 5.5 into 10.0</span>
- [Revision #6860a4b556](https://github.com/MariaDB/server/commit/6860a4b556)
<span class="cstm-style datetime">2017-03-08 10:31:06 +0200</span>
<ul start="1"><li>[MDEV-12206](https://jira.mariadb.org/browse/MDEV-12206) Query_cache::send_result_to_client() may corrupt THD::query_plan_flags
</li></ul>
- [Revision #9c47beb8bd](https://github.com/MariaDB/server/commit/9c47beb8bd)
<span class="cstm-style datetime">2017-03-08 10:07:50 +0200</span>
<ul start="1"><li>[MDEV-11027](https://jira.mariadb.org/browse/MDEV-11027) InnoDB log recovery is too noisy
</li></ul>
- [Revision #1fd3cc8c1f](https://github.com/MariaDB/server/commit/1fd3cc8c1f)
<span class="cstm-style datetime">2017-03-08 10:06:34 +0200</span>
<ul start="1"><li>Fix a compiler warning.
</li></ul>
- [Revision #17a1b194e2](https://github.com/MariaDB/server/commit/17a1b194e2)
<span class="cstm-style datetime">2017-03-08 10:03:35 +0200</span>
<ul start="1"><li>Fix some GCC 6.3.0 warnings in MyISAM and Maria.
</li></ul>
- [Revision #30cac41c2f](https://github.com/MariaDB/server/commit/30cac41c2f)
<span class="cstm-style datetime">2017-03-06 23:07:59 +0400</span>
<ul start="1"><li>[MDEV-11084](https://jira.mariadb.org/browse/MDEV-11084) server_audit does not work with mysql_community 5.7.16.
</li></ul>
- [Revision #43903745e5](https://github.com/MariaDB/server/commit/43903745e5)
<span class="cstm-style datetime">2017-03-05 10:58:05 +0530</span>
<ul start="1"><li>[MDEV-11078](https://jira.mariadb.org/browse/MDEV-11078): NULL NOT IN (non-empty subquery) should never return results
</li></ul>
- [Revision #6b8173b6e9](https://github.com/MariaDB/server/commit/6b8173b6e9)
<span class="cstm-style datetime">2017-03-03 11:47:31 +0200</span>
<ul start="1"><li>[MDEV-11520](https://jira.mariadb.org/browse/MDEV-11520): Retry posix_fallocate() after EINTR.
</li></ul>
- [Revision #75f6067e89](https://github.com/MariaDB/server/commit/75f6067e89)
<span class="cstm-style datetime">2017-02-28 17:39:28 +0100</span>
<ul start="1"><li>[MDEV-9635](https://jira.mariadb.org/browse/MDEV-9635): Server crashes in part_of_refkey  or assertion `!created &amp;&amp; key_to_save &lt; (int)s-&gt;keys' failed in TABLE::use_index(int) or with join_cache_level&gt;2
</li></ul>
- [Revision #53c6195eed](https://github.com/MariaDB/server/commit/53c6195eed)
<span class="cstm-style datetime">2017-03-20 10:17:13 +0200</span>
<ul start="1"><li>Fixed test failure on galere_wsrep_log_conflicts on XtraDB.
</li></ul>
- <span class="cstm-style datetime">[Revision #f66395f7c0](https://github.com/MariaDB/server/commit/f66395f7c0) 2017-03-17 02:05:20 +0530 - Merge tag 'mariadb-10.0.30' into bb-sachin-10.0-galera-merge</span>
- [Revision #c401773c8d](https://github.com/MariaDB/server/commit/c401773c8d)
<span class="cstm-style datetime">2017-03-16 08:07:58 +0530</span>
<ul start="1"><li>Fix test cases
</li></ul>
- [Revision #5bb7653667](https://github.com/MariaDB/server/commit/5bb7653667)
<span class="cstm-style datetime">2017-03-16 02:13:31 +0530</span>
<ul start="1"><li>Fix wsrep_affected_rows.
</li></ul>
- [Revision #1743d68868](https://github.com/MariaDB/server/commit/1743d68868)
<span class="cstm-style datetime">2017-03-14 18:41:38 +0530</span>
<ul start="1"><li>Fix Some failing tests
</li></ul>
- [Revision #25070d2a2c](https://github.com/MariaDB/server/commit/25070d2a2c)
<span class="cstm-style datetime">2017-01-25 09:58:07 +0200</span>
<ul start="1"><li>Bump WSREP_PATCH_VERSION to 19
</li></ul>
- [Revision #ad7b00fb90](https://github.com/MariaDB/server/commit/ad7b00fb90)
<span class="cstm-style datetime">2017-03-14 16:17:28 +0530</span>
<ul start="1"><li>Galera MTR Tests: do not run innodb.innodb_stats_del_mark and some other tests with Galera, as it produces warnings
</li></ul>
- [Revision #69b5bd7ae3](https://github.com/MariaDB/server/commit/69b5bd7ae3)
<span class="cstm-style datetime">2016-12-16 02:30:09 -0800</span>
<ul start="1"><li>Galera MTR Tests: Tests for MW-328 Fix unnecessary/silent BF aborts
</li></ul>
- [Revision #dd2f023427](https://github.com/MariaDB/server/commit/dd2f023427)
<span class="cstm-style datetime">2016-12-15 01:22:44 -0800</span>
<ul start="1"><li>Galera MTR Tests: restore galera_autoinc_sst_xtrabackup.test to use xtrabackup SST
</li></ul>
- [Revision #5ac0d5fc24](https://github.com/MariaDB/server/commit/5ac0d5fc24)
<span class="cstm-style datetime">2016-12-08 05:15:31 -0800</span>
<ul start="1"><li>Galera MTR Tests: Stability fix for MW-329
</li></ul>
- [Revision #17f716062d](https://github.com/MariaDB/server/commit/17f716062d)
<span class="cstm-style datetime">2016-12-08 00:17:28 -0800</span>
<ul start="1"><li>Galera MTR Tests: Test for MW-329 Fix incorrect affected rows count after replay
</li></ul>
- [Revision #00f1ed6655](https://github.com/MariaDB/server/commit/00f1ed6655)
<span class="cstm-style datetime">2017-03-14 14:42:52 +0530</span>
<ul start="1"><li>Galera MTR Tests: fix variable output in galera_as_slave_gtid_replicate_do_db.result
</li></ul>
- [Revision #f29c40d0a5](https://github.com/MariaDB/server/commit/f29c40d0a5)
<span class="cstm-style datetime">2017-03-14 14:31:13 +0530</span>
<ul start="1"><li>[GAL-480](https://github.com/codership/galera/issues/480) MTR test
</li></ul>
- [Revision #6fabf12ba0](https://github.com/MariaDB/server/commit/6fabf12ba0)
<span class="cstm-style datetime">2016-11-23 02:55:36 -0800</span>
<ul start="1"><li>Galera MTR Test: Test for MW-28 : Assertion with --wsrep-log-conflicts
</li></ul>
- [Revision #0e0ae0bb06](https://github.com/MariaDB/server/commit/0e0ae0bb06)
<span class="cstm-style datetime">2017-03-14 13:13:22 +0530</span>
<ul start="1"><li>MW-28, codership/mysql-wsrep#28 Fix sync_thread_levels debug assert
</li></ul>
- [Revision #471dd11381](https://github.com/MariaDB/server/commit/471dd11381)
<span class="cstm-style datetime">2016-11-21 10:38:20 +0200</span>
<ul start="1"><li>refs: MW-319 * silenced the WSREP_ERROR, this fires for all replication filtered DDL,   and is false positive
</li></ul>
- [Revision #c49bfff992](https://github.com/MariaDB/server/commit/c49bfff992)
<span class="cstm-style datetime">2017-03-14 11:21:16 +0530</span>
<ul start="1"><li>Galera MTR tests: Make the mysqlhotcopy tests pass on Ubuntu 16.04
</li></ul>
- [Revision #e29d7b1d0b](https://github.com/MariaDB/server/commit/e29d7b1d0b)
<span class="cstm-style datetime">2016-11-08 15:19:37 +0200</span>
<ul start="1"><li>Bump WSREP_PATCH_VERSION to 18
</li></ul>
- [Revision #f78332c581](https://github.com/MariaDB/server/commit/f78332c581)
<span class="cstm-style datetime">2016-11-05 09:15:14 -0700</span>
<ul start="1"><li>Galera MTR Tests: stability fix for galera#414.test
</li></ul>
- [Revision #64bb59fce9](https://github.com/MariaDB/server/commit/64bb59fce9)
<span class="cstm-style datetime">2017-03-14 11:19:03 +0530</span>
<ul start="1"><li>Galera MTR Tests: stability fixes
</li></ul>
- [Revision #451bf7243a](https://github.com/MariaDB/server/commit/451bf7243a)
<span class="cstm-style datetime">2016-11-04 01:33:54 -0700</span>
<ul start="1"><li>Galera MTR Tests: Test for MW-313 Enforce wsrep_max_ws_rows also when binlog is enabled
</li></ul>
- [Revision #9dda6cb08d](https://github.com/MariaDB/server/commit/9dda6cb08d)
<span class="cstm-style datetime">2016-11-03 16:04:22 +0100</span>
<ul start="1"><li>MW-313 Enforce wsrep_max_ws_rows also when binlog is enabled
</li></ul>
- [Revision #395c420f0f](https://github.com/MariaDB/server/commit/395c420f0f)
<span class="cstm-style datetime">2016-11-03 04:05:05 -0700</span>
<ul start="1"><li>Galera MTR Tests: MW-305 , re-enable the test for ALTER USER
</li></ul>
- [Revision #0a06347333](https://github.com/MariaDB/server/commit/0a06347333)
<span class="cstm-style datetime">2016-11-03 00:49:20 -0700</span>
<ul start="1"><li>Galera MTR Tests: Test for MW-309 - Fix wsrep_max_ws_rows so that it does not affect SELECT queries
</li></ul>
- [Revision #5d9c747193](https://github.com/MariaDB/server/commit/5d9c747193)
<span class="cstm-style datetime">2016-11-02 07:04:34 -0700</span>
<ul start="1"><li>Galera MTR tests: Update galera_defaults.result for [GAL-360](https://github.com/codership/galera/issues/360)
</li></ul>
- [Revision #16e683fdad](https://github.com/MariaDB/server/commit/16e683fdad)
<span class="cstm-style datetime">2017-03-14 07:11:17 +0530</span>
<ul start="1"><li>Galera MTR Tests: Tests for [GAL-419](https://github.com/codership/galera/issues/419) Respect safe_to_bootstrap flag also with gcomm:<em>
</em></li></ul>
- [Revision #108fd77486](https://github.com/MariaDB/server/commit/108fd77486)
<span class="cstm-style datetime">2016-11-02 02:01:10 -0700</span>
<ul start="1"><li>Galera MTR Tests: [GAL-405](https://github.com/codership/galera/issues/405) Initial implementation of GCache recovery on startup.
</li></ul>
- [Revision #9be994ba69](https://github.com/MariaDB/server/commit/9be994ba69)
<span class="cstm-style datetime">2017-03-13 05:30:02 +0530</span>
<ul start="1"><li>MW-309 Fix wsrep_max_ws_rows so that it does not affect queries
</li></ul>
- [Revision #4573924f7d](https://github.com/MariaDB/server/commit/4573924f7d)
<span class="cstm-style datetime">2017-03-12 23:00:20 +0530</span>
<ul start="1"><li>Galera MTR Tests: MW-308 , MW-307, GCF-992
</li></ul>
- [Revision #c2eaae268d](https://github.com/MariaDB/server/commit/c2eaae268d)
<span class="cstm-style datetime">2016-10-11 03:37:42 -0700</span>
<ul start="1"><li>Galera MTR Tests: GCF-981 - galera_bf_abort is non deterministic
</li></ul>
- [Revision #0e105bc1f2](https://github.com/MariaDB/server/commit/0e105bc1f2)
<span class="cstm-style datetime">2016-10-03 03:09:49 -0700</span>
<ul start="1"><li>Galera MTR Tests: Test for GCF-942 - safe_to_bootstrap flag in grastate.dat
</li></ul>
- [Revision #15298689cb](https://github.com/MariaDB/server/commit/15298689cb)
<span class="cstm-style datetime">2016-09-14 14:33:59 +0300</span>
<ul start="1"><li>Bump WSREP_PATCH_VERSION to 17
</li></ul>
- [Revision #86ec6c221a](https://github.com/MariaDB/server/commit/86ec6c221a)
<span class="cstm-style datetime">2017-03-12 13:56:29 +0530</span>
<ul start="1"><li>MW-267: followup to the original pull request, removed unnecessary cast.
</li></ul>
- [Revision #3045b60f0f](https://github.com/MariaDB/server/commit/3045b60f0f)
<span class="cstm-style datetime">2016-08-20 13:42:11 +0200</span>
<ul start="1"><li>[GAL-401](https://github.com/codership/galera/issues/401): MTR test for the fix.</li></ul>