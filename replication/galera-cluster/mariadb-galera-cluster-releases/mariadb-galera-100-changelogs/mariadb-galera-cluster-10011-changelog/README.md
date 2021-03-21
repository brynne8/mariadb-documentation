# MariaDB Galera Cluster 10.0.11 Changelog

The most recent [MariaDB Galera Cluster 10.0](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 10.0.38](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10038-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/10.0)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/10.0.11)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10011-release-notes)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10011-changelog)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 11 Jun 2014

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10010-release-notes).

The revision number links will take you to the revision's page on Launchpad. On
Launchpad you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #3843](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3843)
  <span class="cstm-style datetime">Mon 2014-06-09 16:13:30 -0400</span>
<ul start="1"><li>Fix for a debian build failure (cherry-picked from 10.0:r4231).
</li></ul>
- [Revision #3842](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3842)
  <span class="cstm-style datetime">Fri 2014-06-06 13:27:15 -0400</span>
<ul start="1"><li>[MDEV-6317](https://jira.mariadb.org/browse/MDEV-6317): Fix rsync SST method to transfer binlog state to the joiner
</li></ul>
- [Revision #3841](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3841)
  <span class="cstm-style datetime">Thu 2014-06-05 23:31:00 -0400</span>
<ul start="1"><li>Modified mtr script to skip inclusion of 'galera' test suites if galera library is not specified or found.
</li></ul>
- [Revision #3840](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3840)
  <span class="cstm-style datetime">Mon 2014-06-02 22:45:41 -0400</span>
<ul start="1"><li>Fix for wsrep_sst_xtrabackup-v2.sh script.
</li></ul>
- [Revision #3839](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3839)
  <span class="cstm-style datetime">Mon 2014-06-02 08:26:42 -0400</span>
<ul start="1"><li>Fixed a typo in debian control file.
</li></ul>
- [Revision #3838](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3838)
  <span class="cstm-style datetime">Thu 2014-05-29 21:02:17 -0400</span>
<ul start="1"><li>Added rsync to galera server's rpm dependency list.
</li></ul>
- [Revision #3837](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3837)
  <span class="cstm-style datetime">Thu 2014-05-29 15:39:29 -0400</span>
<ul start="1"><li>Added rsync to galera server's debian dependency list.
</li></ul>
- [Revision #3836](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3836)
  <span class="cstm-style datetime">Wed 2014-05-28 00:46:21 -0400</span>
<ul start="1"><li>[MDEV-6266](https://jira.mariadb.org/browse/MDEV-6266): Changing password fails on galera cluster
</li></ul>
- [Revision #3835](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3835)
  <span class="cstm-style datetime">Tue 2014-05-27 10:11:42 -0400</span>
<ul start="1"><li>Fixing a typo s/connection_tcpwrap_errors/connection_errors_tcpwrap, causing build to fail when HAVE_LIBWRAP is enabled.
</li></ul>
- [Revision #3834](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3834)
  <span class="cstm-style datetime">Tue 2014-05-27 09:06:04 -0400</span>
<ul start="1"><li>Removing rsync from the debian build dependency list.
</li></ul>
- [Revision #3833](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3833)
  <span class="cstm-style datetime">Sun 2014-05-25 00:18:26 -0400</span>
<ul start="1"><li>[MDEV-6211](https://jira.mariadb.org/browse/MDEV-6211): MariaDB-Galera-server uses 'socat', but 'socat' is not in the dependency list
</li></ul>
- [Revision #3832](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3832)
  <span class="cstm-style datetime">Thu 2014-05-22 18:31:04 -0400</span>
<ul start="1"><li>Merging changes from maria-5.5-galera and some test fixes.
</li></ul>
- [Revision #3831](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3831)
  <span class="cstm-style datetime">Wed 2014-05-21 18:10:43 -0400</span>
<ul start="1"><li>bzr merge -r4089..4091 codership/5.6
</li></ul>
- [Revision #3830](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3830)
  <span class="cstm-style datetime">Wed 2014-05-21 17:07:17 -0400</span>
<ul start="1"><li>bzr merge -r4065..4088 codership/5.6
</li></ul>
- [Revision #3829](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3829)
  <span class="cstm-style datetime">Wed 2014-05-21 16:03:58 -0400</span>
<ul start="1"><li>Fix for a segfault.
</li></ul>
- [Revision #3828](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3828)
  <span class="cstm-style datetime">Wed 2014-05-21 15:16:15 -0400</span>
<ul start="1"><li>Fix for a build failure.
</li></ul>
- [Revision #3827](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3827)
  <span class="cstm-style datetime">Wed 2014-05-21 15:04:13 -0400</span>
<ul start="1"><li>bzr merge -r3985..3991 codership/5.5
</li></ul>
- [Revision #3826](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3826)
  <span class="cstm-style datetime">Wed 2014-05-21 14:32:57 -0400</span>
<ul start="1"><li>bzr merge -r3968..3984 codership/5.5 (non-Innodb changes only).
</li></ul>
- [Revision #3825](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3825)
  <span class="cstm-style datetime">Wed 2014-05-21 12:09:31 -0400</span>
<ul start="1"><li>Updated wsrep.variables result.
</li></ul>
- [Revision #3824](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3824)
  <span class="cstm-style datetime">Wed 2014-05-21 11:59:33 -0400</span>
<ul start="1"><li>Added test for [MDEV-4953](https://jira.mariadb.org/browse/MDEV-4953).
</li></ul>
- [Revision #3823](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3823)
  <span class="cstm-style datetime">Wed 2014-05-21 11:23:59 -0400</span>
<ul start="1"><li>Fixed a segfault issue by initializing thd's system_thread_info in wsrep applier threads, introduced by [MDEV-6156](https://jira.mariadb.org/browse/MDEV-6156).
</li></ul>
- [Revision #3822](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3822) [merge]
  <span class="cstm-style datetime">Wed 2014-05-21 11:09:55 -0400</span>
<ul start="1"><li>bzr merge -r4209 maria/10.0.
</li></ul>
- [Revision #3821](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3821)
  <span class="cstm-style datetime">Mon 2014-05-12 12:14:27 -0400</span>
<ul start="1"><li>[MDEV-5942](https://jira.mariadb.org/browse/MDEV-5942) (Issue 1), [MDEV-5903](https://jira.mariadb.org/browse/MDEV-5903)
</li></ul>
- [Revision #3820](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3820)
  <span class="cstm-style datetime">Wed 2014-04-30 15:40:00 -0400</span>
<ul start="1"><li>[MDEV-6192](https://jira.mariadb.org/browse/MDEV-6192) [Warning] Failed to load slave replication state from table mysql.gtid_slave_pos: 1286: Unknown storage engine 'InnoDB'
</li></ul>