# MariaDB Galera Cluster 10.0.33 Changelog

The most recent [MariaDB Galera Cluster 10.0](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 10.0.38](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10038-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/10.0)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/10.0.33)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10033-release-notes)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10033-changelog)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 14 Nov 2017

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10033-release-notes).
For changes made in MariaDB, see the [MariaDB 10.0.33 Changelog](/kb/en/mariadb-10033-changelog/).

The revision number links will take you to the revision's page on Github. On
Github you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #9572bbdc37](https://github.com/MariaDB/server/commit/9572bbdc37)
<span class="cstm-style datetime">2017-11-08 17:22:11 +0200</span>
<ul start="1"><li>MW-388
</li></ul>
- [Revision #ca42ee0ff3](https://github.com/MariaDB/server/commit/ca42ee0ff3)
<span class="cstm-style datetime">2017-09-19 16:23:29 +0300</span>
<ul start="1"><li>Fix galera.galera_suspend_slave on FreeBSD
</li></ul>
- [Revision #0eaf24e842](https://github.com/MariaDB/server/commit/0eaf24e842)
<span class="cstm-style datetime">2017-09-22 10:06:59 +0200</span>
<ul start="1"><li>MW-410 Stability fix for test galera.galera_ftwrl
</li></ul>
- [Revision #736d75d455](https://github.com/MariaDB/server/commit/736d75d455)
<span class="cstm-style datetime">2017-09-18 16:22:32 +0300</span>
<ul start="1"><li>MW-406 Bumped up the wsrep patch version (5.6.37-25.21)
</li></ul>
- [Revision #b5802888e3](https://github.com/MariaDB/server/commit/b5802888e3)
<span class="cstm-style datetime">2017-09-07 16:45:21 +0300</span>
<ul start="1"><li>MW-402 cascading FK issues, 5.6 version
</li></ul>
- [Revision #c6251e36fc](https://github.com/MariaDB/server/commit/c6251e36fc)
<span class="cstm-style datetime">2017-08-10 14:04:55 +0300</span>
<ul start="1"><li>MW-399 Freeing wsrep_status_vars, before provider is released.
</li></ul>
- [Revision #6d783b6a76](https://github.com/MariaDB/server/commit/6d783b6a76)
<span class="cstm-style datetime">2017-11-08 14:15:54 +0200</span>
<ul start="1"><li>MW-388
</li></ul>
- [Revision #f4f2e8fa2a](https://github.com/MariaDB/server/commit/f4f2e8fa2a)
<span class="cstm-style datetime">2017-08-24 10:34:21 +0300</span>
<ul start="1"><li>MW-402 cascading FK issues
</li></ul>
- [Revision #e5e33db5fb](https://github.com/MariaDB/server/commit/e5e33db5fb)
<span class="cstm-style datetime">2017-11-08 13:55:09 +0200</span>
<ul start="1"><li>MW-394
</li></ul>
- [Revision #7cedebb99b](https://github.com/MariaDB/server/commit/7cedebb99b)
<span class="cstm-style datetime">2017-07-27 11:39:31 +0300</span>
<ul start="1"><li>MW-394
</li></ul>
- [Revision #b79407c5e9](https://github.com/MariaDB/server/commit/b79407c5e9)
<span class="cstm-style datetime">2017-07-21 10:56:25 +0200</span>
<ul start="1"><li>MW-388 Remove unnecessary conditions
</li></ul>
- [Revision #958ad5a880](https://github.com/MariaDB/server/commit/958ad5a880)
<span class="cstm-style datetime">2017-11-08 12:25:46 +0200</span>
<ul start="1"><li>MW-388 Fix conflict handling of SPs with DECLARE ... HANDLER
</li></ul>
- [Revision #76f1195f5b](https://github.com/MariaDB/server/commit/76f1195f5b)
<span class="cstm-style datetime">2017-07-11 12:55:03 +0200</span>
<ul start="1"><li>MW-388 Fix conflict handling of SPs with DECLARE ... HANDLER
</li></ul>
- <span class="cstm-style merge">Merge [Revision #3cecb1bab3](https://github.com/MariaDB/server/commit/3cecb1bab3) 2017-11-03 12:34:05 +0530 - Merge tag 'mariadb-10.0.33' into bb-10.0-galera</span>
- [Revision #53b4185e58](https://github.com/MariaDB/server/commit/53b4185e58)
<span class="cstm-style datetime">2017-10-25 11:45:30 -0400</span>
<ul start="1"><li>bump the VERSION
</li></ul>
- <span class="cstm-style merge">Merge [Revision #bb62a0ad99](https://github.com/MariaDB/server/commit/bb62a0ad99) 2017-10-04 07:59:25 +0300 - Merge pull request #458 from darkain/patch-1</span>
- [Revision #98e09ee4b6](https://github.com/MariaDB/server/commit/98e09ee4b6)
<span class="cstm-style datetime">2017-10-02 13:21:00 -0700</span>
<ul start="1"><li>[MDEV-13909](https://jira.mariadb.org/browse/MDEV-13909) Fix wsrep_sst_rsync fails on debian
</li></ul>
- [Revision #eba0120d8f](https://github.com/MariaDB/server/commit/eba0120d8f)
<span class="cstm-style datetime">2017-08-31 08:27:59 +0300</span>
<ul start="1"><li>Fix test failures on embedded server.
</li></ul>
- [Revision #f1af211499](https://github.com/MariaDB/server/commit/f1af211499)
<span class="cstm-style datetime">2017-08-21 07:11:04 +0300</span>
<ul start="1"><li>Add galera_admin to disabled.
</li></ul>
- [Revision #d20923debb](https://github.com/MariaDB/server/commit/d20923debb)
<span class="cstm-style datetime">2017-08-20 07:49:07 +0300</span>
<ul start="1"><li>Add more disabled test.
</li></ul>
- [Revision #449a996c6a](https://github.com/MariaDB/server/commit/449a996c6a)
<span class="cstm-style datetime">2017-08-19 07:52:31 +0300</span>
<ul start="1"><li>Add more disabled tests and one ignored warning.
</li></ul>
- [Revision #f7e1b99895](https://github.com/MariaDB/server/commit/f7e1b99895)
<span class="cstm-style datetime">2017-08-16 13:29:38 +0300</span>
<ul start="1"><li>Galera test fixes and add remaining test failures as disabled.</li></ul>