# MariaDB Galera Cluster 10.0.7 Changelog

The most recent [MariaDB Galera Cluster 10.0](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 10.0.38](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-10038-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/10.0)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/10.0.7) |
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-1007-release-notes) |
<strong>Changelog</strong> |
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 24 Feb 2014

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-100-release-notes/mariadb-galera-cluster-1007-release-notes).

The revision number links will take you to the revision's page on Launchpad. On
Launchpad you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #3803](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3803)
  <span class="cstm-style datetime">Wed 2014-02-19 13:59:10 -0500</span>
<ul start="1"><li>Fixed install_macros.cmake to set the correct destination for documentation.
</li></ul>
- [Revision #3802](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3802)
  <span class="cstm-style datetime">Fri 2014-02-14 17:03:44 -0500</span>
<ul start="1"><li>mysqld_safe could not start server as it failed trying to perform wsrep position recovery.
</li><li>Fixed by correcting the erroneous mysqld command by properly quoting it.
</li></ul>
- [Revision #3801](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3801)
  <span class="cstm-style datetime">Thu 2014-02-13 09:33:04 -0500</span>
<ul start="1"><li>Fixes in debian distribution files.
</li></ul>
- [Revision #3800](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3800)
  <span class="cstm-style datetime">Tue 2014-02-11 15:03:42 +0200</span>
<ul start="1"><li>[MDEV-5644](https://jira.mariadb.org/browse/MDEV-5644): Assertion failure during lock_cancel_waiting_and_release.
</li></ul>
- [Revision #3799](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3799)
  <span class="cstm-style datetime">Mon 2014-02-10 09:22:24 -0500</span>
<ul start="1"><li>[MDEV-5626](https://jira.mariadb.org/browse/MDEV-5626) : Cannot install InnoDB/XtraDB plugin (undefined symbol: wsrep_md5_update)
</li></ul>
- [Revision #3798](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3798)
  <span class="cstm-style datetime">Fri 2014-02-07 09:14:43 -0500</span>
<ul start="1"><li>Dummy empty revision (to trigger bb).
</li></ul>
- [Revision #3797](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3797)
  <span class="cstm-style datetime">Mon 2014-02-03 17:14:38 -0500</span>
<ul start="1"><li>Fix for main.commit test.
</li></ul>
- [Revision #3796](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3796)
  <span class="cstm-style datetime">Thu 2014-01-30 20:27:01 -0500</span>
<ul start="1"><li>Fixed debian dist file names.
</li><li>Fixed failing test results.
</li><li>Updated tztime.cc ([Bug #1161432](https://bugs.launchpad.net/bugs/1161432)).
</li></ul>
- [Revision #3795](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3795)
  <span class="cstm-style datetime">Thu 2014-01-30 12:45:38 -0500</span>
<ul start="1"><li>Merged revisions: 3431, 3435..3457, 3459, 3460 from maria-5.5-galera.
</li><li>Fixed Debian/Ubuntu dist files.
</li><li>Fixed some compiler warnings.
</li></ul>
- [Revision #3794](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3794)
  <span class="cstm-style datetime">Wed 2014-01-29 08:54:17 +0200</span>
<ul start="1"><li>Fixed issue on wsrep_kill_victim mutexing order error. Furthermore, fixed merge errors found on mysql-test suite testing.
</li></ul>
- [Revision #3793](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3793)
  <span class="cstm-style datetime">Tue 2014-01-28 09:48:51 +0200</span>
<ul start="1"><li>Fixed issue with extra status lines Wsrep_local_bf_aborts at SHOW GLOBAL STATUS  LIKE 'x'; where x != wsrep_local_bf_aborts by changing it as SHOW_SIMPLE_FUNC from SHOW_FUNC.
</li></ul>
- [Revision #3792](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3792) [merge]
  <span class="cstm-style datetime">Sat 2014-01-25 11:02:49 +0200</span>
<ul start="1"><li>Merge MariaDB-10.0.7 revision 3961.
</li></ul>
- [Revision #3791](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3791)
  <span class="cstm-style datetime">Mon 2014-01-20 12:17:31 +0200</span>
<ul start="1"><li>Fixed issue with retrying autocommitted transactions. We might need to clean up the explain structure in this case.
</li></ul>
- [Revision #3790](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3790)
  <span class="cstm-style datetime">Fri 2014-01-17 13:55:09 +0200</span>
<ul start="1"><li>Fixed one compiler warning in wsrep_applier.cc
</li></ul>
- [Revision #3789](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3789)
  <span class="cstm-style datetime">Fri 2014-01-17 13:28:43 +0200</span>
<ul start="1"><li>Added missing files
</li></ul>
- [Revision #3788](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3788) [merge]
  <span class="cstm-style datetime">Wed 2013-12-04 10:32:43 +0200</span>
<ul start="1"><li>merge with [MariaDB 5.6](/kb/en/what-is-mariadb-56/) <code class="fixed" style="white-space:pre-wrap">bzr merge lp:maria --rtag:mariadb-10.0.6</code> and a number of fixes to make this buildable.
</li><li>Run also few short multi-master high conflict rate tests, with no issues
</li></ul>
- [Revision #3787](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3787)
  <span class="cstm-style datetime">Wed 2013-11-27 22:20:32 +0200</span>
<ul start="1"><li>fixes for wsrep-5.5 merges
</li></ul>
- [Revision #3786](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3786)
  <span class="cstm-style datetime">Wed 2013-11-27 14:45:32 +0200</span>
<ul start="1"><li>Ported all remaining storage/innobase changes from lp:codership-mysql/5.6, up tp revision #4021 This is same level as wsrep_25.1 milestone
</li><li>Note: stotage/xtaradb is not upgraded yet
</li></ul>
- [Revision #3785](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3785)
  <span class="cstm-style datetime">Wed 2013-11-27 01:10:29 +0200</span>
<ul start="1"><li>diffed in fix in #3953 from lp:codership-mysql/5.6
</li></ul>
- [Revision #3784](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3784)
  <span class="cstm-style datetime">Wed 2013-11-27 00:54:21 +0200</span>
<ul start="1"><li>diffed in the fix from revision #3937 from lp:codership-mysql/5.6
</li></ul>
- [Revision #3783](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3783)
  <span class="cstm-style datetime">Wed 2013-11-27 00:44:10 +0200</span>
<ul start="1"><li>bzr merge -c 3921 lp:codership-mysql/5.6
</li></ul>
- [Revision #3782](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3782)
  <span class="cstm-style datetime">Wed 2013-11-27 00:18:44 +0200</span>
<ul start="1"><li>bzr merge -r3904..3928 lp:codership-mysql/5.5 This is now otherwise on level wsrep-25.9, but storage/innobase has not been fully merged wsrep-5.5 is not good source for that, so we probably have to cherry pick innodb changes from wsrep-5.6
</li></ul>
- [Revision #3781](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3781)
  <span class="cstm-style datetime">Tue 2013-11-26 22:09:14 +0200</span>
<ul start="1"><li>bzr merge -r3895..3903 lp:codership-mysql/5.5  This is just before 5.5.34 merge in wsrep-5.5 branch
</li></ul>
- [Revision #3780](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3780)
  <span class="cstm-style datetime">Tue 2013-11-26 17:03:14 +0200</span>
<ul start="1"><li>merge from lp:codership-mysql/5.5 rev #3895
</li></ul>
- [Revision #3779](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3779)
  <span class="cstm-style datetime">Tue 2013-11-26 16:48:30 +0200</span>
<ul start="1"><li>Merges from lp:codership-mysql/5.5 up to rev #3893, this changes to wsrep API #24
</li></ul>
- [Revision #3778](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3778)
  <span class="cstm-style datetime">Wed 2013-11-06 00:29:37 +0200</span>
<ul start="1"><li>bzr merge -r3890..3891 lp:codership-mysql/5.5
</li></ul>
- [Revision #3777](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3777)
  <span class="cstm-style datetime">Wed 2013-11-06 00:02:22 +0200</span>
<ul start="1"><li>bzr merge -r3889..3890 lp:codership-mysql/5.5
</li></ul>
- [Revision #3776](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3776)
  <span class="cstm-style datetime">Tue 2013-10-15 12:03:57 +0300</span>
<ul start="1"><li>Fixed performance schema instrumentation on galera and added correct mutexing when cancelling waiting trx on InnoDB
</li></ul>
- [Revision #3775](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3775)
  <span class="cstm-style datetime">Mon 2013-10-14 11:54:27 +0300</span>
<ul start="1"><li>Fix incorrect merge
</li></ul>
- [Revision #3774](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3774)
  <span class="cstm-style datetime">Fri 2013-10-11 16:51:26 +0300</span>
<ul start="1"><li>Fix temporary table search
</li></ul>
- [Revision #3773](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3773)
  <span class="cstm-style datetime">Fri 2013-10-11 12:28:13 +0300</span>
<ul start="1"><li>Merge fix for DDL handling
</li></ul>
- [Revision #3772](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3772)
  <span class="cstm-style datetime">Mon 2013-10-07 20:18:58 +0300</span>
<ul start="1"><li>Added correct mutexing on trx handling.
</li></ul>
- [Revision #3771](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3771)
  <span class="cstm-style datetime">Mon 2013-10-07 11:35:19 +0300</span>
<ul start="1"><li>Merge fixes, now at level 3430 in mariadb-galera-5.5
</li></ul>
- [Revision #3770](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3770)
  <span class="cstm-style datetime">Mon 2013-10-07 09:43:19 +0300</span>
<ul start="1"><li>Merged revisions 3425..3430 from mariadb-galera-5.5
</li></ul>
- [Revision #3769](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3769)
  <span class="cstm-style datetime">Mon 2013-10-07 08:57:23 +0300</span>
<ul start="1"><li>Merged revisions 3418..3424 from mariadb-galera-5.5
</li></ul>
- [Revision #3768](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3768)
  <span class="cstm-style datetime">Mon 2013-10-07 00:18:26 +0300</span>
<ul start="1"><li>Merged revisions 3411..3417 from mariadb-galera-5.5
</li></ul>
- [Revision #3767](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3767)
  <span class="cstm-style datetime">Sun 2013-10-06 23:59:20 +0300</span>
<ul start="1"><li>Merged revisions 3409..3411 from mariadb-galera-5.5
</li></ul>
- [Revision #3766](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3766)
  <span class="cstm-style datetime">Sun 2013-10-06 23:54:18 +0300</span>
<ul start="1"><li>Merged #3909 from mariadb-galera-5.5
</li></ul>
- [Revision #3765](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3765)
  <span class="cstm-style datetime">Thu 2013-09-26 14:10:47 +0300</span>
<ul start="1"><li>Fixed merge error on rollback to savepoint
</li></ul>
- [Revision #3764](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3764)
  <span class="cstm-style datetime">Wed 2013-09-25 10:42:05 +0300</span>
<ul start="1"><li>After merge fixes
</li></ul>
- [Revision #3763](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3763)
  <span class="cstm-style datetime">Mon 2013-09-09 10:38:58 +0300</span>
<ul start="1"><li>Merge fix.
</li></ul>
- [Revision #3762](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3762)
  <span class="cstm-style datetime">Wed 2013-09-04 09:54:40 +0300</span>
<ul start="1"><li>Fix merge error
</li></ul>
- [Revision #3761](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3761)
  <span class="cstm-style datetime">Wed 2013-09-04 08:47:05 +0300</span>
<ul start="1"><li>Fixed merge errors and XA prepare
</li></ul>
- [Revision #3760](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3760) [merge]
  <span class="cstm-style datetime">Tue 2013-09-03 17:50:36 +0300</span>
<ul start="1"><li>Merge 10.0 to galera-10.0
</li></ul>
- [Revision #3759](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3759)
  <span class="cstm-style datetime">Tue 2013-07-16 12:09:38 +0300</span>
<ul start="1"><li>Initial fixes after mariadb 10 merge, basic replication works now
</li></ul>
- [Revision #3758](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3758) [merge]
  <span class="cstm-style datetime">Sat 2013-07-13 13:30:03 +0300</span>
<ul start="1"><li>Merged with lp:maria revision #3766
</li></ul>
- [Revision #3757](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-10.0-galera/revision/3757)
  <span class="cstm-style datetime">Sat 2013-07-13 13:01:13 +0300</span>
<ul start="1"><li>Initial merge result with mariaDB 10: lp:maria
</li></ul>