# MariaDB Galera Cluster 5.5.38 Changelog

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.38)
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5538-release-notes)
[Changelog](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-changelogs/mariadb-galera-cluster-5538-changelog)
[Overview of MariaDB Galera Cluster](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 25 Jun 2014

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5538-release-notes).

The revision number links will take you to the revision's page on Launchpad. On
Launchpad you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #3505](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3505)
  <span class="cstm-style datetime">Mon 2014-06-23 09:37:46 -0400</span>
<ul start="1"><li>Empty revision to trigger build on buildbot.
</li></ul>
- [Revision #3504](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3504)
  <span class="cstm-style datetime">Sun 2014-06-15 13:53:43 -0400</span>
<ul start="1"><li>[MDEV-6296](https://jira.mariadb.org/browse/MDEV-6296): runtime adjustment of wsrep_slave_threads creates threads but never removes them
</li></ul>
- [Revision #3503](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3503)
  <span class="cstm-style datetime">Wed 2014-06-11 17:13:03 -0400</span>
<ul start="1"><li>Modified patch for [Bug #1310875](https://bugs.launchpad.net/bugs/1310875).
</li></ul>
- [Revision #3502](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3502) [merge]
  <span class="cstm-style datetime">Tue 2014-06-10 18:41:53 -0400</span>
<ul start="1"><li>Merged changeset from codership-mysql/5.5.
</li><li>[Revision #3501.1.4](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3501.1.4)
   <span class="cstm-style datetime">Tue 2014-06-10 18:31:07 -0400</span>
<ul start="1"><li>Fixed a warning in mtr script. Updated wsrep.variables test.
</li></ul>
</li><li>[Revision #3501.1.3](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3501.1.3)
   <span class="cstm-style datetime">Tue 2014-06-10 17:35:44 -0400</span>
<ul start="1"><li>Fix for a segfault.
</li></ul>
</li><li>[Revision #3501.1.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3501.1.2)
   <span class="cstm-style datetime">Tue 2014-06-10 17:00:32 -0400</span>
<ul start="1"><li>bzr merge -r3985..3997 codership/5.5
</li></ul>
</li><li>[Revision #3501.1.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3501.1.1)
   <span class="cstm-style datetime">Tue 2014-06-10 16:33:57 -0400</span>
<ul start="1"><li>bzr merge -r3980..3984 codership/5.5
</li></ul>
</li></ul>
- [Revision #3501](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3501) [merge]
  <span class="cstm-style datetime">Tue 2014-06-10 16:04:26 -0400</span>
<ul start="1"><li>bzr merge -rtag:mariadb-5.5.38 maria/5.5
</li></ul>
- [Revision #3500](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3500)
  <span class="cstm-style datetime">Fri 2014-06-06 13:49:10 -0400</span>
<ul start="1"><li>Updated default load option groups.
</li></ul>
- [Revision #3499](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3499)
  <span class="cstm-style datetime">Thu 2014-06-05 23:40:32 -0400</span>
<ul start="1"><li>Modified mtr script to skip inclusion of 'galera' test suites if galera library is not specified or found.
</li></ul>
- [Revision #3498](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3498)
  <span class="cstm-style datetime">Thu 2014-05-29 21:03:10 -0400</span>
<ul start="1"><li>Added rsync to galera server's debian/rpm dependency list.
</li></ul>
- [Revision #3497](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3497)
  <span class="cstm-style datetime">Tue 2014-05-27 11:04:42 -0400</span>
<ul start="1"><li>s/#if/#ifdef
</li></ul>
- [Revision #3496](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3496)
  <span class="cstm-style datetime">Tue 2014-05-27 09:07:19 -0400</span>
<ul start="1"><li>Removing rsync from the debian build dependency list.
</li></ul>
- [Revision #3495](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3495)
  <span class="cstm-style datetime">Sun 2014-05-25 00:23:17 -0400</span>
<ul start="1"><li>Setting the "Standards-Version" in Debian control file back to 3.8.3.
</li></ul>
- [Revision #3494](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3494)
  <span class="cstm-style datetime">Sun 2014-05-25 00:07:25 -0400</span>
<ul start="1"><li>[MDEV-6211](https://jira.mariadb.org/browse/MDEV-6211): MariaDB-Galera-server uses 'socat', but 'socat' is   not in the dependency list
</li></ul>
- [Revision #3493](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3493)
  <span class="cstm-style datetime">Mon 2014-05-12 12:45:02 -0400</span>
<ul start="1"><li>[MDEV-5925](https://jira.mariadb.org/browse/MDEV-5925): New mariadb-galera-test packages
</li></ul>
- [Revision #3492](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3492)
  <span class="cstm-style datetime">Thu 2014-05-08 14:45:00 -0400</span>
<ul start="1"><li>[MDEV-6206](https://jira.mariadb.org/browse/MDEV-6206): wsrep_slave_threads subtracts from max_connections
</li></ul>
- [Revision #3491](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3491)
  <span class="cstm-style datetime">Sat 2014-05-03 23:53:19 -0400</span>
<ul start="1"><li>Fix for build failure when WITH_WSREP=OFF.
</li></ul>
- [Revision #3490](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3490)
  <span class="cstm-style datetime">Sat 2014-05-03 12:58:40 -0400</span>
<ul start="1"><li>Added galera, wsrep suites to the default mtr suite list.
</li></ul>
- [Revision #3489](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3489)
  <span class="cstm-style datetime">Sat 2014-05-03 09:18:11 -0400</span>
<ul start="1"><li>[MDEV-6204](https://jira.mariadb.org/browse/MDEV-6204): wsrep_sst_rsync timeout when lsof is not installed
</li></ul>
- [Revision #3488](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3488)
  <span class="cstm-style datetime">Thu 2014-05-01 19:19:48 -0400</span>
<ul start="1"><li>[MDEV-6196](https://jira.mariadb.org/browse/MDEV-6196) MTR: Do not hardcode path for libgalera_smm.so
</li></ul>
- [Revision #3487](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3487)
  <span class="cstm-style datetime">Mon 2014-04-28 10:33:22 -0400</span>
<ul start="1"><li>[MDEV-6148](https://jira.mariadb.org/browse/MDEV-6148) : Updating auto_increment_offset_func.result.
</li></ul>