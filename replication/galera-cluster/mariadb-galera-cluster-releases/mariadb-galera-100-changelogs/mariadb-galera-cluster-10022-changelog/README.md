# MariaDB Galera Cluster 10.0.22 Changelog

The most recent [MariaDB Galera Cluster 10.0](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 10.0.38](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10038-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/10.0)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/10.0.22)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10022-release-notes)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10022-changelog)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 19 Nov 2015

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10022-release-notes).

The revision number links will take you to the revision's page on Github. On
Github you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #f4421c8](https://github.com/MariaDB/server/commit/f4421c8)
<span class="cstm-style datetime">2015-11-16 12:39:56 -0500</span>
<ul start="1"><li>Fix for some failing tests.
</li></ul>
- [Revision #c78fc8b](https://github.com/MariaDB/server/commit/c78fc8b)
<span class="cstm-style datetime">2015-11-16 12:35:06 -0500</span>
<ul start="1"><li>MTR: rsync process is left running if pid file is removed
</li></ul>
- [Revision #dcb7996](https://github.com/MariaDB/server/commit/dcb7996)
<span class="cstm-style datetime">2015-11-15 17:24:47 -0500</span>
<ul start="1"><li>Fix/disable failing tests.
</li></ul>
- [Revision #e947a52](https://github.com/MariaDB/server/commit/e947a52)
<span class="cstm-style datetime">2015-11-04 21:58:07 -0500</span>
<ul start="1"><li>Update global_suppressions.
</li></ul>
- [Revision #2399f1a](https://github.com/MariaDB/server/commit/2399f1a)
<span class="cstm-style datetime">2015-11-04 21:56:46 -0500</span>
<ul start="1"><li>Fix for build failure.
</li></ul>
- [Revision #4d15112](https://github.com/MariaDB/server/commit/4d15112)
<span class="cstm-style datetime">2015-10-31 18:07:02 -0400</span>
<ul start="1"><li>Merge tag 'mariadb-10.0.22' into 10.0-galera
</li></ul>
- [Revision #d775ecd](https://github.com/MariaDB/server/commit/d775ecd)
<span class="cstm-style datetime">2015-10-28 08:11:54 +0100</span>
<ul start="1"><li>[MDEV-8543](https://jira.mariadb.org/browse/MDEV-8543) mysql.server script not correctly handle --pid-file.
</li></ul>
- [Revision #d88aaaa](https://github.com/MariaDB/server/commit/d88aaaa)
<span class="cstm-style datetime">2015-10-28 08:34:08 +0100</span>
<ul start="1"><li>[MDEV-8525](https://jira.mariadb.org/browse/MDEV-8525) [mariadb 10.0.20](/kb/en/mariadb-10020-release-notes/) crashing when data is read by Kodi media center ([http://kodi.tv](http://kodi.tv)).
</li></ul>
- [Revision #b0e3f48](https://github.com/MariaDB/server/commit/b0e3f48)
<span class="cstm-style datetime">2015-10-22 16:08:45 +0200</span>
<ul start="1"><li>[MDEV-8756](https://jira.mariadb.org/browse/MDEV-8756) [MariaDB 10.0.21](/kb/en/mariadb-10021-release-notes/) crashes during PREPARE
</li></ul>
- [Revision #ac67f9a](https://github.com/MariaDB/server/commit/ac67f9a)
<span class="cstm-style datetime">2015-10-28 12:53:23 +0400</span>
<ul start="1"><li>Removed mistakenly committed test file.
</li></ul>
- [Revision #ce1b450](https://github.com/MariaDB/server/commit/ce1b450)
<span class="cstm-style datetime">2015-10-22 13:57:44 +0200</span>
<ul start="1"><li>[MDEV-7930](https://jira.mariadb.org/browse/MDEV-7930) Assertion `table_share-&gt;tmp_table != NO_TMP_TABLE || m_lock_type != 2' failed in  handler::ha_index_read_map
</li></ul>
- [Revision #e1ed331](https://github.com/MariaDB/server/commit/e1ed331)
<span class="cstm-style datetime">2014-07-01 16:52:35 +0530</span>
<ul start="1"><li>[MDEV-8805](https://jira.mariadb.org/browse/MDEV-8805) - Assertion `!m_ordered_rec_buffer' failed in             ha_partition::init_record_priority_queue()
</li></ul>
- [Revision #4834d82](https://github.com/MariaDB/server/commit/4834d82)
<span class="cstm-style datetime">2015-10-28 08:42:51 +0200</span>
<ul start="1"><li>[MDEV-8932](https://jira.mariadb.org/browse/MDEV-8932): innodb buffer pool hit rate is less than zero
</li></ul>
- [Revision #a9b5a8d](https://github.com/MariaDB/server/commit/a9b5a8d)
<span class="cstm-style datetime">2015-10-28 00:08:18 +0100</span>
<ul start="1"><li>Merge branch 'bb-10.0-serg' into 10.0
</li></ul>
- [Revision #3c5733c](https://github.com/MariaDB/server/commit/3c5733c)
<span class="cstm-style datetime">2015-10-27 18:57:28 +0100</span>
<ul start="1"><li>Merge branch 'connect/10.0' into 10.0
</li></ul>
- [Revision #13884cf](https://github.com/MariaDB/server/commit/13884cf)
<span class="cstm-style datetime">2015-10-27 13:00:15 +0200</span>
<ul start="1"><li>[MDEV-8696](https://jira.mariadb.org/browse/MDEV-8696): Adding indexes on empty table is slow with large innodb_sort_buffer_size.
</li></ul>
- [Revision #d6480f4](https://github.com/MariaDB/server/commit/d6480f4)
<span class="cstm-style datetime">2015-10-11 10:32:44 +0200</span>
<ul start="1"><li>Fixed Fedora 22 package build failure.
</li></ul>
- [Revision #e4f9d20](https://github.com/MariaDB/server/commit/e4f9d20)
<span class="cstm-style datetime">2015-10-23 15:06:43 +0400</span>
<ul start="1"><li>[MDEV-8498](https://jira.mariadb.org/browse/MDEV-8498) - mysql_secure_installation can't find "mysql" in basedir
</li></ul>
- [Revision #c918522](https://github.com/MariaDB/server/commit/c918522)
<span class="cstm-style datetime">2015-10-25 22:45:48 +0400</span>
<ul start="1"><li>[MDEV-8358](https://jira.mariadb.org/browse/MDEV-8358) ALTER TABLE .. ADD PRIMARY KEY IF NOT EXISTS -&gt; ERROR 1068 (42000): Multiple primary key defined         Checks for multiple primary keys added.
</li></ul>
- [Revision #6a28882](https://github.com/MariaDB/server/commit/6a28882)
<span class="cstm-style datetime">2015-10-24 20:06:59 +0200</span>
<ul start="1"><li>merge commit 02b00b154
</li></ul>
- [Revision #84da154](https://github.com/MariaDB/server/commit/84da154)
<span class="cstm-style datetime">2015-10-23 22:21:50 +0200</span>
<ul start="1"><li>[MDEV-8883](https://jira.mariadb.org/browse/MDEV-8883) more cross-compiling fixes
</li></ul>
- [Revision #fb87133](https://github.com/MariaDB/server/commit/fb87133)
<span class="cstm-style datetime">2015-10-23 11:31:18 +0200</span>
<ul start="1"><li>remove unneded #include's that had a dubious explanation
</li></ul>
- [Revision #2c0bcff](https://github.com/MariaDB/server/commit/2c0bcff)
<span class="cstm-style datetime">2015-10-24 20:16:06 +0400</span>
<ul start="1"><li>[MDEV-8693](https://jira.mariadb.org/browse/MDEV-8693) Tests connect.bin connect.endian fail on armhf (on Debian build system)
</li></ul>
- [Revision #d546d1c](https://github.com/MariaDB/server/commit/d546d1c)
<span class="cstm-style datetime">2015-10-23 18:49:02 +0300</span>
<ul start="1"><li>Fixed [MDEV-8408](https://jira.mariadb.org/browse/MDEV-8408) Assertion `inited==INDEX' failed in int handler::ha_index_first(uchar*)
</li></ul>
- [Revision #df8832c](https://github.com/MariaDB/server/commit/df8832c)
<span class="cstm-style datetime">2015-10-22 15:23:18 +0200</span>
<ul start="1"><li>[MDEV-8883](https://jira.mariadb.org/browse/MDEV-8883) more cross-compiling fixes
</li></ul>
- [Revision #581d852](https://github.com/MariaDB/server/commit/581d852)
<span class="cstm-style datetime">2015-10-22 13:55:55 +0200</span>
<ul start="1"><li>[MDEV-8868](https://jira.mariadb.org/browse/MDEV-8868) Consider adding a check for libjemalloc version in cmake and/or at runtime
</li></ul>
- [Revision #6f07547](https://github.com/MariaDB/server/commit/6f07547)
<span class="cstm-style datetime">2015-10-22 13:09:38 +0200</span>
<ul start="1"><li>[MDEV-8614](https://jira.mariadb.org/browse/MDEV-8614) Assertion `status == 0' failed in add_role_user_mapping_action on RENAME USER
</li></ul>
- [Revision #956e92d](https://github.com/MariaDB/server/commit/956e92d)
<span class="cstm-style datetime">2015-10-22 11:58:54 +0200</span>
<ul start="1"><li>[MDEV-8609](https://jira.mariadb.org/browse/MDEV-8609) Server crashes in is_invalid_role_name on reloading ACL with a blank role name
</li></ul>
- [Revision #27328ca](https://github.com/MariaDB/server/commit/27328ca)
<span class="cstm-style datetime">2015-10-22 10:27:36 +0200</span>
<ul start="1"><li>add comment to a test
</li></ul>
- [Revision #3e1c743](https://github.com/MariaDB/server/commit/3e1c743)
<span class="cstm-style datetime">2015-10-22 07:23:59 +0200</span>
<ul start="1"><li>[MDEV-7656](https://jira.mariadb.org/browse/MDEV-7656) init_file option does not allow changing passwords
</li></ul>
- [Revision #e257b8b](https://github.com/MariaDB/server/commit/e257b8b)
<span class="cstm-style datetime">2015-10-21 16:22:20 +0200</span>
<ul start="1"><li>fix the dbug tag name
</li></ul>
- [Revision #e5cce2b](https://github.com/MariaDB/server/commit/e5cce2b)
<span class="cstm-style datetime">2015-10-22 07:15:23 +0200</span>
<ul start="1"><li>fix build on sol10-64
</li></ul>
- [Revision #41a3c58](https://github.com/MariaDB/server/commit/41a3c58)
<span class="cstm-style datetime">2015-10-21 19:40:38 +0200</span>
<ul start="1"><li>Merge branch 'ob-10.0' into 10.0
</li></ul>
- [Revision #b35f997](https://github.com/MariaDB/server/commit/b35f997)
<span class="cstm-style datetime">2015-10-21 19:24:01 +0200</span>
<ul start="1"><li>Fix [MDEV-8882](https://jira.mariadb.org/browse/MDEV-8882)   modified:   storage/connect/tabodbc.cpp
</li></ul>
- [Revision #18f7dfe](https://github.com/MariaDB/server/commit/18f7dfe)
<span class="cstm-style datetime">2015-10-15 12:11:17 +0300</span>
<ul start="1"><li>Allow mysql_upgrade to enable event after table is corrected
</li></ul>
- [Revision #95faf34](https://github.com/MariaDB/server/commit/95faf34)
<span class="cstm-style datetime">2015-10-12 13:05:31 +0300</span>
<ul start="1"><li>Set opt_noacl (running with--skip-grant-tables) to 0 if we reload grant tables.
</li></ul>
- [Revision #0d90b8b](https://github.com/MariaDB/server/commit/0d90b8b)
<span class="cstm-style datetime">2015-10-21 14:59:36 +0200</span>
<ul start="1"><li>Merge branch '5.5' into 10.0
</li></ul>
- [Revision #df80420](https://github.com/MariaDB/server/commit/df80420)
<span class="cstm-style datetime">2015-10-21 14:42:56 +0200</span>
<ul start="1"><li>fix events_1 test for October 2015
</li></ul>
- [Revision #17b0b45](https://github.com/MariaDB/server/commit/17b0b45)
<span class="cstm-style datetime">2015-10-21 09:20:54 +0300</span>
<ul start="1"><li>Code cleanup.
</li></ul>
- [Revision #ac9141c](https://github.com/MariaDB/server/commit/ac9141c)
<span class="cstm-style datetime">2015-10-20 18:45:45 +0200</span>
<ul start="1"><li>Merge branch 'ob-10.0' into 10.0
</li></ul>
- [Revision #d51e466](https://github.com/MariaDB/server/commit/d51e466)
<span class="cstm-style datetime">2015-10-20 13:20:10 +0200</span>
<ul start="1"><li>Fix [MDEV-8966](https://jira.mariadb.org/browse/MDEV-8966)   modified:   storage/connect/ha_connect.cc
</li></ul>
- [Revision #f3e3624](https://github.com/MariaDB/server/commit/f3e3624)
<span class="cstm-style datetime">2015-10-20 13:41:14 +0300</span>
<ul start="1"><li>[MDEV-8869](https://jira.mariadb.org/browse/MDEV-8869): Potential lock_sys-&gt;mutex deadlock
</li></ul>
- [Revision #76701c6](https://github.com/MariaDB/server/commit/76701c6)
<span class="cstm-style datetime">2015-10-19 14:17:36 +0300</span>
<ul start="1"><li>Merge pull request #105 from philip-galera/10.0-galera-mysql-wsrep-gh202
</li></ul>
- [Revision #52a9103](https://github.com/MariaDB/server/commit/52a9103)
<span class="cstm-style datetime">2015-10-19 04:14:51 -0700</span>
<ul start="1"><li>refs codership/mysql-wsrep#202 Fix bad cherry-pick (and the compiler warnings it generated)
</li></ul>
- [Revision #9a3ff07](https://github.com/MariaDB/server/commit/9a3ff07)
<span class="cstm-style datetime">2015-10-19 12:15:49 +0200</span>
<ul start="1"><li>[MDEV-8565](https://jira.mariadb.org/browse/MDEV-8565): COLUMN_CHECK fails on valid data
</li></ul>
- [Revision #3ec139a](https://github.com/MariaDB/server/commit/3ec139a)
<span class="cstm-style datetime">2015-10-19 12:05:25 +0300</span>
<ul start="1"><li>Merge pull request #104 from philip-galera/10.0-galera-mysql-wsrep-gh202
</li></ul>
- [Revision #43b2a45](https://github.com/MariaDB/server/commit/43b2a45)
<span class="cstm-style datetime">2015-10-19 01:56:04 -0700</span>
<ul start="1"><li>refs codership/mysql-wsrep#202 Added schema info into wsrep messages
</li></ul>
- [Revision #f515422](https://github.com/MariaDB/server/commit/f515422)
<span class="cstm-style datetime">2015-10-18 15:06:14 +0200</span>
<ul start="1"><li>Merge branch 'ob-10.0' into 10.0
</li></ul>
- [Revision #9020946](https://github.com/MariaDB/server/commit/9020946)
<span class="cstm-style datetime">2015-10-18 15:03:45 +0200</span>
<ul start="1"><li>Fix [MDEV-8926](https://jira.mariadb.org/browse/MDEV-8926)   modified:   storage/connect/ha_connect.cc
</li></ul>
- [Revision #978c2a3](https://github.com/MariaDB/server/commit/978c2a3)
<span class="cstm-style datetime">2015-10-11 17:06:03 -0400</span>
<ul start="1"><li>[MDEV-7640](https://jira.mariadb.org/browse/MDEV-7640): CHANGE MASTER TO doesn't work with prepared statements
</li></ul>
- [Revision #151f967](https://github.com/MariaDB/server/commit/151f967)
<span class="cstm-style datetime">2015-10-11 17:06:03 -0400</span>
<ul start="1"><li>[MDEV-7640](https://jira.mariadb.org/browse/MDEV-7640): CHANGE MASTER TO doesn't work with prepared statements
</li></ul>
- [Revision #e7cb032](https://github.com/MariaDB/server/commit/e7cb032)
<span class="cstm-style datetime">2015-10-09 19:29:03 +0200</span>
<ul start="1"><li>fixes for buildbot:
</li></ul>
- [Revision #2ca4141](https://github.com/MariaDB/server/commit/2ca4141)
<span class="cstm-style datetime">2015-10-09 18:24:17 +0200</span>
<ul start="1"><li>Merge branch 'merge-perfschema-5.6' into 10.0
</li></ul>
- [Revision #01be663](https://github.com/MariaDB/server/commit/01be663)
<span class="cstm-style datetime">2015-10-09 18:16:27 +0200</span>
<ul start="1"><li>Merge branch 'merge-xtradb-5.6' into 10.0
</li></ul>
- [Revision #77c44a3](https://github.com/MariaDB/server/commit/77c44a3)
<span class="cstm-style datetime">2015-10-09 17:48:31 +0200</span>
<ul start="1"><li>update innodb version
</li></ul>
- [Revision #04af573](https://github.com/MariaDB/server/commit/04af573)
<span class="cstm-style datetime">2015-10-09 17:47:30 +0200</span>
<ul start="1"><li>Merge branch 'merge-innodb-5.6' into 10.0
</li></ul>
- [Revision #1b41eed](https://github.com/MariaDB/server/commit/1b41eed)
<span class="cstm-style datetime">2015-10-09 17:22:53 +0200</span>
<ul start="1"><li>5.6.27
</li></ul>
- [Revision #86ff4da](https://github.com/MariaDB/server/commit/86ff4da)
<span class="cstm-style datetime">2015-10-09 17:21:46 +0200</span>
<ul start="1"><li>5.6.27
</li></ul>
- [Revision #6a821d7](https://github.com/MariaDB/server/commit/6a821d7)
<span class="cstm-style datetime">2015-10-09 17:20:49 +0200</span>
<ul start="1"><li>5.6.26-74.0
</li></ul>
- [Revision #cfeedbf](https://github.com/MariaDB/server/commit/cfeedbf)
<span class="cstm-style datetime">2015-10-09 17:12:26 +0200</span>
<ul start="1"><li>Merge branch '5.5' into 10.0
</li></ul>
- [Revision #16c4b3c](https://github.com/MariaDB/server/commit/16c4b3c)
<span class="cstm-style datetime">2015-10-09 16:43:59 +0200</span>
<ul start="1"><li>fixes for buildbot:
</li></ul>
- [Revision #bff1af9](https://github.com/MariaDB/server/commit/bff1af9)
<span class="cstm-style datetime">2015-07-16 15:50:26 -0700</span>
<ul start="1"><li>Clarify the log message about master_info and relay_info files.
</li></ul>
- [Revision #0ea4233](https://github.com/MariaDB/server/commit/0ea4233)
<span class="cstm-style datetime">2015-09-18 18:27:54 +0200</span>
<ul start="1"><li>remove --default-myisam from mtr
</li></ul>
- [Revision #2a9bcc6](https://github.com/MariaDB/server/commit/2a9bcc6)
<span class="cstm-style datetime">2015-09-17 14:45:28 +0200</span>
<ul start="1"><li>[MDEV-7680](https://jira.mariadb.org/browse/MDEV-7680): mysqld man page
</li></ul>
- [Revision #99142ab](https://github.com/MariaDB/server/commit/99142ab)
<span class="cstm-style datetime">2015-09-17 14:34:03 +0200</span>
<ul start="1"><li>mysql and mysqldhow man pages
</li></ul>
- [Revision #ed195b2](https://github.com/MariaDB/server/commit/ed195b2)
<span class="cstm-style datetime">2015-09-10 20:12:50 +0200</span>
<ul start="1"><li>[MDEV-7680](https://jira.mariadb.org/browse/MDEV-7680): mysqld_safe and mysql_multi man pages
</li></ul>
- [Revision #5077509](https://github.com/MariaDB/server/commit/5077509)
<span class="cstm-style datetime">2015-09-09 14:32:52 +0200</span>
<ul start="1"><li>[MDEV-7680](https://jira.mariadb.org/browse/MDEV-7680): Update man pages
</li></ul>
- [Revision #f41a41f](https://github.com/MariaDB/server/commit/f41a41f)
<span class="cstm-style datetime">2015-10-09 00:06:16 +0200</span>
<ul start="1"><li>Merge branch 'merge-xtradb-5.5' into 5.5
</li></ul>
- [Revision #db79f4c](https://github.com/MariaDB/server/commit/db79f4c)
<span class="cstm-style datetime">2015-10-08 23:02:43 +0200</span>
<ul start="1"><li>5.5.45-37.4
</li></ul>
- [Revision #82e9f6d](https://github.com/MariaDB/server/commit/82e9f6d)
<span class="cstm-style datetime">2015-10-08 22:54:24 +0200</span>
<ul start="1"><li>Merge remote-tracking branch 'mysql/5.5' into 5.5
</li></ul>
- [Revision #c8d5112](https://github.com/MariaDB/server/commit/c8d5112)
<span class="cstm-style datetime">2015-10-08 00:32:07 +0200</span>
<ul start="1"><li>[MDEV-8796](https://jira.mariadb.org/browse/MDEV-8796) Delete with sub query with information_schema.TABLES deletes too many rows
</li></ul>
- [Revision #6dd4114](https://github.com/MariaDB/server/commit/6dd4114)
<span class="cstm-style datetime">2015-10-08 10:45:32 +0300</span>
<ul start="1"><li>Better error messages if slave is not properly configured
</li></ul>
- [Revision #a69a6dd](https://github.com/MariaDB/server/commit/a69a6dd)
<span class="cstm-style datetime">2015-10-08 10:45:09 +0300</span>
<ul start="1"><li>[MDEV-4487](https://jira.mariadb.org/browse/MDEV-4487) Allow replication from MySQL 5.6+ when GTID is enabled on the master [MDEV-8685](https://jira.mariadb.org/browse/MDEV-8685) MariaDB fails to decode Anonymous_GTID entries [MDEV-5705](https://jira.mariadb.org/browse/MDEV-5705) Replication testing: 5.6-&gt;10.0
</li></ul>
- [Revision #7c1e2fe](https://github.com/MariaDB/server/commit/7c1e2fe)
<span class="cstm-style datetime">2015-10-08 10:17:07 +0300</span>
<ul start="1"><li>Better error message if failed
</li></ul>
- [Revision #ca051fa](https://github.com/MariaDB/server/commit/ca051fa)
<span class="cstm-style datetime">2015-10-08 10:16:35 +0300</span>
<ul start="1"><li>Allow row events in replication stream for slave in all cases (even when configured with --binlog-format=statement). Before we got an error on the slave and the slave stopped if the master was configured with --binlog-format=mixed or --binlog-format=row.
</li></ul>
- [Revision #d278fb4](https://github.com/MariaDB/server/commit/d278fb4)
<span class="cstm-style datetime">2015-10-08 09:58:44 +0300</span>
<ul start="1"><li>Fixed tokudb test result to make it stable (was altering between index and range)
</li></ul>
- [Revision #4a60204](https://github.com/MariaDB/server/commit/4a60204)
<span class="cstm-style datetime">2015-10-06 16:15:34 +0300</span>
<ul start="1"><li>[MDEV-8903](https://jira.mariadb.org/browse/MDEV-8903): Buildbot valgrind failure: Invalid read of size 1 in sql_memdup...
</li></ul>
- [Revision #1289794](https://github.com/MariaDB/server/commit/1289794)
<span class="cstm-style datetime">2015-10-06 15:54:37 +0300</span>
<ul start="1"><li>Fix for [MDEV-8321](https://jira.mariadb.org/browse/MDEV-8321), [MDEV-6223](https://jira.mariadb.org/browse/MDEV-6223)
</li></ul>
- [Revision #504802f](https://github.com/MariaDB/server/commit/504802f)
<span class="cstm-style datetime">2015-08-05 11:57:35 +0200</span>
<ul start="1"><li>[MDEV-7846](https://jira.mariadb.org/browse/MDEV-7846): postreview fix
</li></ul>
- [Revision #54b9981](https://github.com/MariaDB/server/commit/54b9981)
<span class="cstm-style datetime">2015-04-23 20:08:57 +0200</span>
<ul start="1"><li>[MDEV-7846](https://jira.mariadb.org/browse/MDEV-7846): Server crashes in Item_subselect::fix_fields or fails with Thread stack overrun
</li></ul>
- [Revision #0ab93fd](https://github.com/MariaDB/server/commit/0ab93fd)
<span class="cstm-style datetime">2015-04-23 19:16:57 +0200</span>
<ul start="1"><li>[MDEV-7445](https://jira.mariadb.org/browse/MDEV-7445):Server crash with Signal 6 [MDEV-7565](https://jira.mariadb.org/browse/MDEV-7565): Server crash with Signal 6 (part 2)
</li></ul>
- [Revision #2e3e818](https://github.com/MariaDB/server/commit/2e3e818)
<span class="cstm-style datetime">2015-04-23 19:11:06 +0200</span>
<ul start="1"><li>[MDEV-7445](https://jira.mariadb.org/browse/MDEV-7445): Server crash with Signal 6
</li></ul>
- [Revision #7ccde2c](https://github.com/MariaDB/server/commit/7ccde2c)
<span class="cstm-style datetime">2015-04-23 19:04:11 +0200</span>
<ul start="1"><li>[MDEV-7565](https://jira.mariadb.org/browse/MDEV-7565): Server crash with Signal 6 (part 2)
</li></ul>
- [Revision #a7dd24c](https://github.com/MariaDB/server/commit/a7dd24c)
<span class="cstm-style datetime">2015-10-06 13:52:27 +0300</span>
<ul start="1"><li>[MDEV-8299](https://jira.mariadb.org/browse/MDEV-8299): MyISAM or Aria table gets corrupted after EXPLAIN INSERT and INSERT
</li></ul>
- [Revision #bb22eb5](https://github.com/MariaDB/server/commit/bb22eb5)
<span class="cstm-style datetime">2015-10-01 13:40:23 +0400</span>
<ul start="1"><li>[MDEV-8379](https://jira.mariadb.org/browse/MDEV-8379) - SUSE mariadb patches
</li></ul>
- [Revision #727da9c](https://github.com/MariaDB/server/commit/727da9c)
<span class="cstm-style datetime">2015-10-01 13:04:59 +0400</span>
<ul start="1"><li>[MDEV-8379](https://jira.mariadb.org/browse/MDEV-8379) - SUSE mariadb patches
</li></ul>
- [Revision #006acf7](https://github.com/MariaDB/server/commit/006acf7)
<span class="cstm-style datetime">2015-09-30 10:49:45 +0300</span>
<ul start="1"><li>Bug #68148: drop index on a foreign key column leads to missing table [MDEV-8845](https://jira.mariadb.org/browse/MDEV-8845): Table disappear after modifying FK
</li></ul>
- [Revision #a95711e](https://github.com/MariaDB/server/commit/a95711e)
<span class="cstm-style datetime">2015-09-29 08:39:54 +0300</span>
<ul start="1"><li>[MDEV-8855](https://jira.mariadb.org/browse/MDEV-8855): innodb.innodb-fk-warnings fails on Windows
</li></ul>
- [Revision #02a38fd](https://github.com/MariaDB/server/commit/02a38fd)
<span class="cstm-style datetime">2015-09-24 17:25:52 +0200</span>
<ul start="1"><li>[MDEV-8624](https://jira.mariadb.org/browse/MDEV-8624): MariaDB hangs on query with many logical condition
</li></ul>
- [Revision #f804b74](https://github.com/MariaDB/server/commit/f804b74)
<span class="cstm-style datetime">2015-09-28 03:40:29 +0300</span>
<ul start="1"><li>[MDEV-8154](https://jira.mariadb.org/browse/MDEV-8154) rpl.show_status_stop_slave_race-7126 sporadically causes internal check failure
</li></ul>
- [Revision #ce7d8c5](https://github.com/MariaDB/server/commit/ce7d8c5)
<span class="cstm-style datetime">2015-09-27 18:01:47 +0300</span>
<ul start="1"><li>[MDEV-7330](https://jira.mariadb.org/browse/MDEV-7330) plugins.feedback_plugin_send fails sporadically in buildbot
</li></ul>
- [Revision #bdcf370](https://github.com/MariaDB/server/commit/bdcf370)
<span class="cstm-style datetime">2015-09-27 16:00:48 +0300</span>
<ul start="1"><li>[MDEV-7933](https://jira.mariadb.org/browse/MDEV-7933) plugins.feedback_plugin_send depends on being executed after plugins.feedback_plugin_load
</li></ul>
- [Revision #2563609](https://github.com/MariaDB/server/commit/2563609)
<span class="cstm-style datetime">2015-09-26 02:51:29 +0300</span>
<ul start="1"><li>Increased the version number
</li></ul>
- [Revision #86ed494](https://github.com/MariaDB/server/commit/86ed494)
<span class="cstm-style datetime">2015-09-26 02:48:55 +0300</span>
<ul start="1"><li>[MDEV-8849](https://jira.mariadb.org/browse/MDEV-8849) rpl.rpl_innodb_bug30888 sporadically fails in buildbot with testcase timeout
</li></ul>
- [Revision #4d33f9d](https://github.com/MariaDB/server/commit/4d33f9d)
<span class="cstm-style datetime">2015-09-25 14:57:56 -0400</span>
<ul start="1"><li>Merge branch '5.5-galera' into 10.0-galera
</li></ul>
- [Revision #13615c5](https://github.com/MariaDB/server/commit/13615c5)
<span class="cstm-style datetime">2015-09-25 13:56:02 -0400</span>
<ul start="1"><li>[MDEV-8208](https://jira.mariadb.org/browse/MDEV-8208): Sporadic SEGFAULT on startup
</li></ul>
- [Revision #dca4ab9](https://github.com/MariaDB/server/commit/dca4ab9)
<span class="cstm-style datetime">2015-09-24 21:24:28 +0300</span>
<ul start="1"><li>[MDEV-8841](https://jira.mariadb.org/browse/MDEV-8841) innodb_zip.innodb-create-options fails in buildbot
</li></ul>
- [Revision #5cc149f](https://github.com/MariaDB/server/commit/5cc149f)
<span class="cstm-style datetime">2015-09-24 10:28:47 +0200</span>
<ul start="1"><li>The compiler warnings fixed.
</li></ul>
- [Revision #fea1568](https://github.com/MariaDB/server/commit/fea1568)
<span class="cstm-style datetime">2015-09-22 13:35:23 +0200</span>
<ul start="1"><li>Fix sporadic test failure in rpl_gtid_mdev4820.test
</li></ul>
- [Revision #81727cd](https://github.com/MariaDB/server/commit/81727cd)
<span class="cstm-style datetime">2015-09-22 12:54:01 +0300</span>
<ul start="1"><li>Backport to 10.0: [MDEV-8779](https://jira.mariadb.org/browse/MDEV-8779): mysqld got signal 11 in sql/opt_range_mrr.cc:100(step_down_to)
</li></ul>
- [Revision #d54bc3c](https://github.com/MariaDB/server/commit/d54bc3c)
<span class="cstm-style datetime">2015-09-21 20:49:31 -0400</span>
<ul start="1"><li>Cleanup: remove dead code which could also lead to race.
</li></ul>
- [Revision #9d5767c](https://github.com/MariaDB/server/commit/9d5767c)
<span class="cstm-style datetime">2015-09-21 20:47:05 -0400</span>
<ul start="1"><li>Post-merge fix.
</li></ul>
- [Revision #8d0d445](https://github.com/MariaDB/server/commit/8d0d445)
<span class="cstm-style datetime">2015-09-21 17:32:37 +0300</span>
<ul start="1"><li>Backport to 10.0: [MDEV-8779](https://jira.mariadb.org/browse/MDEV-8779): mysqld got signal 11 in sql/opt_range_mrr.cc:100(step_down_to)
</li></ul>
- [Revision #db2e21b](https://github.com/MariaDB/server/commit/db2e21b)
<span class="cstm-style datetime">2015-09-16 23:20:57 -0400</span>
<ul start="1"><li>[MDEV-8208](https://jira.mariadb.org/browse/MDEV-8208): Sporadic SEGFAULT on startup
</li></ul>
- [Revision #80d1237](https://github.com/MariaDB/server/commit/80d1237)
<span class="cstm-style datetime">2015-09-16 12:14:59 +0200</span>
<ul start="1"><li>Merge branch 'ob-10.0' into 10.0
</li></ul>
- [Revision #019c9e0](https://github.com/MariaDB/server/commit/019c9e0)
<span class="cstm-style datetime">2015-09-16 12:11:28 +0200</span>
<ul start="1"><li>Fix assert error for where clause with UDF's     was fixed in HA_CONNECT::CondFilter moving res= pval-&gt;val_str(&amp;tmp)     but this was wrong. Now res is only used for strings.   modified:   storage/connect/ha_connect.cc
</li></ul>
- [Revision #fd1b2e4](https://github.com/MariaDB/server/commit/fd1b2e4)
<span class="cstm-style datetime">2015-09-15 17:07:41 -0400</span>
<ul start="1"><li>[MDEV-8803](https://jira.mariadb.org/browse/MDEV-8803): Debian jessie 8.2 + [MariaDB 10.1.7](/kb/en/mariadb-1017-release-notes/) + GaleraCluster
</li></ul>
- [Revision #653aadc](https://github.com/MariaDB/server/commit/653aadc)
<span class="cstm-style datetime">2015-09-15 16:27:04 -0400</span>
<ul start="1"><li>[MDEV-8804](https://jira.mariadb.org/browse/MDEV-8804): bootstrap command missing in debian init script
</li></ul>
- [Revision #eac8e43](https://github.com/MariaDB/server/commit/eac8e43)
<span class="cstm-style datetime">2015-09-14 15:43:16 -0400</span>
<ul start="1"><li>Avoid caching wsrep threads (fixed the erroneous condition).
</li></ul>
- [Revision #a401c11](https://github.com/MariaDB/server/commit/a401c11)
<span class="cstm-style datetime">2015-09-14 15:26:50 -0400</span>
<ul start="1"><li>Fix failing test cases
</li></ul>
- [Revision #abd31ca](https://github.com/MariaDB/server/commit/abd31ca)
<span class="cstm-style datetime">2015-05-06 13:19:22 +0200</span>
<ul start="1"><li>[MDEV-7990](https://jira.mariadb.org/browse/MDEV-7990): ERROR 1526 when procedure executed for second time ALTER TABLE partition ... pMAX values less than MAXVALUE
</li></ul>
- [Revision #39e8dc9](https://github.com/MariaDB/server/commit/39e8dc9)
<span class="cstm-style datetime">2015-09-12 00:43:31 +0200</span>
<ul start="1"><li>Merge.
</li></ul>
- [Revision #528729f](https://github.com/MariaDB/server/commit/528729f)
<span class="cstm-style datetime">2015-09-12 00:42:21 +0200</span>
<ul start="1"><li>[MDEV-8193](https://jira.mariadb.org/browse/MDEV-8193): UNTIL clause in START SLAVE is sporadically disobeyed by parallel replication
</li></ul>
- [Revision #244f043](https://github.com/MariaDB/server/commit/244f043)
<span class="cstm-style datetime">2015-09-11 12:03:04 +0200</span>
<ul start="1"><li>Merge [MDEV-8193](https://jira.mariadb.org/browse/MDEV-8193) into 10.0
</li></ul>
- [Revision #51eaa7f](https://github.com/MariaDB/server/commit/51eaa7f)
<span class="cstm-style datetime">2015-09-11 10:51:56 +0200</span>
<ul start="1"><li>[MDEV-8193](https://jira.mariadb.org/browse/MDEV-8193): UNTIL clause in START SLAVE is sporadically disobeyed by parallel replication
</li></ul>
- [Revision #ceac344](https://github.com/MariaDB/server/commit/ceac344)
<span class="cstm-style datetime">2015-09-10 09:57:08 -0400</span>
<ul start="1"><li>[Bug #1372840](https://bugs.launchpad.net/bugs/1372840) - test case
</li></ul>
- [Revision #f479b5a](https://github.com/MariaDB/server/commit/f479b5a)
<span class="cstm-style datetime">2015-09-10 00:29:06 -0400</span>
<ul start="1"><li>Update WSREP_PATCH_REVNO
</li></ul>
- [Revision #f951071](https://github.com/MariaDB/server/commit/f951071)
<span class="cstm-style datetime">2015-09-08 22:11:56 -0700</span>
<ul start="1"><li>Galera MTR Tests: fix typo in the galera_as_slave_nonprim test, in suite/galera/galera_3nodes_as_slave.cnf
</li></ul>
- [Revision #b3ec0a8](https://github.com/MariaDB/server/commit/b3ec0a8)
<span class="cstm-style datetime">2015-09-08 05:04:47 -0700</span>
<ul start="1"><li>Galera MTR Tests: a test for async slave + non-prim
</li></ul>
- [Revision #db66d2f](https://github.com/MariaDB/server/commit/db66d2f)
<span class="cstm-style datetime">2015-09-10 00:20:49 -0400</span>
<ul start="1"><li>refs codership/mysql-wsrep#188
</li></ul>
- [Revision #2012a81](https://github.com/MariaDB/server/commit/2012a81)
<span class="cstm-style datetime">2015-09-10 00:14:24 -0400</span>
<ul start="1"><li>refs codership/mysql-wsrep#181
</li></ul>
- [Revision #c915d8c](https://github.com/MariaDB/server/commit/c915d8c)
<span class="cstm-style datetime">2015-09-01 00:57:20 -0700</span>
<ul start="1"><li>Galera MTR Tests: attempt to work around codership/mysql-wsrep#179
</li></ul>
- [Revision #25bbfe8](https://github.com/MariaDB/server/commit/25bbfe8)
<span class="cstm-style datetime">2015-08-31 02:16:43 -0700</span>
<ul start="1"><li>Galera MTR Tests: Instruct xtrabackup SST's socat to use 1024-bit DH keys to avoid 'error:14082174:SSL routines:SSL3_CHECK_CERT_AND_ALGORITHM:dh key too small'
</li></ul>
- [Revision #b6f8033](https://github.com/MariaDB/server/commit/b6f8033)
<span class="cstm-style datetime">2015-08-31 02:15:37 -0700</span>
<ul start="1"><li>Galera MTR Tests: Tests around do-* and ignore-* binlog options
</li></ul>
- [Revision #f7885fb](https://github.com/MariaDB/server/commit/f7885fb)
<span class="cstm-style datetime">2015-08-27 00:54:26 -0700</span>
<ul start="1"><li>Correct WSREP_PATCH_VERSION for 5.6 is 11
</li></ul>
- [Revision #52c9235](https://github.com/MariaDB/server/commit/52c9235)
<span class="cstm-style datetime">2015-08-25 06:15:20 -0700</span>
<ul start="1"><li>Galera MTR Tests: Add known Galera and mysql-wsrep Valgrind issues to valgrind.supp
</li></ul>
- [Revision #371dc33](https://github.com/MariaDB/server/commit/371dc33)
<span class="cstm-style datetime">2015-08-24 06:56:30 -0700</span>
<ul start="1"><li>refs codership/mysql-wsrep#90 MTR test case for mysql-wsrep#90
</li></ul>
- [Revision #e5b595e](https://github.com/MariaDB/server/commit/e5b595e)
<span class="cstm-style datetime">2015-08-14 05:14:53 -0700</span>
<ul start="1"><li>Galera MTR Tests: fix typo in suite/galera/galera_2nodes_as_slave.cnf
</li></ul>
- [Revision #ee22ac3](https://github.com/MariaDB/server/commit/ee22ac3)
<span class="cstm-style datetime">2015-08-14 01:16:25 -0700</span>
<ul start="1"><li>Galera MTR Tests: Various test stability fixes (take #5)
</li></ul>
- [Revision #7d73931](https://github.com/MariaDB/server/commit/7d73931)
<span class="cstm-style datetime">2015-08-12 06:40:59 -0700</span>
<ul start="1"><li>Galera MTR Tests: Various test stability fixes (take #4)
</li></ul>
- [Revision #ff76214](https://github.com/MariaDB/server/commit/ff76214)
<span class="cstm-style datetime">2015-08-12 05:32:18 -0700</span>
<ul start="1"><li>Galera MTR Tests: Various test stability fixes (take #3)
</li></ul>
- [Revision #fd0aaad](https://github.com/MariaDB/server/commit/fd0aaad)
<span class="cstm-style datetime">2015-08-12 01:03:21 -0700</span>
<ul start="1"><li>Galera MTR Tests: Various test stability fixes (take #2)
</li></ul>
- [Revision #997119d](https://github.com/MariaDB/server/commit/997119d)
<span class="cstm-style datetime">2015-08-11 04:22:38 -0700</span>
<ul start="1"><li>Galera MTR Tests: Various test stability fixes.
</li></ul>
- [Revision #182b237](https://github.com/MariaDB/server/commit/182b237)
<span class="cstm-style datetime">2015-08-07 21:15:02 -0700</span>
<ul start="1"><li>Galera MTR Tests: remove variable output from galera_gra_log.test (take #2)
</li></ul>
- [Revision #c9d4581](https://github.com/MariaDB/server/commit/c9d4581)
<span class="cstm-style datetime">2015-08-06 20:55:30 -0700</span>
<ul start="1"><li>Galera MTR Tests: remove variable output from galera_gra_log.test
</li></ul>
- [Revision #2316a4e](https://github.com/MariaDB/server/commit/2316a4e)
<span class="cstm-style datetime">2015-08-06 00:34:00 -0700</span>
<ul start="1"><li>Galera MTR Tests: Tests for GRA*.log files, replication bundle, preordered events, forced binlog format
</li></ul>
- [Revision #a1a7414](https://github.com/MariaDB/server/commit/a1a7414)
<span class="cstm-style datetime">2015-08-03 03:20:52 -0700</span>
<ul start="1"><li>Galera MTR Tests: An end-to-end test with restoring a node from xtrabackup; a test for restoring the primary component via pc.bootstrap
</li></ul>
- [Revision #1e29068](https://github.com/MariaDB/server/commit/1e29068)
<span class="cstm-style datetime">2015-07-23 04:30:07 -0700</span>
<ul start="1"><li>Galera MTR Tests: Valgrind suppression for codership/galera#306
</li></ul>
- [Revision #3893b5c](https://github.com/MariaDB/server/commit/3893b5c)
<span class="cstm-style datetime">2015-07-23 04:29:40 -0700</span>
<ul start="1"><li>Galera MTR Tests: mark all tests operating on large data sets with --source include/big-test.inc to help with Valgrind
</li></ul>
- [Revision #83579c2](https://github.com/MariaDB/server/commit/83579c2)
<span class="cstm-style datetime">2015-07-10 07:17:20 -0700</span>
<ul start="1"><li>Galera MTR Tests: fixes for mysqldump SST/IST tests
</li></ul>
- [Revision #10f5c08](https://github.com/MariaDB/server/commit/10f5c08)
<span class="cstm-style datetime">2015-07-08 01:52:45 -0700</span>
<ul start="1"><li>Refs codership/QA#47. Additional tests for FTWRL.
</li></ul>
- [Revision #6104a27](https://github.com/MariaDB/server/commit/6104a27)
<span class="cstm-style datetime">2015-07-07 06:01:00 -0700</span>
<ul start="1"><li>Galera MTR Tests: increase lock wait timeout in suite/galera/t/galera_many_rows.test
</li></ul>
- [Revision #4a630ce](https://github.com/MariaDB/server/commit/4a630ce)
<span class="cstm-style datetime">2015-07-07 06:00:22 -0700</span>
<ul start="1"><li>Galera MTR Tests: A test for xtrabackup with key+cert encryption.
</li></ul>
- [Revision #edd9bd3](https://github.com/MariaDB/server/commit/edd9bd3)
<span class="cstm-style datetime">2015-07-07 03:42:03 -0700</span>
<ul start="1"><li>Fixes codership/mysql-wsrep#153 use --defaults-extra-file with mysqldump SST
</li></ul>
- [Revision #5d531f0](https://github.com/MariaDB/server/commit/5d531f0)
<span class="cstm-style datetime">2015-07-01 03:13:04 -0700</span>
<ul start="1"><li>Galera MTR Tests: Use SET GLOBAL when setting wsrep_replicate_myisam, as it is a GLOBAL variable in MySQL Galera Cluster and SESSION in Percona XTraDB Cluster
</li></ul>
- [Revision #fbe739c](https://github.com/MariaDB/server/commit/fbe739c)
<span class="cstm-style datetime">2015-06-29 16:42:58 +0530</span>
<ul start="1"><li>Bug#1421360: Add 'FLUSH TABLES' to Total Order Isolation execution.
</li></ul>
- [Revision #5a002ad](https://github.com/MariaDB/server/commit/5a002ad)
<span class="cstm-style datetime">2015-06-26 01:30:01 -0700</span>
<ul start="1"><li>Galera MTR Tests: various tests and test fixes
</li></ul>
- [Revision #f1a00ed](https://github.com/MariaDB/server/commit/f1a00ed)
<span class="cstm-style datetime">2015-06-17 05:14:36 -0700</span>
<ul start="1"><li>Galera MTR Tests: Use wsrep_sst_auth for tests that use xtrabackup + IST
</li></ul>
- [Revision #2ea16b9](https://github.com/MariaDB/server/commit/2ea16b9)
<span class="cstm-style datetime">2015-06-08 21:06:22 +0300</span>
<ul start="1"><li>This commit fixes   - errno handling in wsp::env::append() method, where error could be returned by mistake   - return code of sst_prepare_other() when pthread_create() fails - it was returning positive error code which by convention is treated as success.
</li></ul>
- [Revision #0ccbbff](https://github.com/MariaDB/server/commit/0ccbbff)
<span class="cstm-style datetime">2015-06-08 12:23:53 +0300</span>
<ul start="1"><li>Slight cleanup improvement on a previous commit.
</li></ul>
- [Revision #bc796c2](https://github.com/MariaDB/server/commit/bc796c2)
<span class="cstm-style datetime">2015-06-08 01:43:27 -0700</span>
<ul start="1"><li>Refs codership/mysql-wsrep#143 . Account for the case where the SST password is empty
</li></ul>
- [Revision #86ee30c](https://github.com/MariaDB/server/commit/86ee30c)
<span class="cstm-style datetime">2015-06-06 01:08:41 +0300</span>
<ul start="1"><li>Refs codership/mysql-wsrep#141: this commit   1. Passes wsrep_sst_auth_value to SST scripts via WSREP_SST_OPT_AUTH envronmental variable, so it never appears on the command line   2. In mysqldump and xtrabackup* SST scripts which rely on MySQL authentication, instead of passing password on the command line, SST script sets MYSQL_PWD environment variable, so that password also never appears on the mysqldump/innobackupex command line.
</li></ul>
- [Revision #197e9d2](https://github.com/MariaDB/server/commit/197e9d2)
<span class="cstm-style datetime">2015-05-26 15:44:34 +0300</span>
<ul start="1"><li>Refs codership/mysql-wsrep#132 - fix for THD::m_digest initialization, according to Raghu
</li></ul>
- [Revision #483078b](https://github.com/MariaDB/server/commit/483078b)
<span class="cstm-style datetime">2015-05-15 02:15:58 -0700</span>
<ul start="1"><li>Fixes codership/QA#87 . An MTR test for SERIALIZABLE
</li></ul>
- [Revision #4102d52](https://github.com/MariaDB/server/commit/4102d52)
<span class="cstm-style datetime">2015-05-11 02:21:39 -0700</span>
<ul start="1"><li>Refs codership/mysql-wsrep#113 - tests around FLUSH TABLE, FLUSH TABLES, LOCK TABLE
</li></ul>
- [Revision #2106fed](https://github.com/MariaDB/server/commit/2106fed)
<span class="cstm-style datetime">2015-05-10 21:49:36 +0300</span>
<ul start="1"><li>Refs codership/mysql-wsrep#113 - changed BF thread's MDL wait to never timeout - all explicit locks are now honored by BF threads
</li></ul>
- [Revision #f9805e4](https://github.com/MariaDB/server/commit/f9805e4)
<span class="cstm-style datetime">2015-05-08 03:21:50 -0700</span>
<ul start="1"><li>Galera MTR Tests: tests for WAN restart, xtrabackup options and others.
</li></ul>
- [Revision #ef7b089](https://github.com/MariaDB/server/commit/ef7b089)
<span class="cstm-style datetime">2015-05-06 10:35:02 +0300</span>
<ul start="1"><li>Fixes codership/mysql-wsrep#122 - causal/casual typos fixed in wsrep code
</li></ul>
- [Revision #bace2a9](https://github.com/MariaDB/server/commit/bace2a9)
<span class="cstm-style datetime">2015-05-05 01:21:55 -0700</span>
<ul start="1"><li>Galera MTR Tests: add a test for socket.ssl_compression
</li></ul>
- [Revision #b5ef2bb](https://github.com/MariaDB/server/commit/b5ef2bb)
<span class="cstm-style datetime">2015-04-28 23:34:47 -0700</span>
<ul start="1"><li>Re-enable tests previously disabled due to mysql-wsrep#114
</li></ul>
- [Revision #63c5bee](https://github.com/MariaDB/server/commit/63c5bee)
<span class="cstm-style datetime">2015-04-28 20:38:25 +0300</span>
<ul start="1"><li>Refs codership/mysql-wsrep#113 - Extended the protection of local FLUSH sessions to cover all exclusive MDL locks
</li></ul>
- [Revision #417f778](https://github.com/MariaDB/server/commit/417f778)
<span class="cstm-style datetime">2015-04-28 00:55:50 -0700</span>
<ul start="1"><li>Galera MTR tests: disable innodb.innodb_stats_* due to mysql-wsrep#114
</li></ul>
- [Revision #6bb890c](https://github.com/MariaDB/server/commit/6bb890c)
<span class="cstm-style datetime">2015-04-24 10:39:42 +0300</span>
<ul start="1"><li>refs codership/mysql-wsrep#114 - skipping TOI if not using wsrep provider
</li></ul>
- [Revision #c666090](https://github.com/MariaDB/server/commit/c666090)
<span class="cstm-style datetime">2015-04-21 16:22:53 +0300</span>
<ul start="1"><li>Refs codership/mysql-wsrep#113 Protecting non replicated FLUSH session from brute force aborts
</li></ul>
- [Revision #045b31c](https://github.com/MariaDB/server/commit/045b31c)
<span class="cstm-style datetime">2015-04-20 05:58:24 -0700</span>
<ul start="1"><li>Test cases for codership/mysql-wsrep/110
</li></ul>
- [Revision #dc9e325](https://github.com/MariaDB/server/commit/dc9e325)
<span class="cstm-style datetime">2015-04-20 13:18:21 +0300</span>
<ul start="1"><li>refs codership/mysql-wsrep#110 - clear table map events on SAVEPOINT
</li></ul>
- [Revision #d0e24c6](https://github.com/MariaDB/server/commit/d0e24c6)
<span class="cstm-style datetime">2015-04-01 02:52:24 -0700</span>
<ul start="1"><li>Galera MTR Tests: Attempt to remove rare sporadic failures in galera_transaction_replay.test by waiting for all transactions to get blocked before proceeding.
</li></ul>
- [Revision #f8b724d](https://github.com/MariaDB/server/commit/f8b724d)
<span class="cstm-style datetime">2015-03-31 06:43:38 -0700</span>
<ul start="1"><li>Galera MTR Tests: Enable the use of --parallel for port-intensive Galera tests by additionally specifying --port-group-size=50
</li></ul>
- [Revision #9f716ae](https://github.com/MariaDB/server/commit/9f716ae)
<span class="cstm-style datetime">2015-03-29 23:56:21 -0700</span>
<ul start="1"><li>Galera MTR: Disable rpl.rpl_rotate_logs binlog.binlog_index due to codership/mysql-wsrep#71
</li></ul>
- [Revision #fa5f18d](https://github.com/MariaDB/server/commit/fa5f18d)
<span class="cstm-style datetime">2015-09-09 20:51:39 -0400</span>
<ul start="1"><li>Merge branch '5.5-galera' into 10.0-galera
</li></ul>
- [Revision #37ae601](https://github.com/MariaDB/server/commit/37ae601)
<span class="cstm-style datetime">2015-09-09 18:54:14 -0400</span>
<ul start="1"><li>Update WSREP_PATCH_REVNO
</li></ul>
- [Revision #760b0c4](https://github.com/MariaDB/server/commit/760b0c4)
<span class="cstm-style datetime">2015-08-27 00:41:56 -0700</span>
<ul start="1"><li>Bump WSREP_PATCH_VERSION in cmake/wsrep.cmake to 12
</li></ul>
- [Revision #bee94cc](https://github.com/MariaDB/server/commit/bee94cc)
<span class="cstm-style datetime">2015-07-07 22:34:25 -0700</span>
<ul start="1"><li>Fixes codership/mysql-wsrep#153 use --defaults-extra-file with mysqldump SST
</li></ul>
- [Revision #55dfddf](https://github.com/MariaDB/server/commit/55dfddf)
<span class="cstm-style datetime">2015-06-09 17:02:26 +0300</span>
<ul start="1"><li>Fixing donate callback return code
</li></ul>
- [Revision #0465e3a](https://github.com/MariaDB/server/commit/0465e3a)
<span class="cstm-style datetime">2015-06-09 11:36:31 +0300</span>
<ul start="1"><li>Logging message cleanup
</li></ul>
- [Revision #d809fcc](https://github.com/MariaDB/server/commit/d809fcc)
<span class="cstm-style datetime">2015-06-08 21:06:22 +0300</span>
<ul start="1"><li>This commit fixes   - errno handling in wsp::env::append() method, where error could be returned by mistake   - return code of sst_prepare_other() when pthread_create() fails - it was returning positive error code which by convention is treated as success.
</li></ul>
- [Revision #1b1410c](https://github.com/MariaDB/server/commit/1b1410c)
<span class="cstm-style datetime">2015-06-08 12:23:53 +0300</span>
<ul start="1"><li>Slight cleanup improvement on a previous commit.
</li></ul>
- [Revision #62c2539](https://github.com/MariaDB/server/commit/62c2539)
<span class="cstm-style datetime">2015-06-08 01:46:20 -0700</span>
<ul start="1"><li>Refs codership/mysql-wsrep#143 . Account for the case where the SST password is empty
</li></ul>
- [Revision #a7ea3ec](https://github.com/MariaDB/server/commit/a7ea3ec)
<span class="cstm-style datetime">2015-06-06 01:38:07 +0300</span>
<ul start="1"><li>Synced xtrabackup SST fixes from Percona tree (as of PXC 5.6.24-25.11 release). This fixes/adresses the following LP bugs:   - LP1380697: wsrep_sst_xtrabackup-v2 doesn't stop when mysql is SIGKILLed. (full fix for this (as engineeered by Percona) requires Linux-specific patch that we don't carry, but keep xtrabackup scripts as close as possible)   - LP1399134: Log the innobackupex/SST logs in SST to syslog if possible. (fixed)   - LP1405668: Race condition between donor and joiner in PXB SST. (fixed)   - LP1405985: Fail early if xtrabackup_checkkpoints is missing. (fixed)   - LP1407599: wsrep_sst_xtrabackup-v2 script causes innobackupex to print a false positive stack trace into the log. (fixed)   - LP1441762: IST Fails with SST script error. (fixed)   - LP1451670: Fail when move-back fails in xtrabackup SST. (fixed)
</li></ul>
- [Revision #d78110e](https://github.com/MariaDB/server/commit/d78110e)
<span class="cstm-style datetime">2015-06-06 01:08:41 +0300</span>
<ul start="1"><li>Refs codership/mysql-wsrep#141: this commit   1. Passes wsrep_sst_auth_value to SST scripts via WSREP_SST_OPT_AUTH envronmental variable, so it never appears on the command line   2. In mysqldump and xtrabackup* SST scripts which rely on MySQL authentication, instead of passing password on the command line, SST script sets MYSQL_PWD environment variable, so that password also never appears on the mysqldump/innobackupex command line.
</li></ul>
- [Revision #4f4f3a5](https://github.com/MariaDB/server/commit/4f4f3a5)
<span class="cstm-style datetime">2015-05-02 22:25:39 +0300</span>
<ul start="1"><li>Fixes codership/mysql-wsrep#118
</li></ul>
- [Revision #d69931e](https://github.com/MariaDB/server/commit/d69931e)
<span class="cstm-style datetime">2015-09-09 01:28:04 +0200</span>
<ul start="1"><li>Merge branch 'ob-10.0' into 10.0
</li></ul>
- [Revision #e939ea5](https://github.com/MariaDB/server/commit/e939ea5)
<span class="cstm-style datetime">2015-09-09 01:26:00 +0200</span>
<ul start="1"><li>Fix assert error for where clause with UDF's     was fixed in HA_CONNECT::CondFilter moving pval-&gt;val_str(&amp;tmp)   modified:   storage/connect/ha_connect.cc
</li></ul>
- [Revision #29ac245](https://github.com/MariaDB/server/commit/29ac245)
<span class="cstm-style datetime">2015-09-07 13:13:52 +0200</span>
<ul start="1"><li>[MDEV-8473](https://jira.mariadb.org/browse/MDEV-8473): mysqlbinlog -v does not properly decode DECIMAL values in an RBR log
</li></ul>
- [Revision #0ce0b88](https://github.com/MariaDB/server/commit/0ce0b88)
<span class="cstm-style datetime">2015-09-01 11:47:06 +0200</span>
<ul start="1"><li>[MDEV-8450](https://jira.mariadb.org/browse/MDEV-8450): PATCH] Wrong macro expansion in Query_cache::send_result_to_client()
</li></ul>
- [Revision #102a85f](https://github.com/MariaDB/server/commit/102a85f)
<span class="cstm-style datetime">2015-09-03 18:00:43 +0200</span>
<ul start="1"><li>[MDEV-8663](https://jira.mariadb.org/browse/MDEV-8663): IF Statement returns multiple values erroneously (or Assertion `!null_value' failed in Item::send(Protocol*, String*))
</li></ul>
- [Revision #9abf426](https://github.com/MariaDB/server/commit/9abf426)
<span class="cstm-style datetime">2015-09-04 13:35:31 +0300</span>
<ul start="1"><li>[MDEV-8443](https://jira.mariadb.org/browse/MDEV-8443): mysql-test - innodb.innodb_simulate_comp_failures 'innodb_plugin' is failing
</li></ul>
- [Revision #bd8ffe7](https://github.com/MariaDB/server/commit/bd8ffe7)
<span class="cstm-style datetime">2015-09-03 09:39:57 +0200</span>
<ul start="1"><li>Merge pull request #87 from pivanof/qplan_macros
</li></ul>
- [Revision #83c7b1e](https://github.com/MariaDB/server/commit/83c7b1e)
<span class="cstm-style datetime">2015-09-02 10:40:34 +0200</span>
<ul start="1"><li>Merge [MDEV-8725](https://jira.mariadb.org/browse/MDEV-8725) into 10.0
</li></ul>
- [Revision #09bfaf3](https://github.com/MariaDB/server/commit/09bfaf3)
<span class="cstm-style datetime">2015-09-02 10:08:09 +0200</span>
<ul start="1"><li>Fix a potential lost wakeup for binlog_commit_wait_usec
</li></ul>
- [Revision #999c43a](https://github.com/MariaDB/server/commit/999c43a)
<span class="cstm-style datetime">2015-09-02 09:57:18 +0200</span>
<ul start="1"><li>[MDEV-8725](https://jira.mariadb.org/browse/MDEV-8725): Assertion `!(thd-&gt;rgi_slave &amp;&amp; thd-&gt; rgi_slave-&gt;did_mark_start_commit)' failed in ha_rollback_trans
</li></ul>
- [Revision #4b41e3c](https://github.com/MariaDB/server/commit/4b41e3c)
<span class="cstm-style datetime">2015-08-31 18:40:24 +0200</span>
<ul start="1"><li>[MDEV-6219](https://jira.mariadb.org/browse/MDEV-6219): Server crashes in Bitmap&lt;64u&gt;::merge (this=0x180, map2=...) on 2nd execution of PS with INSERT .. SELECT, derived_merge
</li></ul>
- [Revision #f533b2b](https://github.com/MariaDB/server/commit/f533b2b)
<span class="cstm-style datetime">2015-08-25 11:15:45 -0400</span>
<ul start="1"><li>Merge branch '5.5-galera' into 10.0-galera
</li></ul>
- [Revision #b66455f6](https://github.com/MariaDB/server/commit/b66455f6)
<span class="cstm-style datetime">2015-08-24 01:41:12 +0300</span>
<ul start="1"><li>Increase the version number
</li></ul>
- [Revision #aef8bfd](https://github.com/MariaDB/server/commit/aef8bfd)
<span class="cstm-style datetime">2015-08-24 01:37:21 +0300</span>
<ul start="1"><li>[MDEV-8670](https://jira.mariadb.org/browse/MDEV-8670) main.[MDEV-504](https://jira.mariadb.org/browse/MDEV-504) fails on Windows (in buildbot and outside)
</li></ul>
- [Revision #472d663](https://github.com/MariaDB/server/commit/472d663)
<span class="cstm-style datetime">2015-08-22 01:18:02 -0400</span>
<ul start="1"><li>[MDEV-8149](https://jira.mariadb.org/browse/MDEV-8149): Random mtr test failures during warning check
</li></ul>
- [Revision #4ee2886](https://github.com/MariaDB/server/commit/4ee2886)
<span class="cstm-style datetime">2015-08-20 20:55:52 -0400</span>
<ul start="1"><li>[MDEV-5146](https://jira.mariadb.org/browse/MDEV-5146) : Bulk loads into partitioned table not working
</li></ul>
- [Revision #ccd39b2](https://github.com/MariaDB/server/commit/ccd39b2)
<span class="cstm-style datetime">2015-08-20 09:55:54 -0400</span>
<ul start="1"><li>Backport partition tests from 10.0-galera.
</li></ul>
- [Revision #98bebad](https://github.com/MariaDB/server/commit/98bebad)
<span class="cstm-style datetime">2015-08-18 17:03:28 -0400</span>
<ul start="1"><li>Fix for a typo.
</li></ul>
- [Revision #9b475ee](https://github.com/MariaDB/server/commit/9b475ee)
<span class="cstm-style datetime">2015-08-05 20:43:25 +0300</span>
<ul start="1"><li>[MDEV-8289](https://jira.mariadb.org/browse/MDEV-8289): Semijoin inflates number of rows in query result - Make semi-join optimizer not to choose LooseScan   when 1) the index is not covered and 2) full index   scan will be required.
</li></ul>
- [Revision #cd9b919](https://github.com/MariaDB/server/commit/cd9b919)
<span class="cstm-style datetime">2015-08-14 15:49:46 +0200</span>
<ul start="1"><li>Merge branch 'ob-10.0' into 10.0
</li></ul>
- [Revision #6d46c97](https://github.com/MariaDB/server/commit/6d46c97)
<span class="cstm-style datetime">2015-08-14 14:23:14 +0200</span>
<ul start="1"><li>Fix crash when SetValue_char is called with a negative length value. This can happen in odbconn.cpp when SQLFetch returns SQL_NO_TOTAL (-4) as length.   modified:   storage/connect/odbconn.cpp   modified:   storage/connect/value.cpp
</li></ul>
- [Revision #1bfe4da](https://github.com/MariaDB/server/commit/1bfe4da)
<span class="cstm-style datetime">2015-08-13 01:28:15 +0300</span>
<ul start="1"><li>Fixed mysqltest run failure: Test did not clean up after itself properly
</li></ul>
- [Revision #afa9cb7](https://github.com/MariaDB/server/commit/afa9cb7)
<span class="cstm-style datetime">2015-08-13 01:27:23 +0300</span>
<ul start="1"><li>Fixed overrun in key cache if one tried to allocate a key cache of more than 45G with a key_cache_block_size of 1024 or less.
</li></ul>
- [Revision #335ec7a](https://github.com/MariaDB/server/commit/335ec7a)
<span class="cstm-style datetime">2015-08-11 21:15:33 +0200</span>
<ul start="1"><li>Prevent wrong update of expanded columns when pretty is not 2.   modified:   storage/connect/tabjson.cpp
</li></ul>
- [Revision #0eacebf](https://github.com/MariaDB/server/commit/0eacebf)
<span class="cstm-style datetime">2015-08-08 10:54:47 +0200</span>
<ul start="1"><li>Merge branch 'ob-10.0' into 10.0
</li></ul>
- [Revision #5f53303](https://github.com/MariaDB/server/commit/5f53303)
<span class="cstm-style datetime">2015-08-06 17:46:47 +0200</span>
<ul start="1"><li>Fix the TDBDOS::EstimatedLength function that was wrongly counting     its calculation virtual and special columns.   modified:   storage/connect/reldef.h   modified:   storage/connect/tabdos.cpp
</li></ul>
- [Revision #203f4d4](https://github.com/MariaDB/server/commit/203f4d4)
<span class="cstm-style datetime">2015-07-16 15:59:55 -0700</span>
<ul start="1"><li>Add parenthesis in macro definitions to prevent order of operation problems.</li></ul>