# MariaDB Galera Cluster 5.5.60 Changelog

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.60)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5560-release-notes/)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5560-changelog/)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 3 May 2018

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5560-release-notes/). For changes in
MariaDB, see the [MariaDB 5.5.60 Changelog](/kb/en/mariadb-5560-changelog/).

The revision number links will take you to the revision's page on Github. On
Github you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #1ecd68d867](https://github.com/MariaDB/server/commit/1ecd68d867)
<span class="cstm-style datetime">2018-04-30 23:06:09 +0200</span>
<ul start="1"><li>Use after free in authentication
</li></ul>
- [Revision #ccad629d7e](https://github.com/MariaDB/server/commit/ccad629d7e)
<span class="cstm-style datetime">2018-04-30 13:50:59 +0200</span>
<ul start="1"><li>Bug#25471090: MYSQL USE AFTER FREE
</li></ul>
- [Revision #75dec85c2e](https://github.com/MariaDB/server/commit/75dec85c2e)
<span class="cstm-style datetime">2018-04-27 11:21:55 +0200</span>
<ul start="1"><li>Bug#25471090: MYSQL USE AFTER FREE
</li></ul>
- [Revision #231c02f7b9](https://github.com/MariaDB/server/commit/231c02f7b9)
<span class="cstm-style datetime">2018-04-24 13:58:42 +0300</span>
<ul start="1"><li>MariaDB adjustments.
</li></ul>
- [Revision #c2c61bbcce](https://github.com/MariaDB/server/commit/c2c61bbcce)
<span class="cstm-style datetime">2017-12-17 14:41:55 +0200</span>
<ul start="1"><li>Provider rollback for ineffective trx
</li></ul>
- <span class="cstm-style merge">Merge [Revision #a5001a2ad7](https://github.com/MariaDB/server/commit/a5001a2ad7) 2018-04-24 13:34:57 +0300 - Merge tag 'mariadb-5.5.60' into 5.5-galera</span>
- <span class="cstm-style merge">Merge [Revision #804a7e60d7](https://github.com/MariaDB/server/commit/804a7e60d7) 2018-03-14 10:29:47 +0200 - Merge pull request #637 from grooverdan/5.5-galera</span>
- [Revision #d3c0e34bdc](https://github.com/MariaDB/server/commit/d3c0e34bdc)
<span class="cstm-style datetime">2018-03-02 16:19:14 +1100</span>
<ul start="1"><li>[MDEV-8743](https://jira.mariadb.org/browse/MDEV-8743): protect myisam/aria MYI with O_CLOEXEC
</li></ul>
- [Revision #bbee025370](https://github.com/MariaDB/server/commit/bbee025370)
<span class="cstm-style datetime">2018-03-02 12:45:36 +1100</span>
<ul start="1"><li>[MDEV-8743](https://jira.mariadb.org/browse/MDEV-8743): O_CLOEXEC on innodb/xtradb temp files
</li></ul>
- [Revision #88fb8b2e36](https://github.com/MariaDB/server/commit/88fb8b2e36)
<span class="cstm-style datetime">2018-03-02 11:52:20 +1100</span>
<ul start="1"><li>[MDEV-8743](https://jira.mariadb.org/browse/MDEV-8743): protect myisam/aria MYD files and aria log files
</li></ul>
- [Revision #c54c490c59](https://github.com/MariaDB/server/commit/c54c490c59)
<span class="cstm-style datetime">2018-03-02 11:17:35 +1100</span>
<ul start="1"><li>[MDEV-8743](https://jira.mariadb.org/browse/MDEV-8743): O_CLOEXEC/SOCK_CLOEXEC defines for non-unix compatibility
</li></ul>
- [Revision #4ec7b84077](https://github.com/MariaDB/server/commit/4ec7b84077)
<span class="cstm-style datetime">2018-03-02 10:54:34 +1100</span>
<ul start="1"><li>[MDEV-8743](https://jira.mariadb.org/browse/MDEV-8743): use O_CLOEXEC MYSQL_LOG::open / TC_LOG_MMAP::open
</li></ul>
- [Revision #9629bca1f0](https://github.com/MariaDB/server/commit/9629bca1f0)
<span class="cstm-style datetime">2018-03-02 10:54:00 +1100</span>
<ul start="1"><li>[MDEV-8743](https://jira.mariadb.org/browse/MDEV-8743): use O_CLOEXEC (innodb/xtradb)
</li></ul>
- [Revision #09b25f8596](https://github.com/MariaDB/server/commit/09b25f8596)
<span class="cstm-style datetime">2018-03-01 16:35:46 +0100</span>
<ul start="1"><li>only allow SUPER user to modify wsrep_on
</li></ul>
- [Revision #7cec685758](https://github.com/MariaDB/server/commit/7cec685758)
<span class="cstm-style datetime">2018-01-30 05:37:22 -0800</span>
<ul start="1"><li>Bump wsrep patch version to 25.23
</li></ul>
- <span class="cstm-style merge">Merge [Revision #a8d64fbe16](https://github.com/MariaDB/server/commit/a8d64fbe16) 2018-01-24 10:34:25 +0200 - Merge pull request #570 from grooverdan/5.5-[MDEV-1044](https://jira.mariadb.org/browse/MDEV-1044)-backport-wsrep-no-new-processgroup</span>
- [Revision #2400b769c6](https://github.com/MariaDB/server/commit/2400b769c6)
<span class="cstm-style datetime">2016-11-21 16:20:10 -0500</span>
<ul start="1"><li>[MDEV-10442](https://jira.mariadb.org/browse/MDEV-10442): "Address already in use" on restart
</li></ul>
- [Revision #4132b1785a](https://github.com/MariaDB/server/commit/4132b1785a)
<span class="cstm-style datetime">2018-01-23 12:05:10 -0500</span>
<ul start="1"><li>bump the VERSION</li></ul>