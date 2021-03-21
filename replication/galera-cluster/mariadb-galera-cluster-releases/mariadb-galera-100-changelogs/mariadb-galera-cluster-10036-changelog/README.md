# MariaDB Galera Cluster 10.0.36 Changelog

The most recent [MariaDB Galera Cluster 10.0](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 10.0.38](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10038-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/10.0)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/10.0.36)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10036-release-notes/)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10036-changelog/)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong>  7 Aug 2018

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10036-release-notes/).
For changes made in MariaDB, see the [MariaDB 10.0.36 Changelog](/kb/en/mariadb-10036-changelog/).

The revision number links will take you to the revision's page on Github. On
Github you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #5d90717cc9](https://github.com/MariaDB/server/commit/5d90717cc9)
<span class="cstm-style datetime">2018-08-04 11:28:25 +0300</span>
<ul start="1"><li>Add wsrep.cnf
</li></ul>
- [Revision #ea0356e1ad](https://github.com/MariaDB/server/commit/ea0356e1ad)
<span class="cstm-style datetime">2018-08-03 16:43:32 +0300</span>
<ul start="1"><li>Add galera library dependency directly to test case.
</li></ul>
- [Revision #9808e23a7a](https://github.com/MariaDB/server/commit/9808e23a7a)
<span class="cstm-style datetime">2018-08-03 13:44:30 +0300</span>
<ul start="1"><li>MariaDB adjustments.
</li></ul>
- [Revision #3f0cd66a2b](https://github.com/MariaDB/server/commit/3f0cd66a2b)
<span class="cstm-style datetime">2018-07-16 09:42:53 +0200</span>
<ul start="1"><li>Also include InnoDB undo tablespaces in rsync sst
</li></ul>
- [Revision #62e290923e](https://github.com/MariaDB/server/commit/62e290923e)
<span class="cstm-style datetime">2018-07-16 09:41:37 +0200</span>
<ul start="1"><li>Put one filter per line in wsrep_sst_rsync.sh
</li></ul>
- [Revision #639bd1c71f](https://github.com/MariaDB/server/commit/639bd1c71f)
<span class="cstm-style datetime">2018-07-06 11:53:43 +0200</span>
<ul start="1"><li>galera#505 mtr test
</li></ul>
- [Revision #1d414d9491](https://github.com/MariaDB/server/commit/1d414d9491)
<span class="cstm-style datetime">2018-03-01 17:07:07 +0200</span>
<ul start="1"><li>codership/galera#501 Check cluster weight in galera_pc_weight test
</li></ul>
- [Revision #40f6bcb856](https://github.com/MariaDB/server/commit/40f6bcb856)
<span class="cstm-style datetime">2018-08-02 20:35:44 +0300</span>
<ul start="1"><li>Add missing WSREP(thd) condition and remove unnecessary DBUG_RETURN.
</li></ul>
- <span class="cstm-style merge">Merge [Revision #9b29bda0d6](https://github.com/MariaDB/server/commit/9b29bda0d6) 2018-08-02 13:13:21 +0300 - Merge remote-tracking branch 'origin/5.5-galera' into 10.0-galera</span>
- [Revision #e88e26b424](https://github.com/MariaDB/server/commit/e88e26b424)
<span class="cstm-style datetime">2018-05-23 14:13:11 +0200</span>
<ul start="1"><li>Follow up to previous commit for codership/mysql-wsrep#332
</li></ul>
- [Revision #4d2b552369](https://github.com/MariaDB/server/commit/4d2b552369)
<span class="cstm-style datetime">2018-05-22 16:45:27 +0200</span>
<ul start="1"><li>Fix FK constraint violation in applier, after ALTER TABLE ADD FK
</li></ul>
- <span class="cstm-style merge">Merge [Revision #b286a9f823](https://github.com/MariaDB/server/commit/b286a9f823) 2018-08-01 10:58:38 +0300 - Merge tag 'mariadb-5.5.61' into 5.5-galera</span>
- [Revision #33e39f0682](https://github.com/MariaDB/server/commit/33e39f0682)
<span class="cstm-style datetime">2018-05-03 10:23:36 -0400</span>
<ul start="1"><li>bump the VERSION
</li></ul>
- <span class="cstm-style merge">Merge [Revision #c5a8583b31](https://github.com/MariaDB/server/commit/c5a8583b31) 2018-08-02 11:44:02 +0300 - Merge tag 'mariadb-10.0.36' into 10.0-galera</span>
- [Revision #c863159c32](https://github.com/MariaDB/server/commit/c863159c32)
<span class="cstm-style datetime">2018-07-24 14:54:50 +0300</span>
<ul start="1"><li>[MDEV-16799](https://jira.mariadb.org/browse/MDEV-16799): Test wsrep.variables crash at sql_class.cc:639 thd_get_ha_data
</li></ul>
- [Revision #f99fe68b4f](https://github.com/MariaDB/server/commit/f99fe68b4f)
<span class="cstm-style datetime">2018-07-19 21:05:36 +0300</span>
<ul start="1"><li>Fix compile error.
</li></ul>
- [Revision #c09d54924a](https://github.com/MariaDB/server/commit/c09d54924a)
<span class="cstm-style datetime">2018-07-19 15:13:31 +0300</span>
<ul start="1"><li>[MDEV-10564](https://jira.mariadb.org/browse/MDEV-10564): Galera `wsrep_debug` patch logs MySQL user credentials
</li></ul>
- <span class="cstm-style merge">Merge [Revision #71e0ba4ae6](https://github.com/MariaDB/server/commit/71e0ba4ae6) 2018-07-19 07:04:40 +0300 - Merge pull request #645 from grooverdan/10.0-wsrep_sst_common_bashism</span>
- [Revision #6aa22eaf62](https://github.com/MariaDB/server/commit/6aa22eaf62)
<span class="cstm-style datetime">2018-03-07 14:36:40 +1100</span>
<ul start="1"><li>wsrep_sst_common: fix per shellcheck
</li></ul>
- [Revision #cf648afd5b](https://github.com/MariaDB/server/commit/cf648afd5b)
<span class="cstm-style datetime">2018-06-14 15:12:13 +0200</span>
<ul start="1"><li>fix galera sst tests
</li></ul>
- [Revision #215d652c66](https://github.com/MariaDB/server/commit/215d652c66)
<span class="cstm-style datetime">2018-05-09 09:16:20 +0300</span>
<ul start="1"><li>[MDEV-15351](https://jira.mariadb.org/browse/MDEV-15351): wsrep_sst_xtrabackup is broken in 10.1.31
</li></ul>
- [Revision #82f26dafcb](https://github.com/MariaDB/server/commit/82f26dafcb)
<span class="cstm-style datetime">2018-03-12 15:23:58 +1100</span>
<ul start="1"><li>[MDEV-13968](https://jira.mariadb.org/browse/MDEV-13968): wsrep_log_error not defined until later in wsrep_sst_common
</li></ul>
- [Revision #fe3c4a4182](https://github.com/MariaDB/server/commit/fe3c4a4182)
<span class="cstm-style datetime">2017-10-24 20:59:54 +0200</span>
<ul start="1"><li>[MDEV-13968](https://jira.mariadb.org/browse/MDEV-13968) sst fails with "WSREP_SST_OPT_PORT: readonly variable"
</li></ul>
- [Revision #e6b31df6df](https://github.com/MariaDB/server/commit/e6b31df6df)
<span class="cstm-style datetime">2017-10-16 17:49:52 +0200</span>
<ul start="1"><li>[MDEV-13968](https://jira.mariadb.org/browse/MDEV-13968) sst fails with "WSREP_SST_OPT_PORT: readonly variable"
</li></ul>
- [Revision #24ab82e675](https://github.com/MariaDB/server/commit/24ab82e675)
<span class="cstm-style datetime">2018-03-12 15:14:15 +1100</span>
<ul start="1"><li>[MDEV-15541](https://jira.mariadb.org/browse/MDEV-15541): wsrep_sst_common - WSREP_SST_OPT_PORT set twice (--address and --port)
</li></ul>
- [Revision #2b35db5ac4](https://github.com/MariaDB/server/commit/2b35db5ac4)
<span class="cstm-style datetime">2018-03-07 13:10:01 +1100</span>
<ul start="1"><li>[MDEV-15496](https://jira.mariadb.org/browse/MDEV-15496): wsrep_sst_common - parse IPv6 correct
</li></ul>
- [Revision #0f80c014c1](https://github.com/MariaDB/server/commit/0f80c014c1)
<span class="cstm-style datetime">2018-05-09 11:22:26 -0400</span>
<ul start="1"><li>bump the VERSION
</li></ul>
- <span class="cstm-style merge">Merge [Revision #2bbc868c50](https://github.com/MariaDB/server/commit/2bbc868c50) 2018-05-09 10:05:14 +0300 - Merge pull request #710 from grooverdan/10.0-galera-[MDEV-8743](https://jira.mariadb.org/browse/MDEV-8743)-mysqld-socket-o_cloexec</span>
- [Revision #ccd566af20](https://github.com/MariaDB/server/commit/ccd566af20)
<span class="cstm-style datetime">2018-04-19 07:38:57 +1000</span>
<ul start="1"><li>[MDEV-8743](https://jira.mariadb.org/browse/MDEV-8743): mysqld port/socket - FD_CLOEXEC if no SOCK_CLOEXEC</li></ul>