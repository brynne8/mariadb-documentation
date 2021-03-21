# MariaDB Galera Cluster 10.0.17 Changelog

The most recent [MariaDB Galera Cluster 10.0](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 10.0.38](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10038-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/10.0)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/10.0.17)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10017-release-notes)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10017-changelog)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 11 Mar 2015

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10017-release-notes).

The revision number links will take you to the revision's page on Github. On
Github you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #d146605](https://github.com/MariaDB/server/commit/d146605)
<span class="cstm-style datetime">2015-03-06 13:22:15 -0500</span>
<ul start="1"><li>[MDEV-7673](https://jira.mariadb.org/browse/MDEV-7673): CREATE TABLE SELECT fails on Galera cluster
</li></ul>
- [Revision #c6acdf7](https://github.com/MariaDB/server/commit/c6acdf7)
<span class="cstm-style datetime">2015-03-06 13:19:49 -0500</span>
<ul start="1"><li>[MDEV-7203](https://jira.mariadb.org/browse/MDEV-7203): replicate_events_marked_for_skip didn't work on Galera cluster
</li></ul>
- [Revision #6f9e33e](https://github.com/MariaDB/server/commit/6f9e33e)
<span class="cstm-style datetime">2014-12-26 23:38:45 +0400</span>
<ul start="1"><li>[MDEV-7273](https://jira.mariadb.org/browse/MDEV-7273) - 10.1 fails to start up during tc_log initializations on PPC64
</li></ul>
- [Revision #9af42db](https://github.com/MariaDB/server/commit/9af42db)
<span class="cstm-style datetime">2015-03-05 14:14:00 -0500</span>
<ul start="1"><li>[MDEV-7192](https://jira.mariadb.org/browse/MDEV-7192): binlog_annotate_row_events not completely compatible with galera
</li></ul>
- [Revision #73a143a](https://github.com/MariaDB/server/commit/73a143a)
<span class="cstm-style datetime">2015-03-04 19:52:15 -0500</span>
<ul start="1"><li>Update galera package name
</li></ul>
- [Revision #e52a58a](https://github.com/MariaDB/server/commit/e52a58a)
<span class="cstm-style datetime">2015-03-04 19:49:17 -0500</span>
<ul start="1"><li>Update galera package name
</li></ul>
- [Revision #aa2904a](https://github.com/MariaDB/server/commit/aa2904a)
<span class="cstm-style datetime">2015-02-27 22:13:37 -0500</span>
<ul start="1"><li>[MDEV-7560](https://jira.mariadb.org/browse/MDEV-7560): wsrep* tests depend on the version of galera library
</li></ul>
- [Revision #8ee5668](https://github.com/MariaDB/server/commit/8ee5668)
<span class="cstm-style datetime">2015-02-24 21:55:22 -0500</span>
<ul start="1"><li>Changes in wsrep_guess_ip()
</li></ul>
- [Revision #4a3e94e](https://github.com/MariaDB/server/commit/4a3e94e)
<span class="cstm-style datetime">2015-02-25 16:58:36 +0300</span>
<ul start="1"><li>[MDEV-7413](https://jira.mariadb.org/browse/MDEV-7413): optimizer_use_condition_selectivity &gt; 2 crashes 10.0.15+maria-1wheezy
</li></ul>
- [Revision #f825b5a](https://github.com/MariaDB/server/commit/f825b5a)
<span class="cstm-style datetime">2015-02-25 14:13:32 +0400</span>
<ul start="1"><li>[MDEV-7629](https://jira.mariadb.org/browse/MDEV-7629) Regression: Bit and hex string literals changed column names in 10.0.14
</li></ul>
- [Revision #cbf8cdc](https://github.com/MariaDB/server/commit/cbf8cdc)
<span class="cstm-style datetime">2015-02-25 09:43:31 +0100</span>
<ul start="1"><li>[MDEV-7530](https://jira.mariadb.org/browse/MDEV-7530) !includedir reads files in random order
</li></ul>
- [Revision #4fb2f66](https://github.com/MariaDB/server/commit/4fb2f66)
<span class="cstm-style datetime">2015-02-24 21:55:22 -0500</span>
<ul start="1"><li>Changes in wsrep_guess_ip()
</li></ul>
- [Revision #126523d](https://github.com/MariaDB/server/commit/126523d)
<span class="cstm-style datetime">2015-02-23 20:53:41 +0100</span>
<ul start="1"><li>[MDEV-6703](https://jira.mariadb.org/browse/MDEV-6703) Add "mysqlbinlog --binlog-row-event-max-size" support
</li></ul>
- [Revision #73033e5](https://github.com/MariaDB/server/commit/73033e5)
<span class="cstm-style datetime">2015-02-24 09:18:53 +0100</span>
<ul start="1"><li>fix mroonga to compile w/o performance schema
</li></ul>
- [Revision #a227cf8](https://github.com/MariaDB/server/commit/a227cf8)
<span class="cstm-style datetime">2015-02-24 14:03:14 +0100</span>
<ul start="1"><li>[MDEV-7335](https://jira.mariadb.org/browse/MDEV-7335): Potential parallel slave deadlock with specific binlog corruption
</li></ul>
- [Revision #8799f87](https://github.com/MariaDB/server/commit/8799f87)
<span class="cstm-style datetime">2015-02-24 10:33:49 +0200</span>
<ul start="1"><li>[MDEV-7623](https://jira.mariadb.org/browse/MDEV-7623): Add lock wait time and hold time to every record/table lock in InnoDB transaction lock printout.
</li></ul>
- [Revision #79e9ff4](https://github.com/MariaDB/server/commit/79e9ff4)
<span class="cstm-style datetime">2015-02-23 13:37:34 +0100</span>
<ul start="1"><li>[MDEV-7458](https://jira.mariadb.org/browse/MDEV-7458): Deadlock in parallel replication can allow following transaction to start replicating too early
</li></ul>
- [Revision #41cfdc8](https://github.com/MariaDB/server/commit/41cfdc8)
<span class="cstm-style datetime">2015-02-23 13:36:52 +0100</span>
<ul start="1"><li>Add error handling on realpath() call.
</li></ul>
- [Revision #90635c6](https://github.com/MariaDB/server/commit/90635c6)
<span class="cstm-style datetime">2015-02-23 11:24:19 +0200</span>
<ul start="1"><li>[MDEV-7620](https://jira.mariadb.org/browse/MDEV-7620): Transaction lock wait is missing number of lock waits and total wait time.
</li></ul>
- [Revision #a736e63](https://github.com/MariaDB/server/commit/a736e63)
<span class="cstm-style datetime">2015-02-22 17:53:02 +0100</span>
<ul start="1"><li>- Add new Json UDF's Json_Array_Add, Json_Array_Grp and Json_Object_Grp.   Handle longjmp's raised during json processing. modified:   storage/connect/global.h   storage/connect/ha_connect.cc   storage/connect/json.cpp   storage/connect/jsonudf.cpp
</li></ul>
- [Revision #3653de8](https://github.com/MariaDB/server/commit/3653de8)
<span class="cstm-style datetime">2015-02-21 19:49:57 +0100</span>
<ul start="1"><li>test failure on labrador: account for a different errno on Mac OS X
</li></ul>
- [Revision #dc94bd0](https://github.com/MariaDB/server/commit/dc94bd0)
<span class="cstm-style datetime">2015-02-21 12:15:19 +0100</span>
<ul start="1"><li>[MDEV-7520](https://jira.mariadb.org/browse/MDEV-7520) gtid replication broken during upgrade to debian 10.0.16
</li></ul>
- [Revision #0ba1680](https://github.com/MariaDB/server/commit/0ba1680)
<span class="cstm-style datetime">2015-02-21 10:43:27 +0100</span>
<ul start="1"><li>[MDEV-6769](https://jira.mariadb.org/browse/MDEV-6769) DROP TRIGGER IF NOT EXIST binlogged on master but not on slave
</li></ul>
- [Revision #b739103](https://github.com/MariaDB/server/commit/b739103)
<span class="cstm-style datetime">2015-02-20 19:01:03 +0100</span>
<ul start="1"><li>[MDEV-7591](https://jira.mariadb.org/browse/MDEV-7591) master crashed when slave specfied a future position with semi-repl plugin
</li></ul>
- [Revision #22cf2f1](https://github.com/MariaDB/server/commit/22cf2f1)
<span class="cstm-style datetime">2015-02-20 16:37:02 +0100</span>
<ul start="1"><li>[MDEV-7482](https://jira.mariadb.org/browse/MDEV-7482) VIEW containing INTERVAL(...)
</li></ul>
- [Revision #f02fdb6](https://github.com/MariaDB/server/commit/f02fdb6)
<span class="cstm-style datetime">2015-02-20 22:11:45 -0500</span>
<ul start="1"><li>[MDEV-7615](https://jira.mariadb.org/browse/MDEV-7615): remove galera_sst_mode.result file
</li></ul>
- [Revision #f68ce68](https://github.com/MariaDB/server/commit/f68ce68)
<span class="cstm-style datetime">2015-02-20 17:51:33 -0500</span>
<ul start="1"><li>[MDEV-7615](https://jira.mariadb.org/browse/MDEV-7615): Remove --galera-sst-mode option from mysqldump
</li></ul>
- [Revision #2a798ce](https://github.com/MariaDB/server/commit/2a798ce)
<span class="cstm-style datetime">2015-02-20 17:45:18 -0500</span>
<ul start="1"><li>[MDEV-7615](https://jira.mariadb.org/browse/MDEV-7615): Remove --galera-sst-mode option from mysqldump
</li></ul>
- [Revision #360ff3b0](https://github.com/MariaDB/server/commit/360ff3b0)
<span class="cstm-style datetime">2015-02-20 17:29:03 -0500</span>
<ul start="1"><li>Fix for build failures on Power8
</li></ul>
- [Revision #c6e62ac](https://github.com/MariaDB/server/commit/c6e62ac)
<span class="cstm-style datetime">2015-02-20 17:21:15 -0500</span>
<ul start="1"><li>Fix for build failures on Power8
</li></ul>
- [Revision #8366ce4](https://github.com/MariaDB/server/commit/8366ce4)
<span class="cstm-style datetime">2015-02-20 18:48:29 +0200</span>
<ul start="1"><li>Fix test failure on labrador.
</li></ul>
- [Revision #7b6beef](https://github.com/MariaDB/server/commit/7b6beef)
<span class="cstm-style datetime">2015-02-20 14:21:27 +0100</span>
<ul start="1"><li>disable -Werror in MYSQL_MAINTAINER_MODE=ON until all plugins are ready
</li></ul>
- [Revision #6a1d443](https://github.com/MariaDB/server/commit/6a1d443)
<span class="cstm-style datetime">2015-02-20 14:10:25 +0100</span>
<ul start="1"><li>fix after 5.5 merge, debian packaging
</li></ul>
- [Revision #775528a](https://github.com/MariaDB/server/commit/775528a)
<span class="cstm-style datetime">2015-02-20 03:17:46 +0300</span>
<ul start="1"><li>[MDEV-7220](https://jira.mariadb.org/browse/MDEV-7220): Materialization strategy is not used for REPLACE ... SELECT
</li></ul>
- [Revision #69e5f0f](https://github.com/MariaDB/server/commit/69e5f0f)
<span class="cstm-style datetime">2015-02-19 17:48:23 -0500</span>
<ul start="1"><li>cleanup: remove duplicate declaration
</li></ul>
- [Revision #a174aae](https://github.com/MariaDB/server/commit/a174aae)
<span class="cstm-style datetime">2015-02-19 17:28:18 -0500</span>
<ul start="1"><li>cleanup: remove unused THD::COND_wsrep_thd
</li></ul>
- [Revision #1e6f46d](https://github.com/MariaDB/server/commit/1e6f46d)
<span class="cstm-style datetime">2015-02-19 17:28:18 -0500</span>
<ul start="1"><li>cleanup: remove unused THD::COND_wsrep_thd
</li></ul>
- [Revision #0f8b194](https://github.com/MariaDB/server/commit/0f8b194)
<span class="cstm-style datetime">2015-02-19 20:54:20 +0300</span>
<ul start="1"><li>[MDEV-6687](https://jira.mariadb.org/browse/MDEV-6687): Assertion `0' failed in Protocol::end_statement on query
</li></ul>
- [Revision #cf3b51b](https://github.com/MariaDB/server/commit/cf3b51b)
<span class="cstm-style datetime">2015-02-20 00:41:26 +0900</span>
<ul start="1"><li>Merge Spider 3.2.18
</li></ul>
- [Revision #004dd0a](https://github.com/MariaDB/server/commit/004dd0a)
<span class="cstm-style datetime">2015-02-19 15:43:27 +0100</span>
<ul start="1"><li>[MDEV-7568](https://jira.mariadb.org/browse/MDEV-7568): STOP SLAVE crashes the server
</li></ul>
- [Revision #c1ebb4a](https://github.com/MariaDB/server/commit/c1ebb4a)
<span class="cstm-style datetime">2015-02-19 11:28:03 +0100</span>
<ul start="1"><li>compiler warnings in spider
</li></ul>
- [Revision #16c01c7](https://github.com/MariaDB/server/commit/16c01c7)
<span class="cstm-style datetime">2015-02-19 10:26:52 +0100</span>
<ul start="1"><li>after merge: fix mroonga to compile and pass its tests
</li></ul>
- [Revision #d9175f3](https://github.com/MariaDB/server/commit/d9175f3)
<span class="cstm-style datetime">2015-02-19 01:25:31 +0100</span>
<ul start="1"><li>- Remove GCC warnings modified:   storage/connect/jsonudf.cpp   storage/connect/tabutil.h
</li></ul>
- [Revision #fcc6e12](https://github.com/MariaDB/server/commit/fcc6e12)
<span class="cstm-style datetime">2015-02-18 19:02:00 -0500</span>
<ul start="1"><li>[MDEV-7544](https://jira.mariadb.org/browse/MDEV-7544): Update global_suppressions to include warning related to "gvwstate.dat"
</li></ul>
- [Revision #564d41f](https://github.com/MariaDB/server/commit/564d41f)
<span class="cstm-style datetime">2015-02-19 00:59:02 +0100</span>
<ul start="1"><li>- Work on JSON and JSON UDF's modified:   storage/connect/json.cpp   storage/connect/jsonudf.cpp   storage/connect/tabjson.cpp
</li></ul>
- [Revision #a518cc4](https://github.com/MariaDB/server/commit/a518cc4)
<span class="cstm-style datetime">2015-02-18 18:59:01 -0500</span>
<ul start="1"><li>[MDEV-7544](https://jira.mariadb.org/browse/MDEV-7544): Update global_suppressions to include warning related to "gvwstate.dat"
</li></ul>
- [Revision #1645930](https://github.com/MariaDB/server/commit/1645930)
<span class="cstm-style datetime">2015-02-18 16:20:46 +0100</span>
<ul start="1"><li>5.6.23
</li></ul>
- [Revision #dfb001e](https://github.com/MariaDB/server/commit/dfb001e)
<span class="cstm-style datetime">2015-02-18 13:19:09 +0100</span>
<ul start="1"><li>percona-server-5.6.22-72.0
</li></ul>
- [Revision #865b83e](https://github.com/MariaDB/server/commit/865b83e)
<span class="cstm-style datetime">2015-02-18 14:07:13 +0200</span>
<ul start="1"><li>Fixed test failure seen on partition_innodb_plugin test case.
</li></ul>
- [Revision #2fb81b9](https://github.com/MariaDB/server/commit/2fb81b9)
<span class="cstm-style datetime">2015-02-18 11:25:59 +0200</span>
<ul start="1"><li>[MDEV-7408](https://jira.mariadb.org/browse/MDEV-7408): Cannot use a table containing special chars for InnoDB stopwords
</li></ul>
- [Revision #63905f1](https://github.com/MariaDB/server/commit/63905f1)
<span class="cstm-style datetime">2015-02-18 07:28:44 +0200</span>
<ul start="1"><li>Add forgotten test case change (add more).
</li></ul>
- [Revision #a1a32f8](https://github.com/MariaDB/server/commit/a1a32f8)
<span class="cstm-style datetime">2015-02-18 06:59:28 +0200</span>
<ul start="1"><li>Revert file space allocation change on row0merge.cc.
</li></ul>
- [Revision #44cf4d6](https://github.com/MariaDB/server/commit/44cf4d6)
<span class="cstm-style datetime">2015-02-17 18:07:56 +0100</span>
<ul start="1"><li>fix a case where automatic procedure grant was changing user's password
</li></ul>
- [Revision #22dae70](https://github.com/MariaDB/server/commit/22dae70)
<span class="cstm-style datetime">2015-02-17 20:07:12 +0300</span>
<ul start="1"><li>Added testcase for [MDEV-7193](https://jira.mariadb.org/browse/MDEV-7193): Incorrect Query Result (MySQL Bug 68897) ...
</li></ul>
- [Revision #f5dabd7](https://github.com/MariaDB/server/commit/f5dabd7)
<span class="cstm-style datetime">2015-02-17 13:34:27 +0900</span>
<ul start="1"><li>Update Mroonga to the latest version on 2015-02-17T13:34:27+0900
</li></ul>
- [Revision #fdd6c11](https://github.com/MariaDB/server/commit/fdd6c11)
<span class="cstm-style datetime">2015-02-13 12:57:11 +0100</span>
<ul start="1"><li>[MDEV-7419](https://jira.mariadb.org/browse/MDEV-7419) Function cli_safe_read not exported
</li></ul>
- [Revision #454beee](https://github.com/MariaDB/server/commit/454beee)
<span class="cstm-style datetime">2015-02-13 11:49:31 +0200</span>
<ul start="1"><li>[MDEV-6288](https://jira.mariadb.org/browse/MDEV-6288) :Innodb causes server crash after disk full, then can't ALTER TABLE any more
</li></ul>
- [Revision #2201aa6](https://github.com/MariaDB/server/commit/2201aa6)
<span class="cstm-style datetime">2015-02-12 17:23:28 +0100</span>
<ul start="1"><li>- Typo on the jsonudf.cpp name modified:   storage/connect/CMakeLists.txt
</li></ul>
- [Revision #356ae62](https://github.com/MariaDB/server/commit/356ae62)
<span class="cstm-style datetime">2015-02-12 15:44:44 +0200</span>
<ul start="1"><li>Crash during configure without development SSL libraries installed
</li></ul>
- [Revision #dcfe068](https://github.com/MariaDB/server/commit/dcfe068)
<span class="cstm-style datetime">2015-02-11 21:39:41 +0100</span>
<ul start="1"><li>- Adding json udf's. Making the second version of json tables. added:   storage/connect/jsonudf.cpp modified:   storage/connect/CMakeLists.txt   storage/connect/json.cpp   storage/connect/json.h   storage/connect/tabjson.cpp   storage/connect/tabjson.h
</li></ul>
- [Revision #13927f8](https://github.com/MariaDB/server/commit/13927f8)
<span class="cstm-style datetime">2015-02-11 18:32:40 +0100</span>
<ul start="1"><li>percona-server-5.5.41-37.0
</li></ul>
- [Revision #d996dc2](https://github.com/MariaDB/server/commit/d996dc2)
<span class="cstm-style datetime">2015-02-11 15:02:15 +0100</span>
<ul start="1"><li>[MDEV-7290](https://jira.mariadb.org/browse/MDEV-7290) please update MSI installer to include HeidiSQL 9.1
</li></ul>
- [Revision #56da625](https://github.com/MariaDB/server/commit/56da625)
<span class="cstm-style datetime">2015-02-10 15:15:27 +0200</span>
<ul start="1"><li>Improve InnoDB transaction lock output by providing number of table locks this transaction is currently holding and total number of table locks to the table where lock is held.
</li></ul>
- [Revision #a257801](https://github.com/MariaDB/server/commit/a257801)
<span class="cstm-style datetime">2015-02-10 16:01:03 +0400</span>
<ul start="1"><li>Fixing compilation failure in storage/connect when -DMYSQL_MAINTAINER_MODE=ON: disabling some CXX errors in storage/connect/CMakeLists.txt.
</li></ul>
- [Revision #63108dc](https://github.com/MariaDB/server/commit/63108dc)
<span class="cstm-style datetime">2015-02-10 12:26:21 +0100</span>
<ul start="1"><li>Fix the tree to work in git. Backport corresponing 10.1 changes.
</li></ul>
- [Revision #7588424](https://github.com/MariaDB/server/commit/7588424)
<span class="cstm-style datetime">2015-02-10 10:19:42 +0100</span>
<ul start="1"><li>restore a cross-compiling bit that was lost in a merge
</li></ul>
- [Revision #48e7c19](https://github.com/MariaDB/server/commit/48e7c19)
<span class="cstm-style datetime">2015-02-10 09:41:54 +0200</span>
<ul start="1"><li>Fix test failure innodb-mdev7046 on Windows. Test causes OS error printout from InnoDB.
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
- [Revision #3c097fd](https://github.com/MariaDB/server/commit/3c097fd)
<span class="cstm-style datetime">2015-02-08 19:47:26 +0100</span>
<ul start="1"><li>- Remove some GCC warnings modified:   storage/connect/ha_connect.cc
</li></ul>
- [Revision #919f40e](https://github.com/MariaDB/server/commit/919f40e)
<span class="cstm-style datetime">2015-02-08 22:38:19 +0400</span>
<ul start="1"><li>Audit plugin v1.2.0.
</li></ul>
- [Revision #96ba1f1](https://github.com/MariaDB/server/commit/96ba1f1)
<span class="cstm-style datetime">2015-02-08 18:17:29 +0100</span>
<ul start="1"><li>- Handle the use of date/time values when making queries for MYSQL or   ODBC. Was raised by 7549. modified:   storage/connect/ha_connect.cc   storage/connect/odbconn.cpp   storage/connect/tabodbc.cpp
</li></ul>
- [Revision #0d73bc1](https://github.com/MariaDB/server/commit/0d73bc1)
<span class="cstm-style datetime">2015-02-08 15:47:00 +0300</span>
<ul start="1"><li>[MDEV-7519](https://jira.mariadb.org/browse/MDEV-7519) debian / ubuntu packaging creation of plugin table (if not exists)
</li></ul>
- [Revision #35548d5](https://github.com/MariaDB/server/commit/35548d5)
<span class="cstm-style datetime">2015-02-07 11:33:52 +0100</span>
<ul start="1"><li>- Modify the connect_type_conv and connect_conv_size variables.   They were global (read-only) now they are session (not read-only) modified:   storage/connect/checklvl.h   storage/connect/ha_connect.cc   storage/connect/myconn.cpp   storage/connect/myutil.cpp   storage/connect/tabutil.cpp
</li></ul>
- [Revision #b9d616c](https://github.com/MariaDB/server/commit/b9d616c)
<span class="cstm-style datetime">2015-02-06 15:49:45 +0400</span>
<ul start="1"><li>[MDEV-7435](https://jira.mariadb.org/browse/MDEV-7435) Windows debug: Run-Time Check Failure #3 - The variable 'unused' is being used without being initialized. Fixed as it's done in 10.0.
</li></ul>
- [Revision #587c720](https://github.com/MariaDB/server/commit/587c720)
<span class="cstm-style datetime">2015-02-05 20:09:08 -0800</span>
<ul start="1"><li>Fixed bug [MDEV-7316](https://jira.mariadb.org/browse/MDEV-7316). The function table_cond_selectivity() should take into account that condition selectivity for some fields can be set to 0.
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
- [Revision #22367ba](https://github.com/MariaDB/server/commit/22367ba)
<span class="cstm-style datetime">2015-02-02 19:34:35 +0100</span>
<ul start="1"><li>- Add or correct some tracing code modified:   storage/connect/odbconn.cpp   storage/connect/tabodbc.cpp
</li></ul>
- [Revision #82f2be6](https://github.com/MariaDB/server/commit/82f2be6)
<span class="cstm-style datetime">2015-02-02 15:35:58 +0100</span>
<ul start="1"><li>- Fix a bug causing Insert into ODBC to fail when the column name is   UTF8 encoded. modified:   storage/connect/tabodbc.cpp
</li></ul>
- [Revision #6a78371](https://github.com/MariaDB/server/commit/6a78371)
<span class="cstm-style datetime">2015-02-01 12:16:30 +0100</span>
<ul start="1"><li>- Fix a bug causing UseCnc not being initialized for ODBC catalog tables.   This made errors by calling SQLConnect or SQLDriverConnect randomly   with incorrect parameters. modified:   storage/connect/tabodbc.cpp
</li></ul>
- [Revision #180b2be](https://github.com/MariaDB/server/commit/180b2be)
<span class="cstm-style datetime">2015-01-31 15:05:43 +0100</span>
<ul start="1"><li>- Add the possibility to establish an ODBC connection via SQLConnect (the   default being still to use SQLDriverConnect) modified:   storage/connect/ha_connect.cc   storage/connect/odbccat.h   storage/connect/odbconn.cpp   storage/connect/odbconn.h   storage/connect/tabodbc.cpp   storage/connect/tabodbc.h
</li></ul>
- [Revision #dfc7e95](https://github.com/MariaDB/server/commit/dfc7e95)
<span class="cstm-style datetime">2015-01-30 15:53:24 +0100</span>
<ul start="1"><li>[MDEV-7531](https://jira.mariadb.org/browse/MDEV-7531) Update 10.0.15 to 10.0.16 -&gt; Error 2003 (HY000) can't connect to MySql server.
</li></ul>
- [Revision #fd1ca70](https://github.com/MariaDB/server/commit/fd1ca70)
<span class="cstm-style datetime">2015-01-30 10:57:00 +0100</span>
<ul start="1"><li>- Enhance JSON tables handling. modified:   storage/connect/json.cpp   storage/connect/json.h   storage/connect/mysql-test/connect/r/json.result   storage/connect/mysql-test/connect/t/json.test   storage/connect/tabjson.cpp   storage/connect/tabjson.h
</li></ul>
- [Revision #5c30901](https://github.com/MariaDB/server/commit/5c30901)
<span class="cstm-style datetime">2015-01-29 21:10:45 +0100</span>
<ul start="1"><li>increase the version
</li></ul>
- [Revision #5f63c9c](https://github.com/MariaDB/server/commit/5f63c9c)
<span class="cstm-style datetime">2015-01-29 14:34:31 +0100</span>
<ul start="1"><li>recreate expired certificates for SSL tests
</li></ul>
- [Revision #1e227b8](https://github.com/MariaDB/server/commit/1e227b8)
<span class="cstm-style datetime">2015-01-29 12:12:29 +0100</span>
<ul start="1"><li>clarify the comment and trivial cleanups
</li></ul>
- [Revision #9033aa0](https://github.com/MariaDB/server/commit/9033aa0)
<span class="cstm-style datetime">2015-01-28 11:49:55 +0100</span>
<ul start="1"><li>[MDEV-6128](https://jira.mariadb.org/browse/MDEV-6128):[PATCH] mysqlcheck wrongly escapes '.' in table names
</li></ul>