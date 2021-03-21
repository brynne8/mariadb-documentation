# MariaDB Galera Cluster 10.0.20 Changelog

The most recent [MariaDB Galera Cluster 10.0](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 10.0.38](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10038-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/10.0)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/10.0.20)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10020-release-notes/)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10020-changelog/)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 27 Jun 2015

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10020-release-notes/).

The revision number links will take you to the revision's page on Github. On
Github you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #5467b12](https://github.com/MariaDB/server/commit/5467b12)
<span class="cstm-style datetime">2015-06-24 23:28:42 -0400</span>
<ul start="1"><li>[MDEV-7903](https://jira.mariadb.org/browse/MDEV-7903) : xtrabackup SST failing with maria-10.0-galera
</li></ul>
- [Revision #9f00950](https://github.com/MariaDB/server/commit/9f00950)
<span class="cstm-style datetime">2015-06-24 23:25:22 -0400</span>
<ul start="1"><li>[MDEV-7631](https://jira.mariadb.org/browse/MDEV-7631) : Invalid WSREP_SST rows appear in mysqld-bin.index file
</li></ul>
- [Revision #0f44781](https://github.com/MariaDB/server/commit/0f44781)
<span class="cstm-style datetime">2015-06-24 17:02:33 -0400</span>
<ul start="1"><li>Add close-on-exec flag to open(), socket(), accept() &amp; fopen().
</li></ul>
- [Revision #70714d3](https://github.com/MariaDB/server/commit/70714d3)
<span class="cstm-style datetime">2015-06-23 16:46:12 -0400</span>
<ul start="1"><li>Merge branch '5.5-galera' into 10.0-galera
</li></ul>
- [Revision #71d1f35](https://github.com/MariaDB/server/commit/71d1f35)
<span class="cstm-style datetime">2015-06-23 13:48:39 -0400</span>
<ul start="1"><li>Update SELinux policy to allow UDP for multicast repl in galera.
</li></ul>
- [Revision #4602409](https://github.com/MariaDB/server/commit/4602409)
<span class="cstm-style datetime">2015-06-21 23:54:55 -0400</span>
<ul start="1"><li>Merge tag 'mariadb-10.0.20' into 10.0-galera
</li></ul>
- [Revision #41d4002](https://github.com/MariaDB/server/commit/41d4002)
<span class="cstm-style datetime">2015-06-21 23:09:10 -0400</span>
<ul start="1"><li>Remove duplicate script added due to bad merge.
</li></ul>
- [Revision #3274094](https://github.com/MariaDB/server/commit/3274094)
<span class="cstm-style datetime">2015-06-21 21:50:43 -0400</span>
<ul start="1"><li>Merge tag 'mariadb-5.5.44' into 5.5-galera
</li></ul>
- [Revision #fc716dc](https://github.com/MariaDB/server/commit/fc716dc)
<span class="cstm-style datetime">2015-06-19 19:25:15 -0400</span>
<ul start="1"><li>[MDEV-8260](https://jira.mariadb.org/browse/MDEV-8260) : Issues related to concurrent CTAS
</li></ul>
- [Revision #8c44fd6](https://github.com/MariaDB/server/commit/8c44fd6)
<span class="cstm-style datetime">2015-06-19 00:17:25 -0400</span>
<ul start="1"><li>[MDEV-8239](https://jira.mariadb.org/browse/MDEV-8239) : Idle threads post-execution end up in closing tables state
</li></ul>
- [Revision #6050ab6](https://github.com/MariaDB/server/commit/6050ab6)
<span class="cstm-style datetime">2015-06-18 09:59:09 -0400</span>
<ul start="1"><li>[MDEV-6829](https://jira.mariadb.org/browse/MDEV-6829) : SELinux/AppArmor policies for Galera server
</li></ul>
- [Revision #a6087e7](https://github.com/MariaDB/server/commit/a6087e7)
<span class="cstm-style datetime">2015-06-17 16:13:02 +0200</span>
<ul start="1"><li>[MDEV-5309](https://jira.mariadb.org/browse/MDEV-5309) - RENAME TABLE does not check for existence of the table's engine
</li></ul>
- [Revision #5a4c5fa](https://github.com/MariaDB/server/commit/5a4c5fa)
<span class="cstm-style datetime">2015-06-17 14:18:16 +0200</span>
<ul start="1"><li>[MDEV-5977](https://jira.mariadb.org/browse/MDEV-5977) [MariaDB 10.0](/kb/en/what-is-mariadb-100/) is not installable on Trusty when "trusty-updates universe" is in sources.list
</li></ul>
- [Revision #b56ad49](https://github.com/MariaDB/server/commit/b56ad49)
<span class="cstm-style datetime">2015-06-16 17:27:53 +0200</span>
<ul start="1"><li>[MDEV-8287](https://jira.mariadb.org/browse/MDEV-8287) DROP TABLE suppresses all engine errors
</li></ul>
- [Revision #66fd45a](https://github.com/MariaDB/server/commit/66fd45a)
<span class="cstm-style datetime">2015-06-08 21:06:56 +0200</span>
<ul start="1"><li>[MDEV-7398](https://jira.mariadb.org/browse/MDEV-7398) mysqld segfaults on FreeBSD 10.1 i386 when built with clang 3.4
</li></ul>
- [Revision #7bfda27](https://github.com/MariaDB/server/commit/7bfda27)
<span class="cstm-style datetime">2015-06-16 23:46:22 +0200</span>
<ul start="1"><li>[MDEV-8128](https://jira.mariadb.org/browse/MDEV-8128) cmake fails to detect boost libraries
</li></ul>
- [Revision #26b0cf4](https://github.com/MariaDB/server/commit/26b0cf4)
<span class="cstm-style datetime">2015-06-16 21:18:59 +0200</span>
<ul start="1"><li>[MDEV-8183](https://jira.mariadb.org/browse/MDEV-8183) Adding option mysqldump --no-data-med
</li></ul>
- [Revision #569d2f8](https://github.com/MariaDB/server/commit/569d2f8)
<span class="cstm-style datetime">2015-06-16 23:57:49 +0200</span>
<ul start="1"><li>Merge branch 'connect-10.0' into 10.0
</li></ul>
- [Revision #985e430](https://github.com/MariaDB/server/commit/985e430)
<span class="cstm-style datetime">2015-06-16 23:55:56 +0200</span>
<ul start="1"><li>after-merge fixes
</li></ul>
- [Revision #27f0bd7](https://github.com/MariaDB/server/commit/27f0bd7)
<span class="cstm-style datetime">2015-06-16 17:33:21 +0300</span>
<ul start="1"><li>Fix test case innodb.xa_recovery crash on xtradb.
</li></ul>
- [Revision #9680602](https://github.com/MariaDB/server/commit/9680602)
<span class="cstm-style datetime">2015-06-16 16:20:55 +0300</span>
<ul start="1"><li>Fix test failure on main.partition_innodb.
</li></ul>
- [Revision #ababe04](https://github.com/MariaDB/server/commit/ababe04)
<span class="cstm-style datetime">2015-06-16 15:16:53 +0300</span>
<ul start="1"><li>Fix crash on test innodb.innodb-virtual-columns. We should create only columns really stored to database.
</li></ul>
- [Revision #b83855a](https://github.com/MariaDB/server/commit/b83855a)
<span class="cstm-style datetime">2015-06-16 14:55:21 +0300</span>
<ul start="1"><li>Fix innochecksum build failure.
</li></ul>
- [Revision #5355972](https://github.com/MariaDB/server/commit/5355972)
<span class="cstm-style datetime">2015-06-16 12:49:00 +0200</span>
<ul start="1"><li>after merge fixes: InnoDB and XtraDB
</li></ul>
- [Revision #ede0880](https://github.com/MariaDB/server/commit/ede0880)
<span class="cstm-style datetime">2015-06-16 12:47:58 +0200</span>
<ul start="1"><li>Merge branch 'merge-perfschema-5.6' into 10.0
</li></ul>
- [Revision #9859d36](https://github.com/MariaDB/server/commit/9859d36)
<span class="cstm-style datetime">2015-06-16 12:46:14 +0200</span>
<ul start="1"><li>Merge branch 'merge-xtradb-5.6' into 10.0
</li></ul>
- [Revision #a65162a](https://github.com/MariaDB/server/commit/a65162a)
<span class="cstm-style datetime">2015-06-16 11:08:23 +0200</span>
<ul start="1"><li>Merge branch 'merge-innodb-5.6' into 10.0
</li></ul>
- [Revision #9084945](https://github.com/MariaDB/server/commit/9084945)
<span class="cstm-style datetime">2015-06-16 11:04:40 +0200</span>
<ul start="1"><li>5.6.24-72.2
</li></ul>
- [Revision #3c37249](https://github.com/MariaDB/server/commit/3c37249)
<span class="cstm-style datetime">2015-06-16 11:00:33 +0200</span>
<ul start="1"><li>5.6.25
</li></ul>
- [Revision #139ba26](https://github.com/MariaDB/server/commit/139ba26)
<span class="cstm-style datetime">2015-06-16 10:57:05 +0200</span>
<ul start="1"><li>5.6.25
</li></ul>
- [Revision #909f760](https://github.com/MariaDB/server/commit/909f760)
<span class="cstm-style datetime">2015-06-15 15:37:14 +0400</span>
<ul start="1"><li>[MDEV-5309](https://jira.mariadb.org/browse/MDEV-5309) - RENAME TABLE does not check for existence of the table's engine
</li></ul>
- [Revision #b988553](https://github.com/MariaDB/server/commit/b988553)
<span class="cstm-style datetime">2015-06-15 15:42:14 +0200</span>
<ul start="1"><li>[MDEV-7771](https://jira.mariadb.org/browse/MDEV-7771) missing client plugins when mariadb-shared is not installed
</li></ul>
- [Revision #02421aa](https://github.com/MariaDB/server/commit/02421aa)
<span class="cstm-style datetime">2015-06-15 18:07:41 +0500</span>
<ul start="1"><li>[MDEV-7871](https://jira.mariadb.org/browse/MDEV-7871) Tests fail massively on "Assertion `status_var.memory_used == 0'" when run with --ps --embedded.    As the MF_THREAD_SPECIFIC was introduced to the alloc_root's and    the prealloc added to the statement::mem_root and statement::result.alloc, we have to adjust    the embedded server to it. The preallocation was removed for the embedded server as it    makes no sence for it. The msyqltest should free the statement inside the proper thead to    make the memory statistics happy.
</li></ul>
- [Revision #a117030](https://github.com/MariaDB/server/commit/a117030)
<span class="cstm-style datetime">2015-06-14 18:46:02 +0200</span>
<ul start="1"><li>[MDEV-8131](https://jira.mariadb.org/browse/MDEV-8131) MariaDB does not build on hurd-i386: plugin/auth_dialog/dialog.c:172:20: error: 'RTLD_DEFAULT' undeclared
</li></ul>
- [Revision #3288f26](https://github.com/MariaDB/server/commit/3288f26)
<span class="cstm-style datetime">2015-06-14 20:19:05 +0200</span>
<ul start="1"><li>include the correct IPv6 check in perfschema tests
</li></ul>
- [Revision #2a0f086](https://github.com/MariaDB/server/commit/2a0f086)
<span class="cstm-style datetime">2015-06-14 17:38:30 +0200</span>
<ul start="1"><li>don't scream when auto-selected IPv6 is not available
</li></ul>
- [Revision #a453a28](https://github.com/MariaDB/server/commit/a453a28)
<span class="cstm-style datetime">2015-06-14 17:34:08 +0200</span>
<ul start="1"><li>[MDEV-8083](https://jira.mariadb.org/browse/MDEV-8083) MTR is broken on systems with IPv6 disabled
</li></ul>
- [Revision #aad8667](https://github.com/MariaDB/server/commit/aad8667)
<span class="cstm-style datetime">2015-06-15 11:11:42 +0400</span>
<ul start="1"><li>Committing a change into r/type_time_hires.result forgotten in the previous commit for [MDEV-8205](https://jira.mariadb.org/browse/MDEV-8205).
</li></ul>
- [Revision #43e4522](https://github.com/MariaDB/server/commit/43e4522)
<span class="cstm-style datetime">2015-06-15 11:04:06 +0400</span>
<ul start="1"><li>[MDEV-8205](https://jira.mariadb.org/browse/MDEV-8205) timediff returns null when comparing decimal time to time string value
</li></ul>
- [Revision #f69f3db](https://github.com/MariaDB/server/commit/f69f3db)
<span class="cstm-style datetime">2015-06-15 08:25:09 +0200</span>
<ul start="1"><li>Merge branch 'mdev8294' into 10.0
</li></ul>
- [Revision #4c251af](https://github.com/MariaDB/server/commit/4c251af)
<span class="cstm-style datetime">2015-06-15 08:23:26 +0200</span>
<ul start="1"><li>[MDEV-8316](https://jira.mariadb.org/browse/MDEV-8316): debugger aborting because missing DBUG_RETURN or DBUG_VOID_RETURN macro in function "any_slave_sql_running"
</li></ul>
- [Revision #93c039d](https://github.com/MariaDB/server/commit/93c039d)
<span class="cstm-style datetime">2015-06-15 08:13:40 +0200</span>
<ul start="1"><li>[MDEV-8294](https://jira.mariadb.org/browse/MDEV-8294): Inconsistent behavior of slave parallel threads at runtime
</li></ul>
- [Revision #196528e](https://github.com/MariaDB/server/commit/196528e)
<span class="cstm-style datetime">2015-06-14 18:54:13 +0500</span>
<ul start="1"><li>[MDEV-8212](https://jira.mariadb.org/browse/MDEV-8212) alter table - failing to ADD PRIMARY KEY IF NOT EXISTS when existing index of same as column name.         The default name for the primary key is rather 'PRIMARY' instead of the indexed column name.
</li></ul>
- [Revision #fc31e31](https://github.com/MariaDB/server/commit/fc31e31)
<span class="cstm-style datetime">2015-06-14 17:29:58 +0300</span>
<ul start="1"><li>[MDEV-8179](https://jira.mariadb.org/browse/MDEV-8179): Absent progress report for operations on InnoDB tables
</li></ul>
- [Revision #36bf482](https://github.com/MariaDB/server/commit/36bf482)
<span class="cstm-style datetime">2015-06-14 15:51:34 +0200</span>
<ul start="1"><li>[MDEV-8285](https://jira.mariadb.org/browse/MDEV-8285) compile fails under Mac OS X 10.6.8 due to use of strnlen
</li></ul>
- [Revision #e2879ac](https://github.com/MariaDB/server/commit/e2879ac)
<span class="cstm-style datetime">2015-06-14 08:14:28 +0300</span>
<ul start="1"><li>[MDEV-7881](https://jira.mariadb.org/browse/MDEV-7881): InnoDB Logfile size - misleading error message
</li></ul>
- [Revision #e85b661](https://github.com/MariaDB/server/commit/e85b661)
<span class="cstm-style datetime">2015-06-12 08:00:48 +0200</span>
<ul start="1"><li>Merge branch 'bb-10.0-serg' into 10.0
</li></ul>
- [Revision #d437c35](https://github.com/MariaDB/server/commit/d437c35)
<span class="cstm-style datetime">2015-06-11 22:54:03 +0400</span>
<ul start="1"><li>Adding a few warning related protected methods in Field and reducing some duplicate code.
</li></ul>
- [Revision #b9eb7f1](https://github.com/MariaDB/server/commit/b9eb7f1)
<span class="cstm-style datetime">2015-06-11 20:20:52 +0200</span>
<ul start="1"><li>CRLF
</li></ul>
- [Revision #6d49d3b](https://github.com/MariaDB/server/commit/6d49d3b)
<span class="cstm-style datetime">2015-06-11 20:20:45 +0200</span>
<ul start="1"><li>compiler warnings
</li></ul>
- [Revision #810cf36](https://github.com/MariaDB/server/commit/810cf36)
<span class="cstm-style datetime">2015-06-11 20:20:35 +0200</span>
<ul start="1"><li>Merge branch '5.5' into 10.0
</li></ul>
- [Revision #d199a0f](https://github.com/MariaDB/server/commit/d199a0f)
<span class="cstm-style datetime">2015-06-11 17:47:52 +0200</span>
<ul start="1"><li>more renames after tokudb merge
</li></ul>
- [Revision #b96c196](https://github.com/MariaDB/server/commit/b96c196)
<span class="cstm-style datetime">2015-06-11 16:48:10 +0200</span>
<ul start="1"><li>Item_cache::safe_charset_converter() fixes
</li></ul>
- [Revision #7c98e8a](https://github.com/MariaDB/server/commit/7c98e8a)
<span class="cstm-style datetime">2015-06-11 16:43:56 +0200</span>
<ul start="1"><li>fix after the tokudb ft-index merge
</li></ul>
- [Revision #36f37a4](https://github.com/MariaDB/server/commit/36f37a4)
<span class="cstm-style datetime">2015-06-10 12:01:06 +0200</span>
<ul start="1"><li>Merge [MDEV-8294](https://jira.mariadb.org/browse/MDEV-8294) into 10.0
</li></ul>
- [Revision #682ed00](https://github.com/MariaDB/server/commit/682ed00)
<span class="cstm-style datetime">2015-06-10 11:57:42 +0200</span>
<ul start="1"><li>[MDEV-8294](https://jira.mariadb.org/browse/MDEV-8294): Inconsistent behavior of slave parallel threads at runtime
</li></ul>
- [Revision #5a44e1a](https://github.com/MariaDB/server/commit/5a44e1a)
<span class="cstm-style datetime">2015-06-09 22:11:22 +0200</span>
<ul start="1"><li>tests for [MDEV-7937](https://jira.mariadb.org/browse/MDEV-7937): Enforce SSL when --ssl client option is used
</li></ul>
- [Revision #80f6b22](https://github.com/MariaDB/server/commit/80f6b22)
<span class="cstm-style datetime">2015-06-09 16:08:09 +0400</span>
<ul start="1"><li>[MDEV-3870](https://jira.mariadb.org/browse/MDEV-3870) - Valgrind warnings on OPTIMIZE MyISAM or Aria TABLE with disabled             keys
</li></ul>
- [Revision #3a50a8c](https://github.com/MariaDB/server/commit/3a50a8c)
<span class="cstm-style datetime">2015-06-09 13:50:43 +0400</span>
<ul start="1"><li>[MDEV-363](https://jira.mariadb.org/browse/MDEV-363) - Server crashes in intern_plugin_lock on concurrent installing            semisync plugin and setting rpl_semi_sync_master_enabled
</li></ul>
- [Revision #49a3392](https://github.com/MariaDB/server/commit/49a3392)
<span class="cstm-style datetime">2015-06-09 11:57:31 +0400</span>
<ul start="1"><li>[MDEV-363](https://jira.mariadb.org/browse/MDEV-363) - Server crashes in intern_plugin_lock on concurrent installing            semisync plugin and setting rpl_semi_sync_master_enabled
</li></ul>
- [Revision #e5005ce](https://github.com/MariaDB/server/commit/e5005ce)
<span class="cstm-style datetime">2015-06-09 18:06:41 +0200</span>
<ul start="1"><li>disable ssl for ssl-disabled tests
</li></ul>
- [Revision #992d782](https://github.com/MariaDB/server/commit/992d782)
<span class="cstm-style datetime">2015-06-09 18:56:09 +0300</span>
<ul start="1"><li>[MDEV-6735](https://jira.mariadb.org/browse/MDEV-6735): Range checked for each record used with key (also [MDEV-7786](https://jira.mariadb.org/browse/MDEV-7786), [MDEV-7923](https://jira.mariadb.org/browse/MDEV-7923))
</li></ul>
- [Revision #5d57e2d](https://github.com/MariaDB/server/commit/5d57e2d)
<span class="cstm-style datetime">2015-06-09 16:46:45 +0300</span>
<ul start="1"><li>Fix tests for 7937
</li></ul>
- [Revision #be5035b](https://github.com/MariaDB/server/commit/be5035b)
<span class="cstm-style datetime">2015-06-09 15:59:29 +0300</span>
<ul start="1"><li>Added tests for [MDEV-7937](https://jira.mariadb.org/browse/MDEV-7937)
</li></ul>
- [Revision #4ef7497](https://github.com/MariaDB/server/commit/4ef7497)
<span class="cstm-style datetime">2015-06-09 14:08:44 +0300</span>
<ul start="1"><li>[MDEV-7937](https://jira.mariadb.org/browse/MDEV-7937): Enforce SSL when --ssl client option is used
</li></ul>
- [Revision #56e2d83](https://github.com/MariaDB/server/commit/56e2d83)
<span class="cstm-style datetime">2015-05-02 08:45:10 +0200</span>
<ul start="1"><li>[MDEV-7695](https://jira.mariadb.org/browse/MDEV-7695) MariaDB - ssl - fips: can not connect with --ssl-cipher=DHE-RSA-AES256-SHA - handshake failure
</li></ul>
- [Revision #92b3659](https://github.com/MariaDB/server/commit/92b3659)
<span class="cstm-style datetime">2015-06-09 12:05:06 +0400</span>
<ul start="1"><li>[MDEV-7268](https://jira.mariadb.org/browse/MDEV-7268) Column of table cannot be converted from type 'decimal(0,?)' to type ' 'decimal(10,7)' Changing the error message to:  "...from type 'decimal(0,?)/*old*/' to type ' 'decimal(10,7)'..." So it's now clear that the master data type is OLD decimal.
</li></ul>
- [Revision #b1e1039](https://github.com/MariaDB/server/commit/b1e1039)
<span class="cstm-style datetime">2015-06-09 07:36:24 +0400</span>
<ul start="1"><li>[MDEV-8286](https://jira.mariadb.org/browse/MDEV-8286) Likely a redundant declaration of Item_cache::used_table_map
</li></ul>
- [Revision #a4d93e0](https://github.com/MariaDB/server/commit/a4d93e0)
<span class="cstm-style datetime">2015-06-05 20:05:08 +0200</span>
<ul start="1"><li>[MDEV-8050](https://jira.mariadb.org/browse/MDEV-8050) sphinx test cases cannot run with sphinxsearch-2.2.6
</li></ul>
- [Revision #b41ad55](https://github.com/MariaDB/server/commit/b41ad55)
<span class="cstm-style datetime">2015-06-08 15:09:20 +0200</span>
<ul start="1"><li>update tokudb version
</li></ul>
- [Revision #1707cfc](https://github.com/MariaDB/server/commit/1707cfc)
<span class="cstm-style datetime">2015-06-08 21:55:52 +0500</span>
<ul start="1"><li>[MDEV-8211](https://jira.mariadb.org/browse/MDEV-8211) plugins.server_audit fails sporadically in buildbot.    More fixes to assure the order of queries in the log.
</li></ul>
- [Revision #87088b9](https://github.com/MariaDB/server/commit/87088b9)
<span class="cstm-style datetime">2015-06-08 21:44:13 +0500</span>
<ul start="1"><li>[MDEV-8211](https://jira.mariadb.org/browse/MDEV-8211) plugins.server_audit fails sporadically in buildbot.   This test also should be fixed - delay added so the connection   event doesn't happen before the query.
</li></ul>
- [Revision #96b3703](https://github.com/MariaDB/server/commit/96b3703)
<span class="cstm-style datetime">2015-06-08 21:40:17 +0500</span>
<ul start="1"><li>[MDEV-8211](https://jira.mariadb.org/browse/MDEV-8211) plugins.server_audit fails sporadically in buildbot.    Connection event can happen before the query ends. Added a delay to    confirm the order.
</li></ul>
- [Revision #a765cca](https://github.com/MariaDB/server/commit/a765cca)
<span class="cstm-style datetime">2015-06-08 20:50:40 +0400</span>
<ul start="1"><li>[MDEV-8067](https://jira.mariadb.org/browse/MDEV-8067) correct fix for MySQL Bug # 19699237: UNINITIALIZED VARIABLE IN ITEM_FIELD::STR_RESULT
</li></ul>
- [Revision #b37b52a](https://github.com/MariaDB/server/commit/b37b52a)
<span class="cstm-style datetime">2015-06-08 13:47:07 +0500</span>
<ul start="1"><li>[MDEV-4922](https://jira.mariadb.org/browse/MDEV-4922) Stored Procedure - Geometry parameter not working.   Fhe GEOMETRY field should be handled just as the BLOB field. So that was fiexed in field_conv.   One additional bug was found and fixed meanwhile - thet the geometry field subtypes   should also be merged for UNION command.
</li></ul>
- [Revision #69ed429](https://github.com/MariaDB/server/commit/69ed429)
<span class="cstm-style datetime">2015-06-08 12:09:13 +0500</span>
<ul start="1"><li>[MDEV-7500](https://jira.mariadb.org/browse/MDEV-7500) thread_handling option in my.cnf is not passing "connect events" to audit plugin.   The MYSQL_AUDIT_NOTIFY_CONNECTION_CONNECT() call moved to the login_connection()   function. So that it'll be invoked in any thread handling mode.
</li></ul>
- [Revision #1ae05db](https://github.com/MariaDB/server/commit/1ae05db)
<span class="cstm-style datetime">2015-06-07 15:40:42 +0500</span>
<ul start="1"><li>[MDEV-8078](https://jira.mariadb.org/browse/MDEV-8078) Memory disclosure/buffer overread on audit plugin.         If the SET PASSWORD query doesn't have the password string,         the parsing of it can fail. It manifested first in MySQL 5.6 as         it started to hide password lines sent to the plugins.         Fixed by checking for that case.
</li></ul>
- [Revision #db0ecf2](https://github.com/MariaDB/server/commit/db0ecf2)
<span class="cstm-style datetime">2015-06-06 19:12:44 +0500</span>
<ul start="1"><li>[MDEV-8032](https://jira.mariadb.org/browse/MDEV-8032) [PATCH] audit plugin - csv output broken.    Symbols like TAB or NEWLINE should be escaped, which was         forgotten in one place.
</li></ul>
- [Revision #6264451](https://github.com/MariaDB/server/commit/6264451)
<span class="cstm-style datetime">2015-06-06 16:13:51 +0200</span>
<ul start="1"><li>[MDEV-8114](https://jira.mariadb.org/browse/MDEV-8114): server crash on updates with joins still on 10.0.18
</li></ul>
- [Revision #1443227](https://github.com/MariaDB/server/commit/1443227)
<span class="cstm-style datetime">2015-06-05 16:20:40 +0200</span>
<ul start="1"><li>Merge branch 'ob-10.0' into 10.0
</li></ul>
- [Revision #4728b51](https://github.com/MariaDB/server/commit/4728b51)
<span class="cstm-style datetime">2015-06-05 16:20:23 +0200</span>
<ul start="1"><li>Commit win packaging &amp; upgrade_wizard files
</li></ul>
- [Revision #88998cf](https://github.com/MariaDB/server/commit/88998cf)
<span class="cstm-style datetime">2015-06-05 16:10:50 +0200</span>
<ul start="1"><li>Commit merge resolved files
</li></ul>
- [Revision #a8b7b6c](https://github.com/MariaDB/server/commit/a8b7b6c)
<span class="cstm-style datetime">2015-06-05 10:59:15 +0200</span>
<ul start="1"><li>Commit merge resolved files
</li></ul>
- [Revision #9a3b975](https://github.com/MariaDB/server/commit/9a3b975)
<span class="cstm-style datetime">2015-06-05 09:51:17 +0200</span>
<ul start="1"><li>Merge branch '5.5' into bb-5.5-serg
</li></ul>
- [Revision #a2bb9d2](https://github.com/MariaDB/server/commit/a2bb9d2)
<span class="cstm-style datetime">2015-06-04 16:04:05 +0400</span>
<ul start="1"><li>[MDEV-7505](https://jira.mariadb.org/browse/MDEV-7505) - Too large scale in DECIMAL dynamic column getter crashes mysqld
</li></ul>
- [Revision #b611ac0](https://github.com/MariaDB/server/commit/b611ac0)
<span class="cstm-style datetime">2015-06-03 14:30:09 +0400</span>
<ul start="1"><li>[MDEV-6236](https://jira.mariadb.org/browse/MDEV-6236) - [PATCH] mysql_tzinfo_to_sql may produce invalid SQL
</li></ul>
- [Revision #af2256f](https://github.com/MariaDB/server/commit/af2256f)
<span class="cstm-style datetime">2015-06-03 13:59:58 +0400</span>
<ul start="1"><li>[MDEV-7207](https://jira.mariadb.org/browse/MDEV-7207) - ALTER VIEW does not change ALGORITM
</li></ul>
- [Revision #ae0c576](https://github.com/MariaDB/server/commit/ae0c576)
<span class="cstm-style datetime">2015-06-05 02:14:49 +0200</span>
<ul start="1"><li>Merge branch 'merge/merge-xtradb-5.5' into bb-5.5-serg
</li></ul>
- [Revision #f84f577](https://github.com/MariaDB/server/commit/f84f577)
<span class="cstm-style datetime">2015-06-05 02:06:51 +0200</span>
<ul start="1"><li>Merge tag 'mysql-5.5.44' into bb-5.5-serg
</li></ul>
- [Revision #f07b346](https://github.com/MariaDB/server/commit/f07b346)
<span class="cstm-style datetime">2015-06-05 02:04:32 +0200</span>
<ul start="1"><li>do not re-populate I_S tables in subqueries
</li></ul>
- [Revision #1ff423d](https://github.com/MariaDB/server/commit/1ff423d)
<span class="cstm-style datetime">2015-06-04 21:12:29 +0400</span>
<ul start="1"><li>[MDEV-8243](https://jira.mariadb.org/browse/MDEV-8243) configure defines to empty string, not 1
</li></ul>
- [Revision #750aa8b](https://github.com/MariaDB/server/commit/750aa8b)
<span class="cstm-style datetime">2015-06-04 18:58:12 +0200</span>
<ul start="1"><li>5.5.43-37.2
</li></ul>
- [Revision #980bdc3](https://github.com/MariaDB/server/commit/980bdc3)
<span class="cstm-style datetime">2015-06-04 17:39:05 +0200</span>
<ul start="1"><li>followup: CREATE SERVER tests should not be run for embedded
</li></ul>
- [Revision #a477cd1](https://github.com/MariaDB/server/commit/a477cd1)
<span class="cstm-style datetime">2015-06-03 23:31:05 +0300</span>
<ul start="1"><li>[MDEV-6500](https://jira.mariadb.org/browse/MDEV-6500): Stale data returned after TRUNCATE PARTITION operation
</li></ul>
- [Revision #08fa02c](https://github.com/MariaDB/server/commit/08fa02c)
<span class="cstm-style datetime">2015-06-04 18:51:30 +0400</span>
<ul start="1"><li>Some MYD files (e.g. in mysql-test/std_data) could erroneously be treated by git as text files.
</li></ul>
- [Revision #9da8a8f](https://github.com/MariaDB/server/commit/9da8a8f)
<span class="cstm-style datetime">2015-06-04 18:49:12 +0400</span>
<ul start="1"><li>[MDEV-7269](https://jira.mariadb.org/browse/MDEV-7269) mysqlbinlog Don't know how to handle column type=0 meta=0 (0000)# [MDEV-8267](https://jira.mariadb.org/browse/MDEV-8267) Add /*old*/ comment into I_S.COLUMN_TYPE for old DECIMAL
</li></ul>
- [Revision #a8b8544](https://github.com/MariaDB/server/commit/a8b8544)
<span class="cstm-style datetime">2015-06-04 13:00:53 +0300</span>
<ul start="1"><li>[MDEV-7906](https://jira.mariadb.org/browse/MDEV-7906): InnoDB: Failing assertion: prebuilt-&gt;sql_stat_start || trx-&gt;state == 1 on concurrent multi-table update
</li></ul>
- [Revision #7b05650](https://github.com/MariaDB/server/commit/7b05650)
<span class="cstm-style datetime">2015-06-03 20:24:51 +0200</span>
<ul start="1"><li>Merge tag 'tokudb-engine/tokudb-7.5.7' into 5.5
</li></ul>
- [Revision #e500c47](https://github.com/MariaDB/server/commit/e500c47)
<span class="cstm-style datetime">2015-06-03 19:47:46 +0200</span>
<ul start="1"><li>Merge tag 'tokudb-ft-index/tokudb-7.5.7' into 5.5
</li></ul>
- [Revision #934a18d](https://github.com/MariaDB/server/commit/934a18d)
<span class="cstm-style datetime">2015-06-03 19:42:34 +0200</span>
<ul start="1"><li>.gitattributes: *.dat files should not be CRLF converted
</li></ul>
- [Revision #c79e98e](https://github.com/MariaDB/server/commit/c79e98e)
<span class="cstm-style datetime">2015-06-03 18:45:08 +0200</span>
<ul start="1"><li>[MDEV-8085](https://jira.mariadb.org/browse/MDEV-8085) main.group_by failed in buildbot
</li></ul>
- [Revision #0599d80](https://github.com/MariaDB/server/commit/0599d80)
<span class="cstm-style datetime">2015-06-03 17:37:51 +0200</span>
<ul start="1"><li>Fix swapping key numeric values on Big Endian machines. Fix typo error in CntIndexRange (kp instead of p) Change version date   
</li></ul>
- [Revision #5d8cee4](https://github.com/MariaDB/server/commit/5d8cee4)
<span class="cstm-style datetime">2015-06-03 17:11:07 +0200</span>
<ul start="1"><li>[MDEV-8224](https://jira.mariadb.org/browse/MDEV-8224) Server crashes in get_server_from_table_to_cache on empty name
</li></ul>
- [Revision #33d480f](https://github.com/MariaDB/server/commit/33d480f)
<span class="cstm-style datetime">2015-06-03 16:33:10 +0200</span>
<ul start="1"><li>[MDEV-4608](https://jira.mariadb.org/browse/MDEV-4608) deb packages for jessie
</li></ul>
- [Revision #f806b4d](https://github.com/MariaDB/server/commit/f806b4d)
<span class="cstm-style datetime">2015-06-03 12:13:43 +0200</span>
<ul start="1"><li>[MDEV-8124](https://jira.mariadb.org/browse/MDEV-8124) mysqlcheck: --auto-repair runs REPAIR TABLE instead of REPAIR VIEW on views
</li></ul>
- [Revision #535b514](https://github.com/MariaDB/server/commit/535b514)
<span class="cstm-style datetime">2015-06-03 10:35:34 +0200</span>
<ul start="1"><li>[MDEV-8123](https://jira.mariadb.org/browse/MDEV-8123) mysqlcheck: new --process-views option conflicts with --quick, --extended and such
</li></ul>
- [Revision #64569fa](https://github.com/MariaDB/server/commit/64569fa)
<span class="cstm-style datetime">2015-06-03 11:11:53 +0200</span>
<ul start="1"><li>parser: better error messages for CHECK/REPAIR VIEW
</li></ul>
- [Revision #4821cd9](https://github.com/MariaDB/server/commit/4821cd9)
<span class="cstm-style datetime">2015-06-03 11:38:34 +0200</span>
<ul start="1"><li>Fix swapping key numeric values on Big Endian machines. Fix typo error in CntIndexRange for big endian swapping   
</li></ul>
- [Revision #37a803c](https://github.com/MariaDB/server/commit/37a803c)
<span class="cstm-style datetime">2015-06-03 11:31:18 +0200</span>
<ul start="1"><li>Fix swapping key numeric values on Big Endian machines. Swap the key length when WORDS_BIGENDIAN is defined Make the IOFF structure depending on WORDS_BIGENDIAN   
</li></ul>
- [Revision #65ed254](https://github.com/MariaDB/server/commit/65ed254)
<span class="cstm-style datetime">2015-06-03 10:07:33 +0200</span>
<ul start="1"><li>Fix swapping key numeric values on Big Endian machines. Change the preprocessor variable used from BIG_ENDIAN_ORDER (only used by taoscript) to WORDS_BIGENDIAN.   
</li></ul>
- [Revision #0ffef5d](https://github.com/MariaDB/server/commit/0ffef5d)
<span class="cstm-style datetime">2015-06-03 09:54:56 +0200</span>
<ul start="1"><li>[MDEV-8052](https://jira.mariadb.org/browse/MDEV-8052) abi detection incorrect with clang
</li></ul>
- [Revision #70d8030](https://github.com/MariaDB/server/commit/70d8030)
<span class="cstm-style datetime">2015-06-03 02:02:21 +0200</span>
<ul start="1"><li>Fix swapping key numeric values on Big Endian machines.   
</li></ul>
- [Revision #8e7d665](https://github.com/MariaDB/server/commit/8e7d665)
<span class="cstm-style datetime">2015-06-02 22:07:47 +0200</span>
<ul start="1"><li>CRLF-&gt;LF
</li></ul>
- [Revision #6d5b723](https://github.com/MariaDB/server/commit/6d5b723)
<span class="cstm-style datetime">2015-06-02 13:27:39 -0400</span>
<ul start="1"><li>Merge branch '5.5-galera' into 10.0-galera
</li></ul>
- [Revision #af26c36](https://github.com/MariaDB/server/commit/af26c36)
<span class="cstm-style datetime">2015-06-02 12:28:42 +0200</span>
<ul start="1"><li>Fix handling of NULL values when reading from tables.   
</li></ul>
- [Revision #514a7d8](https://github.com/MariaDB/server/commit/514a7d8)
<span class="cstm-style datetime">2015-05-30 10:59:34 +0200</span>
<ul start="1"><li>Add unicode ODBC types to the types recognized by CONNECT. Was added in function TranslateSQLType.   
</li></ul>
- [Revision #b6a5637](https://github.com/MariaDB/server/commit/b6a5637)
<span class="cstm-style datetime">2015-05-27 16:23:38 +0200</span>
<ul start="1"><li>Change all preprocessor compiler directives to use <u>WIN</u> as the mean of specifying Windows or not Windows compile. This is what MariaDB does.   
</li></ul>
- [Revision #6bd76f8](https://github.com/MariaDB/server/commit/6bd76f8)
<span class="cstm-style datetime">2015-05-27 10:27:18 +0400</span>
<ul start="1"><li>Merge pull request #73 from akopytov/[MDEV-7658](https://jira.mariadb.org/browse/MDEV-7658)-5.5
</li></ul>
- [Revision #a99efc0](https://github.com/MariaDB/server/commit/a99efc0)
<span class="cstm-style datetime">2015-05-27 09:16:24 +0300</span>
<ul start="1"><li>Merge pull request #74 from akopytov/[MDEV-7658](https://jira.mariadb.org/browse/MDEV-7658)-10.0
</li></ul>
- [Revision #7f7cee8](https://github.com/MariaDB/server/commit/7f7cee8)
<span class="cstm-style datetime">2015-05-26 23:58:51 +0300</span>
<ul start="1"><li>Merge branch '[MDEV-7658](https://jira.mariadb.org/browse/MDEV-7658)-5.5' into [MDEV-7658](https://jira.mariadb.org/browse/MDEV-7658)-10.0
</li></ul>
- [Revision #70bc0a3](https://github.com/MariaDB/server/commit/70bc0a3)
<span class="cstm-style datetime">2015-05-26 23:56:00 +0300</span>
<ul start="1"><li>Fixes [MDEV-7658](https://jira.mariadb.org/browse/MDEV-7658): [MDEV-7026](https://jira.mariadb.org/browse/MDEV-7026) fix reintroduces [MDEV-6615](https://jira.mariadb.org/browse/MDEV-6615) on AArch64
</li></ul>
- [Revision #f738598](https://github.com/MariaDB/server/commit/f738598)
<span class="cstm-style datetime">2015-05-26 13:15:57 +0200</span>
<ul start="1"><li>Merge [MDEV-8147](https://jira.mariadb.org/browse/MDEV-8147) into 10.0
</li></ul>
- [Revision #e5f1e84](https://github.com/MariaDB/server/commit/e5f1e84)
<span class="cstm-style datetime">2015-05-26 12:47:35 +0200</span>
<ul start="1"><li>[MDEV-8147](https://jira.mariadb.org/browse/MDEV-8147): Assertion `m_lock_type == 2' failed in handler::ha_close() during parallel replication
</li></ul>
- [Revision #fb98632](https://github.com/MariaDB/server/commit/fb98632)
<span class="cstm-style datetime">2015-05-26 01:02:33 +0200</span>
<ul start="1"><li>JSONColumns and XMLColumns revisited. They can retrieve their parameters directly from the PTOS argument. For this to work, finding the table options is now split in HA_CONNECT functions and exported functions available from out of ha_connect.   
</li></ul>
- [Revision #9eff9ed](https://github.com/MariaDB/server/commit/9eff9ed)
<span class="cstm-style datetime">2015-05-24 13:30:49 -0400</span>
<ul start="1"><li>[MDEV-8208](https://jira.mariadb.org/browse/MDEV-8208) : Sporadic SEGFAULT on startup
</li></ul>
- [Revision #37840d5](https://github.com/MariaDB/server/commit/37840d5)
<span class="cstm-style datetime">2015-05-20 11:19:44 +0200</span>
<ul start="1"><li>Security: EOM modules must now be loaded from the plugin directory.   
</li></ul>
- [Revision #db33294](https://github.com/MariaDB/server/commit/db33294)
<span class="cstm-style datetime">2015-05-17 19:55:48 +0200</span>
<ul start="1"><li>Json array index (position) was badly set for default array setting   
</li></ul>
- [Revision #a82171c](https://github.com/MariaDB/server/commit/a82171c)
<span class="cstm-style datetime">2015-05-17 15:22:42 +0200</span>
<ul start="1"><li>In BIN table date_format now imply by default field_format='C'.   
</li></ul>
- [Revision #0bfae35](https://github.com/MariaDB/server/commit/0bfae35)
<span class="cstm-style datetime">2015-05-16 11:11:26 -0400</span>
<ul start="1"><li>[MDEV-8166](https://jira.mariadb.org/browse/MDEV-8166) : Adding index on new table from select crashes Galera cluster
</li></ul>
- [Revision #5d02928](https://github.com/MariaDB/server/commit/5d02928)
<span class="cstm-style datetime">2015-05-16 10:26:34 +0200</span>
<ul start="1"><li>remove second @ from CONFIGURE_FILE (... @ONLY@)
</li></ul>
- [Revision #b9c9109](https://github.com/MariaDB/server/commit/b9c9109)
<span class="cstm-style datetime">2015-05-15 11:56:29 +0200</span>
<ul start="1"><li>Fix a bug in BIN buffer initialisation (in FIXFAM::AllocateBuffer)   
</li></ul>
- [Revision #46199c0](https://github.com/MariaDB/server/commit/46199c0)
<span class="cstm-style datetime">2015-05-14 14:43:37 +0400</span>
<ul start="1"><li>[MDEV-8102](https://jira.mariadb.org/browse/MDEV-8102) REGEXP function fails to match hex values when expression is stored as a variable We don't fix the bug itself, we just make regex functions display errors returned from pcre_exec() as MariaDB warnings.
</li></ul>
- [Revision #e6b60ee](https://github.com/MariaDB/server/commit/e6b60ee)
<span class="cstm-style datetime">2015-05-13 19:58:21 +0200</span>
<ul start="1"><li>Make BIN table files more flexible with new column format. In particular enable to set length and endian setting. This should solve all problems on IBM390s machines.   
</li></ul>
- [Revision #0b4231e](https://github.com/MariaDB/server/commit/0b4231e)
<span class="cstm-style datetime">2015-05-13 15:17:19 +0300</span>
<ul start="1"><li>[MDEV-8154](https://jira.mariadb.org/browse/MDEV-8154) rpl.show_status_stop_slave_race-7126 sporadically causes internal check failure
</li></ul>
- [Revision #6f8558b](https://github.com/MariaDB/server/commit/6f8558b)
<span class="cstm-style datetime">2015-05-12 14:19:30 -0400</span>
<ul start="1"><li>Fix for debug build failure
</li></ul>
- [Revision #b3d3dd2](https://github.com/MariaDB/server/commit/b3d3dd2)
<span class="cstm-style datetime">2015-05-12 03:44:10 +0300</span>
<ul start="1"><li>[MDEV-8144](https://jira.mariadb.org/browse/MDEV-8144) percona.innodb_sys_index test fails
</li></ul>
- [Revision #f027baf](https://github.com/MariaDB/server/commit/f027baf)
<span class="cstm-style datetime">2015-05-12 03:43:36 +0300</span>
<ul start="1"><li>Increase the version number
</li></ul>
- [Revision #3810fefc](https://github.com/MariaDB/server/commit/3810fefc)
<span class="cstm-style datetime">2015-05-10 12:40:30 +0200</span>
<ul start="1"><li>resolving conflict
</li></ul>
- [Revision #445fc77](https://github.com/MariaDB/server/commit/445fc77)
<span class="cstm-style datetime">2015-05-10 12:14:21 +0200</span>
<ul start="1"><li>Get rid of more GCC warnings about unused parameters   
</li></ul>
- [Revision #7c02f74](https://github.com/MariaDB/server/commit/7c02f74)
<span class="cstm-style datetime">2015-05-10 12:22:43 +0200</span>
<ul start="1"><li>Get rid of more GCC warnings about unused parameters   storage/connect/colblk.cpp
</li></ul>
- [Revision #3b3f6ca](https://github.com/MariaDB/server/commit/3b3f6ca)
<span class="cstm-style datetime">2015-05-08 13:21:42 +0200</span>
<ul start="1"><li>Typo to check buildbot
</li></ul>
- [Revision #c63bd86](https://github.com/MariaDB/server/commit/c63bd86)
<span class="cstm-style datetime">2015-05-10 12:14:21 +0200</span>
<ul start="1"><li>Get rid of more GCC warnings about unused parameters   
</li></ul>
- [Revision #f5d0c77](https://github.com/MariaDB/server/commit/f5d0c77)
<span class="cstm-style datetime">2015-05-09 17:30:20 +0200</span>
<ul start="1"><li>Get rid of GCC warnings about unused parameters   
</li></ul>
- [Revision #373d092](https://github.com/MariaDB/server/commit/373d092)
<span class="cstm-style datetime">2015-05-08 17:19:48 +0300</span>
<ul start="1"><li>Fix win/ files to be stored with LF in repository
</li></ul>
- [Revision #23b2b95](https://github.com/MariaDB/server/commit/23b2b95)
<span class="cstm-style datetime">2015-05-08 17:19:06 +0300</span>
<ul start="1"><li>Update .gitattributes
</li></ul>
- [Revision #6ef3c7d](https://github.com/MariaDB/server/commit/6ef3c7d)
<span class="cstm-style datetime">2015-05-08 17:09:45 +0300</span>
<ul start="1"><li>Updated .gitattributes
</li></ul>
- [Revision #6b56e89](https://github.com/MariaDB/server/commit/6b56e89)
<span class="cstm-style datetime">2015-05-08 13:21:42 +0200</span>
<ul start="1"><li>Typo to check buildbot
</li></ul>
- [Revision #5fa2a6c](https://github.com/MariaDB/server/commit/5fa2a6c)
<span class="cstm-style datetime">2015-05-07 18:02:31 +0200</span>
<ul start="1"><li>Merge branch 'ob-10.0' into 10.0
</li></ul>
- [Revision #7704fde](https://github.com/MariaDB/server/commit/7704fde)
<span class="cstm-style datetime">2015-05-07 18:01:49 +0200</span>
<ul start="1"><li>Heidi stuff
</li></ul>
- [Revision #c387e7d](https://github.com/MariaDB/server/commit/c387e7d)
<span class="cstm-style datetime">2015-05-07 17:36:25 +0200</span>
<ul start="1"><li>Heidi stuff
</li></ul>
- [Revision #3a889b1](https://github.com/MariaDB/server/commit/3a889b1)
<span class="cstm-style datetime">2015-05-07 16:59:25 +0200</span>
<ul start="1"><li>Fix a bug in init_table_share that caused syntax error with Boolean options:   oom|= sql-&gt;append(vull ? "ON" : "OFF");   replaced by:   oom|= sql-&gt;append(vull ? "YES" : "NO"); 
</li></ul>
- [Revision #12bebce](https://github.com/MariaDB/server/commit/12bebce)
<span class="cstm-style datetime">2015-05-05 11:37:21 +0200</span>
<ul start="1"><li>- Fix a regression bug on (XML) HTML tables. 
</li></ul>
- [Revision #1b07ba5](https://github.com/MariaDB/server/commit/1b07ba5)
<span class="cstm-style datetime">2015-05-02 15:36:33 +0200</span>
<ul start="1"><li>Fix [MDEV-8090](https://jira.mariadb.org/browse/MDEV-8090) in tabmysql.cpp
</li></ul>