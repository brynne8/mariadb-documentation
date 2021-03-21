# MariaDB Galera 5.5.28a Changelog

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.28a) | 
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-5528a-release-notes/) | 
<strong>Changelog</strong> |
[Overview of Galera](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 21 Dec 2012

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-5528a-release-notes/).

The revision number links will take you to the revision's page on Launchpad. On
Launchpad you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #3365](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3365)
<span class="cstm-style datetime">Thu 2012-12-20 12:34:37 +0100</span>
<ul start="1"><li>update test cases and results
</li></ul>
- [Revision #3364](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3364) [merge]
<span class="cstm-style datetime">Thu 2012-12-13 18:01:50 +0400</span>
<ul start="1"><li>merging.
</li><li>[Revision #3356.1.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3356.1.2) [merge]
<span class="cstm-style datetime">Fri 2012-11-30 13:36:29 +0200</span>
<ul start="1"><li>References: [Bug #1066784](https://bugs.launchpad.net/bugs/1066784) - Merged with [MariaDB 5.5.28](/kb/en/mariadb-5528-release-notes/)a bzr merge -r tag:mariadb-5.5.28a lp:maria/5.5 ...no conflicts
</li><li>This merges in [MariaDB 5.5.28](/kb/en/mariadb-5528-release-notes/)a:
<ul start="1"><li>[MariaDB 5.5.28a Release Notes](/kb/en/mariadb-5528a-release-notes/)
</li><li>[MariaDB 5.5.28a Changelog](/kb/en/mariadb-5528a-changelog/)
</li></ul>
</li></ul>
</li><li>[Revision #3356.1.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3356.1.1)
<span class="cstm-style datetime">Wed 2012-11-28 17:38:32 +0200</span>
<ul start="1"><li>References: [Bug #1066784](https://bugs.launchpad.net/bugs/1066784) - Merged revisions 3810-3827 from lp:codership-mysql
</li></ul>
</li></ul>
- [Revision #3363](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3363)
<span class="cstm-style datetime">Thu 2012-12-13 17:48:46 +0400</span>
<ul start="1"><li>[MDEV-507](https://jira.mariadb.org/browse/MDEV-507) Galera Deb/RPM packages.         Add new wsrep-related files to the DEB packages.
</li></ul>
- [Revision #3362](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3362)
<span class="cstm-style datetime">Thu 2012-11-29 14:50:52 +0400</span>
<ul start="1"><li>[MDEV-3893](https://jira.mariadb.org/browse/MDEV-3893) mariadb-galera-server deb package cannot be installed on a mysql-free machine.         Fixed templates for messages.
</li></ul>
- [Revision #3361](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3361)
<span class="cstm-style datetime">Wed 2012-11-28 17:15:46 +0400</span>
<ul start="1"><li>[MDEV-507](https://jira.mariadb.org/browse/MDEV-507) deb/rpm packages for Galera.         Ubuntu 'control' file fixed.
</li></ul>
- [Revision #3360](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3360)
<span class="cstm-style datetime">Tue 2012-11-27 16:32:01 +0400</span>
<ul start="1"><li>MDEV 507 deb/rpm packages for galera builds.         lindian-overrides files fixed.
</li></ul>
- [Revision #3359](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3359)
<span class="cstm-style datetime">Tue 2012-11-27 16:22:10 +0400</span>
<ul start="1"><li>[MDEV-507](https://jira.mariadb.org/browse/MDEV-507) deb/rpm packages for galera builds.         If settings are not suitable for the WSREP, just         turn it off and keep working.
</li></ul>
- [Revision #3358](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3358)
<span class="cstm-style datetime">Mon 2012-11-19 13:01:38 +0400</span>
<ul start="1"><li>[MDEV-507](https://jira.mariadb.org/browse/MDEV-507) deb/rpm packages for galera builds.         Debian packaging, part II.         Changes in the set of package-related files. Some were removed, some renamed,         as we only keep the mariadb-galera-server package.
</li></ul>
- [Revision #3357](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3357)
<span class="cstm-style datetime">Sun 2012-11-18 18:07:02 +0400</span>
<ul start="1"><li>MDEV 507 deb/rpm packages for galera builds.         Debian/Ubuntu packages fixed.         The mariadb-server-5.5 and mariadb-server packages became         mariadb-galera-server-5.5 and mariadb-galera-server respectively.         The rest of packages are removed from the build.         This patch reflects only files that were changed.         Second part of this patch has only file renaming/deletions.
</li></ul>
- [Revision #3356](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3356) [merge]
<span class="cstm-style datetime">Wed 2012-10-24 23:13:43 +0300</span>
<ul start="1"><li>References [Bug #1066784](https://bugs.launchpad.net/bugs/1066784) - bzr merge lp:maria/5.5 (rev: 3562)
</li><li>This merges in [MariaDB 5.5.28](/kb/en/mariadb-5528-release-notes/):
<ul start="1"><li>[MariaDB 5.5.28 Release Notes](/kb/en/mariadb-5528-release-notes/)
</li><li>[MariaDB 5.5.28 Changelog](/kb/en/mariadb-5528-changelog/)
</li></ul>
</li></ul>
- [Revision #3355](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3355)
<span class="cstm-style datetime">Tue 2012-10-23 22:38:11 +0300</span>
<ul start="1"><li>References [Bug #1066784](https://bugs.launchpad.net/bugs/1066784) merged with patch: bzr diff lp:codership-mysql/5.5 -r3795..3809
</li></ul>
- [Revision #3354](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3354)
<span class="cstm-style datetime">Thu 2012-09-20 09:35:22 +0300</span>
<ul start="1"><li>References [Bug #1051808](https://bugs.launchpad.net/bugs/1051808) [Bug #1049024](https://bugs.launchpad.net/bugs/1049024) [MDEV-541](https://jira.mariadb.org/browse/MDEV-541)        patched with: bzr diff lp:codership-mysql/5.5 -r3794..3795
</li></ul>
- [Revision #3353](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3353)
<span class="cstm-style datetime">Wed 2012-09-19 00:23:06 +0300</span>
<ul start="1"><li>References [Bug #1052668](https://bugs.launchpad.net/bugs/1052668) - DBUG macro issue in start_wsrep_THD merged fix from upstream: bzr diff lp:codership-mysql/5.5 -r3793..3794
</li></ul>
- [Revision #3352](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3352)
<span class="cstm-style datetime">Tue 2012-09-18 22:49:13 +0300</span>
<ul start="1"><li>References [Bug #1051808](https://bugs.launchpad.net/bugs/1051808) - merged with lp:codership-mysql 5.5.27 based trunk     patched with: bzr diff lp:codership-mysql/5.5 -r3790..3793
</li></ul>
- [Revision #3351](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3351)
<span class="cstm-style datetime">Mon 2012-09-17 17:42:14 +0200</span>
<ul start="1"><li>really disable embedded server in galera builds
</li></ul>
- [Revision #3350](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3350)
<span class="cstm-style datetime">Mon 2012-09-17 15:33:19 +0200</span>
<ul start="1"><li>[MDEV-507](https://jira.mariadb.org/browse/MDEV-507) deb/rpm packages for galera builds
</li><li>rpm part: only build the server rpm, not client or shared or anything else
</li></ul>
- [Revision #3349](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3349) [merge]
<span class="cstm-style datetime">Mon 2012-09-17 12:31:38 +0300</span>
<ul start="1"><li>References [Bug #1051808](https://bugs.launchpad.net/bugs/1051808) - merged with lp:maria/5.5 bzr merge lp:maria/5.5 ... Text conflict in CMakeLists.txt Text conflict in sql/mysqld.cc Text conflict in sql/sql_class.h Text conflict in sql/sql_truncate.cc 4 conflicts encountered.
</li><li>This merges in [MariadB 5.5.27](/kb/en/mariadb-5527-release-notes/):
<ul start="1"><li>[MariaDB 5.5.27 Release Notes](/kb/en/mariadb-5527-release-notes/)
</li><li>[MariaDB 5.5.27 Changelog](/kb/en/mariadb-5527-changelog/)
</li></ul>
</li></ul>
- [Revision #3348](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3348)
<span class="cstm-style datetime">Mon 2012-09-17 12:06:39 +0300</span>
<ul start="1"><li>References [Bug #1051808](https://bugs.launchpad.net/bugs/1051808) - merged with lp:codership-mysql 5.5.27 based trunk merged xtradb storage engine part
</li></ul>
- [Revision #3347](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3347)
<span class="cstm-style datetime">Mon 2012-09-17 11:34:57 +0300</span>
<ul start="1"><li>References [Bug #1051808](https://bugs.launchpad.net/bugs/1051808) - merged with lp:codership-mysql 5.5.27 based trunk patched with: bzr diff lp:codership-mysql/5.5 -r3779..3790
</li></ul>
- [Revision #3346](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3346)
<span class="cstm-style datetime">Sat 2012-09-08 09:51:16 +0200</span>
<ul start="1"><li>[MDEV-507](https://jira.mariadb.org/browse/MDEV-507) deb/rpm packages for galera builds
</li><li>rpm part.
</li></ul>
- [Revision #3345](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3345)
<span class="cstm-style datetime">Tue 2012-09-04 22:13:46 +0200</span>
<ul start="1"><li>Fixes for galera build  - compile with WITH_WSREP on - fix package name
</li></ul>