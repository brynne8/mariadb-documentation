# MariaDB Galera Cluster 5.5.57 Changelog

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.57)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5557-release-notes)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5557-changelog)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 26 Jul 2017

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5557-release-notes). For changes in
MariaDB, see the [MariaDB 5.5.57 Changelog](/kb/en/mariadb-5557-changelog/).

The revision number links will take you to the revision's page on Github. On
Github you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #d9675a10d5](https://github.com/MariaDB/server/commit/d9675a10d5)
<span class="cstm-style datetime">2017-07-24 20:24:03 +0300</span>
<ul start="1"><li>Remove unneeded code.
</li></ul>
- [Revision #eec6417e05](https://github.com/MariaDB/server/commit/eec6417e05)
<span class="cstm-style datetime">2017-07-24 16:14:27 +0300</span>
<ul start="1"><li>Apply galera patches to XtraDB storage engine and remove one debug output.
</li></ul>
- [Revision #2aaed4489f](https://github.com/MariaDB/server/commit/2aaed4489f)
<span class="cstm-style datetime">2017-07-24 15:43:45 +0300</span>
<ul start="1"><li>Fix regression on galera.partition test case by commenting the problematic condition.
</li></ul>
- [Revision #07f8360f17](https://github.com/MariaDB/server/commit/07f8360f17)
<span class="cstm-style datetime">2017-07-24 11:06:42 +0300</span>
<ul start="1"><li>[MDEV-10379](https://jira.mariadb.org/browse/MDEV-10379): Failing assertion: xid_seqno &gt; trx_sys_cur_xid_seqno
</li></ul>
- [Revision #e5c488a49b](https://github.com/MariaDB/server/commit/e5c488a49b)
<span class="cstm-style datetime">2017-07-21 08:29:52 +0300</span>
<ul start="1"><li>Fix failing test case.
</li></ul>
- [Revision #970859a599](https://github.com/MariaDB/server/commit/970859a599)
<span class="cstm-style datetime">2017-05-24 14:46:25 +0300</span>
<ul start="1"><li>MW-383 Bumped wsrep patch version
</li></ul>
- [Revision #be416cfa3b](https://github.com/MariaDB/server/commit/be416cfa3b)
<span class="cstm-style datetime">2017-03-13 22:45:42 +0100</span>
<ul start="1"><li>MW-86 Removed unnecessary wsrep_sync_wait before processing SQLCOM_REPLACE
</li></ul>
- [Revision #34853fa793](https://github.com/MariaDB/server/commit/34853fa793)
<span class="cstm-style datetime">2017-03-13 15:35:04 +0100</span>
<ul start="1"><li>MW-86 Additional wsrep_sync_wait coverage
</li></ul>
- [Revision #a82611771b](https://github.com/MariaDB/server/commit/a82611771b)
<span class="cstm-style datetime">2017-03-08 13:08:21 +0100</span>
<ul start="1"><li>MW-86 Add separate wsrep_sync_wait bitmask value for SHOW commands
</li></ul>
- [Revision #519e4322e1](https://github.com/MariaDB/server/commit/519e4322e1)
<span class="cstm-style datetime">2017-05-08 23:12:51 +0300</span>
<ul start="1"><li>MW-369 FK fixes
</li></ul>
- [Revision #6326f0eac6](https://github.com/MariaDB/server/commit/6326f0eac6)
<span class="cstm-style datetime">2017-05-05 11:09:01 +0300</span>
<ul start="1"><li>MW-322 CTAS fix
</li></ul>
- [Revision #20ab1665af](https://github.com/MariaDB/server/commit/20ab1665af)
<span class="cstm-style datetime">2017-05-05 10:55:45 +0300</span>
<ul start="1"><li>MW-322 Fix compilation error with debug build
</li></ul>
- [Revision #dbda504275](https://github.com/MariaDB/server/commit/dbda504275)
<span class="cstm-style datetime">2017-07-20 13:52:22 +0300</span>
<ul start="1"><li>Fix merge error and compiler warning.
</li></ul>
- [Revision #48b7245bf2](https://github.com/MariaDB/server/commit/48b7245bf2)
<span class="cstm-style datetime">2017-04-27 20:28:22 +0300</span>
<ul start="1"><li>MW-369 - merged fix for FK issue from 5.6-v25 branch
</li></ul>
- [Revision #a4bc8db216](https://github.com/MariaDB/server/commit/a4bc8db216)
<span class="cstm-style datetime">2017-04-27 19:51:18 +0300</span>
<ul start="1"><li>MW-322 - CTAS fix merged to 5.5-v25 branch
</li></ul>
- <span class="cstm-style merge">Merge [Revision #a481de30bb](https://github.com/MariaDB/server/commit/a481de30bb) 2017-07-20 08:56:09 +0300 - Merge tag 'mariadb-5.5.57' into 5.5-galera</span>
- [Revision #e8a2a75121](https://github.com/MariaDB/server/commit/e8a2a75121)
<span class="cstm-style datetime">2017-06-20 14:29:25 +0530</span>
<ul start="1"><li>[MDEV-11036](https://jira.mariadb.org/browse/MDEV-11036) Add link wsrep_sst_rsync_wan -&gt; wsrep_sst_rsync
</li></ul>
- [Revision #9df17790f2](https://github.com/MariaDB/server/commit/9df17790f2)
<span class="cstm-style datetime">2017-05-03 13:10:17 -0400</span>
<ul start="1"><li>bump the VERSION</li></ul>