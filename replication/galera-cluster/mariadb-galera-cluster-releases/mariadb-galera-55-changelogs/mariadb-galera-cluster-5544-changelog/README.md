# MariaDB Galera Cluster 5.5.44 Changelog

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.44)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5544-release-notes)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5544-changelog)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 27 Jun 2015

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5544-release-notes).

The revision number links will take you to the revision's page on Github. On
Github you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #71d1f35](https://github.com/MariaDB/server/commit/71d1f35)
<span class="cstm-style datetime">2015-06-23 13:48:39 -0400</span>
<ul start="1"><li>Update SELinux policy to allow UDP for multicast repl in galera.
</li></ul>
- [Revision #3274094](https://github.com/MariaDB/server/commit/3274094)
<span class="cstm-style datetime">2015-06-21 21:50:43 -0400</span>
<ul start="1"><li>Merge tag 'mariadb-5.5.44' into 5.5-galera
</li></ul>
- [Revision #fc716dc](https://github.com/MariaDB/server/commit/fc716dc)
<span class="cstm-style datetime">2015-06-19 19:25:15 -0400</span>
<ul start="1"><li>[MDEV-8260](https://jira.mariadb.org/browse/MDEV-8260) : Issues related to concurrent CTAS
</li></ul>
- [Revision #6050ab6](https://github.com/MariaDB/server/commit/6050ab6)
<span class="cstm-style datetime">2015-06-18 09:59:09 -0400</span>
<ul start="1"><li>[MDEV-6829](https://jira.mariadb.org/browse/MDEV-6829) : SELinux/AppArmor policies for Galera server
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
- [Revision #0ffef5d](https://github.com/MariaDB/server/commit/0ffef5d)
<span class="cstm-style datetime">2015-06-03 09:54:56 +0200</span>
<ul start="1"><li>[MDEV-8052](https://jira.mariadb.org/browse/MDEV-8052) abi detection incorrect with clang
</li></ul>
- [Revision #8e7d665](https://github.com/MariaDB/server/commit/8e7d665)
<span class="cstm-style datetime">2015-06-02 22:07:47 +0200</span>
<ul start="1"><li>CRLF-&gt;LF
</li></ul>
- [Revision #6bd76f8](https://github.com/MariaDB/server/commit/6bd76f8)
<span class="cstm-style datetime">2015-05-27 10:27:18 +0400</span>
<ul start="1"><li>Merge pull request #73 from akopytov/[MDEV-7658](https://jira.mariadb.org/browse/MDEV-7658)-5.5
</li></ul>
- [Revision #70bc0a3](https://github.com/MariaDB/server/commit/70bc0a3)
<span class="cstm-style datetime">2015-05-26 23:56:00 +0300</span>
<ul start="1"><li>Fixes [MDEV-7658](https://jira.mariadb.org/browse/MDEV-7658): [MDEV-7026](https://jira.mariadb.org/browse/MDEV-7026) fix reintroduces [MDEV-6615](https://jira.mariadb.org/browse/MDEV-6615) on AArch64
</li></ul>
- [Revision #9eff9ed](https://github.com/MariaDB/server/commit/9eff9ed)
<span class="cstm-style datetime">2015-05-24 13:30:49 -0400</span>
<ul start="1"><li>[MDEV-8208](https://jira.mariadb.org/browse/MDEV-8208) : Sporadic SEGFAULT on startup
</li></ul>
- [Revision #0bfae35](https://github.com/MariaDB/server/commit/0bfae35)
<span class="cstm-style datetime">2015-05-16 11:11:26 -0400</span>
<ul start="1"><li>[MDEV-8166](https://jira.mariadb.org/browse/MDEV-8166) : Adding index on new table from select crashes Galera cluster
</li></ul>
- [Revision #5d02928](https://github.com/MariaDB/server/commit/5d02928)
<span class="cstm-style datetime">2015-05-16 10:26:34 +0200</span>
<ul start="1"><li>remove second @ from CONFIGURE_FILE (... @ONLY@)
</li></ul>
- [Revision #8fdf8f0](https://github.com/MariaDB/server/commit/8fdf8f0)
<span class="cstm-style datetime">2015-05-14 14:25:02 -0400</span>
<ul start="1"><li>mysql-wsrep#38 : wsrep_sst_xtrabackup-v2 script causes innobackupex to
</li></ul>
- [Revision #6f8558b](https://github.com/MariaDB/server/commit/6f8558b)
<span class="cstm-style datetime">2015-05-12 14:19:30 -0400</span>
<ul start="1"><li>Fix for debug build failure
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
- [Revision #0014bdc](https://github.com/MariaDB/server/commit/0014bdc)
<span class="cstm-style datetime">2015-05-07 22:18:34 +0200</span>
<ul start="1"><li>[MDEV-8115](https://jira.mariadb.org/browse/MDEV-8115) mysql_upgrade crashes the server with REPAIR VIEW
</li></ul>
- [Revision #8350ea0](https://github.com/MariaDB/server/commit/8350ea0)
<span class="cstm-style datetime">2015-05-07 13:04:03 +0300</span>
<ul start="1"><li>Fix compiler error if compiler does not support c99 style initializers.
</li></ul>
- [Revision #f704b33](https://github.com/MariaDB/server/commit/f704b33)
<span class="cstm-style datetime">2015-05-06 16:47:23 +0300</span>
<ul start="1"><li>Merge pull request #52 from openquery/[MDEV-8053](https://jira.mariadb.org/browse/MDEV-8053)-c99-style-for-structure-members
</li></ul>
- [Revision #4d606cb](https://github.com/MariaDB/server/commit/4d606cb)
<span class="cstm-style datetime">2015-04-24 23:17:16 +1000</span>
<ul start="1"><li>c99 style for assigning structure members
</li></ul>