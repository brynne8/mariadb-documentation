# MariaDB Galera Cluster 5.5.40 Changelog

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.40)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5540-release-notes)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5540-changelog)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 17 Oct 2014

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5540-release-notes).

The revision number links will take you to the revision's page on Launchpad. On
Launchpad you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #3543](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3543)
  <span class="cstm-style datetime">Tue 2014-10-14 18:04:04 -0400</span>
<ul start="1"><li>empty patch
</li></ul>
- [Revision #3542](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3542)
  <span class="cstm-style datetime">Thu 2014-10-09 18:28:14 -0400</span>
<ul start="1"><li>bzr merge -r4015..4026 codership/5.5
</li></ul>
- [Revision #3541](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3541) [merge]
  <span class="cstm-style datetime">Thu 2014-10-09 17:25:08 -0400</span>
<ul start="1"><li>bzr merge -rtag:mariadb-5.5.40 maria/5.5 ([MariaDB 5.5.40](/kb/en/mariadb-5540-release-notes/))
</li></ul>
- [Revision #3540](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3540)
  <span class="cstm-style datetime">Wed 2014-09-24 12:16:09 -0400</span>
<ul start="1"><li>Moved wsrep_slave_threads to optional settings.
</li></ul>
- [Revision #3539](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3539)
  <span class="cstm-style datetime">Tue 2014-09-23 14:03:13 -0400</span>
<ul start="1"><li>Updated config files:
<ul start="1"><li>Removed QC restriction
</li><li>Added bind-address Fixed file permissions for wsrep_sst_rsync.sh.
</li></ul>
</li></ul>
- [Revision #3538](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3538)
  <span class="cstm-style datetime">Wed 2014-09-17 14:39:43 -0400</span>
<ul start="1"><li>[MDEV-6447](https://jira.mariadb.org/browse/MDEV-6447): addendum, moving QC code within HAVE_QUERY_CACHE.
</li></ul>
- [Revision #3537](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3537)
  <span class="cstm-style datetime">Wed 2014-09-17 14:12:00 -0400</span>
<ul start="1"><li>[MDEV-6447](https://jira.mariadb.org/browse/MDEV-6447): Galera: Enable QC
</li></ul>
- [Revision #3536](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3536)
  <span class="cstm-style datetime">Wed 2014-09-17 09:53:06 -0400</span>
<ul start="1"><li>Reverting version change to match the version of supporting packages available on buildbot.
</li></ul>
- [Revision #3535](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3535)
  <span class="cstm-style datetime">Tue 2014-09-16 12:55:29 -0400</span>
<ul start="1"><li>Updated mysqld--help test and result ([MDEV-6717](https://jira.mariadb.org/browse/MDEV-6717), [MDEV-6659](https://jira.mariadb.org/browse/MDEV-6659)).
</li></ul>
- [Revision #3534](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3534)
  <span class="cstm-style datetime">Tue 2014-09-09 19:05:25 -0400</span>
<ul start="1"><li>Minor improvements in mtr and wsrep test files.
</li></ul>
- [Revision #3533](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3533)
  <span class="cstm-style datetime">Tue 2014-09-09 13:41:22 -0400</span>
<ul start="1"><li>[MDEV-6717](https://jira.mariadb.org/browse/MDEV-6717) : wsrep_data_home_dir should default to @@datadir
</li></ul>
- [Revision #3532](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3532)
  <span class="cstm-style datetime">Tue 2014-09-09 09:18:35 -0400</span>
<ul start="1"><li>[MDEV-6699](https://jira.mariadb.org/browse/MDEV-6699) : wsrep_node_name not automatically set to hostname
</li></ul>
- [Revision #3531](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3531)
  <span class="cstm-style datetime">Mon 2014-09-08 13:58:43 -0400</span>
<ul start="1"><li>Bumping server version. (5.5.40-galera)
</li></ul>
- [Revision #3530](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3530)
  <span class="cstm-style datetime">Wed 2014-09-03 18:51:02 +0300</span>
<ul start="1"><li>[MDEV-6651](https://jira.mariadb.org/browse/MDEV-6651): MariaDB galera cluster crashes in file row0mysql.cc line 684 DELETE FROM ports WHERE ports.id = 'f37aa3fe-ab99-4d0f-a566-6cd3169d7516' where table ports have foreign keys.
</li></ul>
- [Revision #3529](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3529)
  <span class="cstm-style datetime">Thu 2014-08-28 23:42:45 -0400</span>
<ul start="1"><li>[MDEV-6659](https://jira.mariadb.org/browse/MDEV-6659): mysqld --help --verbose initializes wsrep
</li></ul>
- [Revision #3528](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3528)
  <span class="cstm-style datetime">Tue 2014-08-26 16:14:46 -0400</span>
<ul start="1"><li>Switched wsrep_causal_reads ON for galera test suite.
</li></ul>
- [Revision #3527](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3527)
  <span class="cstm-style datetime">Tue 2014-08-26 15:56:03 -0400</span>
<ul start="1"><li>[MDEV-6646](https://jira.mariadb.org/browse/MDEV-6646) : global.wsrep_causal_reads no longer honored
</li></ul>
- [Revision #3526](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3526)
  <span class="cstm-style datetime">Mon 2014-08-25 17:03:17 -0400</span>
<ul start="1"><li>[MDEV-6636](https://jira.mariadb.org/browse/MDEV-6636) : Merged fixes for [Bug #1167368](https://bugs.launchpad.net/bugs/1167368) and [Bug #1250805](https://bugs.launchpad.net/bugs/1250805).
</li></ul>