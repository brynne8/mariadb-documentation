# MariaDB Galera Cluster 5.5.45 Changelog

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.45)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5545-release-notes)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5545-changelog)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 18 Aug 2015

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5545-release-notes).

The revision number links will take you to the revision's page on Github. On
Github you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #fe757e0](https://github.com/MariaDB/server/commit/fe757e0)
<span class="cstm-style datetime">2015-08-14 13:45:52 -0400</span>
<ul start="1"><li>Fix for some failing tests.
</li></ul>
- [Revision #c18e0da](https://github.com/MariaDB/server/commit/c18e0da)
<span class="cstm-style datetime">2015-08-14 00:01:18 -0400</span>
<ul start="1"><li>[MDEV-8617](https://jira.mariadb.org/browse/MDEV-8617): Multiple galera tests failures with --ps-protocol
</li></ul>
- [Revision #e998dff](https://github.com/MariaDB/server/commit/e998dff)
<span class="cstm-style datetime">2015-08-12 17:47:25 -0400</span>
<ul start="1"><li>[MDEV-8598](https://jira.mariadb.org/browse/MDEV-8598) : Failed MySQL DDL commands and Galera replication
</li></ul>
- [Revision #fa51f70](https://github.com/MariaDB/server/commit/fa51f70)
<span class="cstm-style datetime">2015-08-04 23:42:44 +0200</span>
<ul start="1"><li>correct the NULL-pointer test
</li></ul>
- [Revision #877de3a](https://github.com/MariaDB/server/commit/877de3a)
<span class="cstm-style datetime">2015-07-30 22:08:39 +0300</span>
<ul start="1"><li>[MDEV-8554](https://jira.mariadb.org/browse/MDEV-8554): Server crashes in base_list_iterator::next_fast ...
</li></ul>
- [Revision #1b0c81c](https://github.com/MariaDB/server/commit/1b0c81c)
<span class="cstm-style datetime">2015-08-01 15:02:14 +0200</span>
<ul start="1"><li>5.5.44-37.3
</li></ul>
- [Revision #96badb1](https://github.com/MariaDB/server/commit/96badb1)
<span class="cstm-style datetime">2015-07-31 22:09:46 +0200</span>
<ul start="1"><li>[MDEV-7821](https://jira.mariadb.org/browse/MDEV-7821) Server crashes in Item_func_group_concat::fix_fields on 2nd execution of PS
</li></ul>
- [Revision #409709e](https://github.com/MariaDB/server/commit/409709e)
<span class="cstm-style datetime">2015-07-31 20:33:10 +0200</span>
<ul start="1"><li>compilation error on windows
</li></ul>
- [Revision #79deefc](https://github.com/MariaDB/server/commit/79deefc)
<span class="cstm-style datetime">2015-07-31 12:31:37 +0200</span>
<ul start="1"><li>[MDEV-8340](https://jira.mariadb.org/browse/MDEV-8340) Add "mysqlbinlog --binlog-row-event-max-size" support for [MariaDB 5.5](/kb/en/what-is-mariadb-55/)
</li></ul>
- [Revision #4d5772c](https://github.com/MariaDB/server/commit/4d5772c)
<span class="cstm-style datetime">2015-07-31 10:13:01 +0200</span>
<ul start="1"><li>[MDEV-7810](https://jira.mariadb.org/browse/MDEV-7810) Wrong result on execution of a query as a PS (both 1st and further executions)
</li></ul>
- [Revision #2721d69](https://github.com/MariaDB/server/commit/2721d69)
<span class="cstm-style datetime">2015-07-28 19:11:53 +0200</span>
<ul start="1"><li>[MDEV-8352](https://jira.mariadb.org/browse/MDEV-8352) Increase Diffie-Helman modulus to 2048-bits
</li></ul>
- [Revision #bfe2689](https://github.com/MariaDB/server/commit/bfe2689)
<span class="cstm-style datetime">2015-07-31 13:13:39 +0400</span>
<ul start="1"><li>[MDEV-8379](https://jira.mariadb.org/browse/MDEV-8379) - SUSE mariadb patches
</li></ul>
- [Revision #360e597](https://github.com/MariaDB/server/commit/360e597)
<span class="cstm-style datetime">2015-07-31 12:06:29 +0300</span>
<ul start="1"><li>Make sure name buffer has string end marker on correct place.
</li></ul>
- [Revision #1ad294e](https://github.com/MariaDB/server/commit/1ad294e)
<span class="cstm-style datetime">2015-07-30 18:51:44 +0400</span>
<ul start="1"><li>[MDEV-7821](https://jira.mariadb.org/browse/MDEV-7821) - Server crashes in Item_func_group_concat::fix_fields on 2nd             execution of PS
</li></ul>
- [Revision #fa765a4](https://github.com/MariaDB/server/commit/fa765a4)
<span class="cstm-style datetime">2015-07-31 08:52:24 +0300</span>
<ul start="1"><li>[MDEV-6697](https://jira.mariadb.org/browse/MDEV-6697): Improve foreign keys warnings/errors
</li></ul>
- [Revision #e05cd97](https://github.com/MariaDB/server/commit/e05cd97)
<span class="cstm-style datetime">2015-07-29 05:58:45 +0300</span>
<ul start="1"><li>[MDEV-8524](https://jira.mariadb.org/browse/MDEV-8524): Improve error messaging when there is duplicate key or foreign key names
</li></ul>
- [Revision #392df76](https://github.com/MariaDB/server/commit/392df76)
<span class="cstm-style datetime">2015-07-23 12:50:58 +0400</span>
<ul start="1"><li>[MDEV-4017](https://jira.mariadb.org/browse/MDEV-4017) - GET_LOCK() with negative timeouts has strange behavior
</li></ul>
- [Revision #e40bc65](https://github.com/MariaDB/server/commit/e40bc65)
<span class="cstm-style datetime">2015-07-25 15:14:40 +0300</span>
<ul start="1"><li>Fixed memory loss detected on P8. This can happen when we call after_flush but never call after_rollback() or after_commit().
</li></ul>
- [Revision #7115341](https://github.com/MariaDB/server/commit/7115341)
<span class="cstm-style datetime">2015-07-23 14:57:12 +0300</span>
<ul start="1"><li>Fixed warnings and errors found by buildbot
</li></ul>
- [Revision #7a96702](https://github.com/MariaDB/server/commit/7a96702)
<span class="cstm-style datetime">2015-07-21 12:12:58 +0300</span>
<ul start="1"><li>[MDEV-8474](https://jira.mariadb.org/browse/MDEV-8474): InnoDB sets per-connection data unsafely
</li></ul>
- [Revision #af2f7ce](https://github.com/MariaDB/server/commit/af2f7ce)
<span class="cstm-style datetime">2015-07-19 22:51:19 -0400</span>
<ul start="1"><li>[MDEV-8464](https://jira.mariadb.org/browse/MDEV-8464) : ALTER VIEW not replicated in some cases
</li></ul>
- [Revision #00d3b20](https://github.com/MariaDB/server/commit/00d3b20)
<span class="cstm-style datetime">2015-07-17 00:06:27 +0300</span>
<ul start="1"><li>[MDEV-8432](https://jira.mariadb.org/browse/MDEV-8432) Slave cannot replicate signed integer-type values with high bit set to 1
</li></ul>
- [Revision #44de090](https://github.com/MariaDB/server/commit/44de090)
<span class="cstm-style datetime">2015-07-17 00:02:25 +0300</span>
<ul start="1"><li>[MDEV-8432](https://jira.mariadb.org/browse/MDEV-8432) Slave cannot replicate signed integer-type values with high bit set to 1
</li></ul>
- [Revision #bc30046](https://github.com/MariaDB/server/commit/bc30046)
<span class="cstm-style datetime">2015-06-26 14:48:22 +0300</span>
<ul start="1"><li>Fix for [MDEV-8301](https://jira.mariadb.org/browse/MDEV-8301);  Statistics for a thread could be counted twice in SHOW STATUS while thread was ending
</li></ul>
- [Revision #67c56ab](https://github.com/MariaDB/server/commit/67c56ab)
<span class="cstm-style datetime">2015-06-25 23:34:54 +0300</span>
<ul start="1"><li>Simple cleanups - Removing use of calls to current_thd - More DBUG_PRINT - Code style changes - Made some local functions static Ensure that calls to print_keyuse are locked with mutex to get all lines in same debug packet
</li></ul>
- [Revision #8c81575](https://github.com/MariaDB/server/commit/8c81575)
<span class="cstm-style datetime">2015-06-25 23:26:29 +0300</span>
<ul start="1"><li>Problem was that for cases like: SELECT ... WHERE XX IN (SELECT YY) this was transformed to something like: SELECT ... WHERE IF_EXISTS(SELECT ... HAVING XX=YY)
</li></ul>
- [Revision #2e941fe](https://github.com/MariaDB/server/commit/2e941fe)
<span class="cstm-style datetime">2015-06-25 23:18:48 +0300</span>
<ul start="1"><li>Fixed crashing bug when using ONLY_FULL_GROUP_BY in a stored procedure/trigger that is repeatedly executed. This is [MDEV-7601](https://jira.mariadb.org/browse/MDEV-7601), including it's sub tasks [MDEV-7594](https://jira.mariadb.org/browse/MDEV-7594), [MDEV-7555](https://jira.mariadb.org/browse/MDEV-7555), [MDEV-7590](https://jira.mariadb.org/browse/MDEV-7590), [MDEV-7581](https://jira.mariadb.org/browse/MDEV-7581), [MDEV-7589](https://jira.mariadb.org/browse/MDEV-7589)
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