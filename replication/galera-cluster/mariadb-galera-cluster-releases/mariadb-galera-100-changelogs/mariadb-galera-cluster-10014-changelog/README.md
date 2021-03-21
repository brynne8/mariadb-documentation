# MariaDB Galera Cluster 10.0.14 Changelog

The most recent [MariaDB Galera Cluster 10.0](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 10.0.38](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10038-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/10.0)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/10.0.14)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10014-release-notes/)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-changelogs/mariadb-galera-cluster-10014-changelog/)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 16 Oct 2014

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10014-release-notes/).

The revision number links will take you to the revision's page on Launchpad. On
Launchpad you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #3899](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3899)
  <span class="cstm-style datetime">Wed 2014-10-08 13:30:45 -0400</span>
<ul start="1"><li>[MDEV-6481](https://jira.mariadb.org/browse/MDEV-6481): Yum Upgrade on CentOS 6.5 causes instant crash of MariaDB/Galera
</li></ul>
- [Revision #3898](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3898)
  <span class="cstm-style datetime">Fri 2014-10-03 21:22:41 -0400</span>
<ul start="1"><li>[MDEV-6807](https://jira.mariadb.org/browse/MDEV-6807): InnoDB: Assertion failure in file lock0lock.cc (lock != ctx-&gt;wait_lock)
</li></ul>
- [Revision #3897](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3897)
  <span class="cstm-style datetime">Tue 2014-09-30 18:06:15 -0400</span>
<ul start="1"><li>bzr merge -r4123..4144 codership/5.6
</li></ul>
- [Revision #3896](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3896) [merge]
  <span class="cstm-style datetime">Sun 2014-09-28 20:43:56 -0400</span>
<ul start="1"><li>bzr merge -rtag:mariadb-10.0.14 maria/10.0/ ([MariaDB 10.0.14](/kb/en/mariadb-10014-release-notes/) merge)
</li></ul>
- [Revision #3895](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3895)
  <span class="cstm-style datetime">Wed 2014-09-24 12:17:29 -0400</span>
<ul start="1"><li>Moved wsrep_slave_threads to optional settings.
</li></ul>
- [Revision #3894](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3894)
  <span class="cstm-style datetime">Tue 2014-09-23 14:33:27 -0400</span>
<ul start="1"><li>Updated config files:
<ul start="1"><li>Removed QC restriction
</li><li>Added bind-address Fixed file permissions for wsrep_sst_rsync.sh
</li><li>Removed some unnecessary files.
</li></ul>
</li></ul>
- [Revision #3893](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3893)
  <span class="cstm-style datetime">Mon 2014-09-22 12:15:44 -0400</span>
<ul start="1"><li>[MDEV-6740](https://jira.mariadb.org/browse/MDEV-6740) : Galera crash in rpl_sql_thread_info/cached_charset_compare
</li></ul>
- [Revision #3892](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3892)
  <span class="cstm-style datetime">Wed 2014-09-17 14:59:39 -0400</span>
<ul start="1"><li>[MDEV-6447](https://jira.mariadb.org/browse/MDEV-6447): Galera: Enable QC
</li></ul>
- [Revision #3891](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3891)
  <span class="cstm-style datetime">Wed 2014-09-17 09:54:04 -0400</span>
<ul start="1"><li>Reverting version change to match the version of supporting packages available on buildbot.
</li></ul>
- [Revision #3890](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3890)
  <span class="cstm-style datetime">Tue 2014-09-16 12:58:35 -0400</span>
<ul start="1"><li>Updated mysqld--help test and result ([MDEV-6717](https://jira.mariadb.org/browse/MDEV-6717), [MDEV-6659](https://jira.mariadb.org/browse/MDEV-6659)).
</li></ul>
- [Revision #3889](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3889)
  <span class="cstm-style datetime">Tue 2014-09-16 12:42:17 -0400</span>
<ul start="1"><li>[MDEV-6659](https://jira.mariadb.org/browse/MDEV-6659): mysqld --help --verbose initializes wsrep
</li></ul>
- [Revision #3888](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3888)
  <span class="cstm-style datetime">Tue 2014-09-09 19:19:12 -0400</span>
<ul start="1"><li>Minor improvements in mtr and wsrep test files.
</li></ul>
- [Revision #3887](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3887)
  <span class="cstm-style datetime">Tue 2014-09-09 13:43:01 -0400</span>
<ul start="1"><li>[MDEV-6717](https://jira.mariadb.org/browse/MDEV-6717) : wsrep_data_home_dir should default to @@datadir
</li></ul>
- [Revision #3886](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3886)
  <span class="cstm-style datetime">Tue 2014-09-09 09:25:47 -0400</span>
<ul start="1"><li>[MDEV-6699](https://jira.mariadb.org/browse/MDEV-6699) : wsrep_node_name not automatically set to hostname
</li></ul>
- [Revision #3885](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3885)
  <span class="cstm-style datetime">Mon 2014-09-08 21:21:37 -0400</span>
<ul start="1"><li>[MDEV-6667](https://jira.mariadb.org/browse/MDEV-6667) : Improved handling of wsrep-new-cluster option
</li></ul>
- [Revision #3884](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3884)
  <span class="cstm-style datetime">Mon 2014-09-08 14:01:41 -0400</span>
<ul start="1"><li>Bumping server version. (10.0.14-galera)
</li></ul>
- [Revision #3883](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3883)
  <span class="cstm-style datetime">Wed 2014-09-03 18:25:49 +0300</span>
<ul start="1"><li>[MDEV-6651](https://jira.mariadb.org/browse/MDEV-6651): MariaDB galera cluster crashes in file row0mysql.cc line 684  DELETE FROM ports WHERE ports.id = 'f37aa3fe-ab99-4d0f-a566-6cd3169d7516'  where table ports have foreign keys. 
</li></ul>