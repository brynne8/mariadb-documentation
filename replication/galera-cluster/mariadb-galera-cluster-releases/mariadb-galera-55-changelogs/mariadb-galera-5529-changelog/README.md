# MariaDB Galera 5.5.29 Changelog

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.29) |
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-5529-release-notes) |
<strong>Changelog</strong> |
[Overview of Galera](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 5 Mar 2013

For the highlights of this release, see the [release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-5529-release-notes).

The revision number links will take you to the revision's page on Launchpad. On Launchpad you can view more details of the revision and view diffs of the code modified in that revision.

- [Revision #3390](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3390)
  <span class="cstm-style datetime">Tue 2013-03-05 00:01:20 +0200</span>
<ul start="1"><li>References: [Bug #1144911](https://bugs.launchpad.net/bugs/1144911) - merged fix for prepared statement processing from upstream. Merged fix is revision 3853 in lp:codership/codership-mysql/5.5-23
</li></ul>
- [Revision #3389](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3389)
  <span class="cstm-style datetime">Mon 2013-03-04 23:01:36 +0200</span>
<ul start="1"><li>References: [MDEV-4211](https://jira.mariadb.org/browse/MDEV-4211) - appended format description event for TOI replication write set, FD carries binlog checksum algorithm
</li></ul>
- [Revision #3388](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3388)
  <span class="cstm-style datetime">Sun 2013-03-03 03:22:48 +0400</span>
<ul start="1"><li>[MDEV-4232](https://jira.mariadb.org/browse/MDEV-4232) : percona.innodb_sys_index fails due to a wrong version_comment
</li></ul>
- [Revision #3387](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3387)
  <span class="cstm-style datetime">Sat 2013-03-02 12:23:08 +0200</span>
<ul start="1"><li>References: [Bug #1136303](https://bugs.launchpad.net/bugs/1136303) - adapting wsrep status variable usage according to wsrep provider version 2.2 behavior
</li></ul>
- [Revision #3386](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3386)
  <span class="cstm-style datetime">Thu 2013-02-28 21:25:56 -0500</span>
<ul start="1"><li>Removed the obsolete instructions from the MySQL 5.1 manual. Instead provide a link to [https://kb.askmonty.org/en/compiling-mariadb-from-source/](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source)
</li></ul>
- [Revision #3385](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3385)
  <span class="cstm-style datetime">Thu 2013-02-28 21:25:04 -0500</span>
<ul start="1"><li>Added a MariaDB Galera Cluster section to the beginning of the README.
</li></ul>
- [Revision #3384](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3384)
  <span class="cstm-style datetime">Tue 2013-02-26 22:19:54 +0200</span>
<ul start="1"><li>References: [Bug #1084702](https://bugs.launchpad.net/bugs/1084702), [Bug #1019473](https://bugs.launchpad.net/bugs/1019473)
</li><li>Merged revisions 3851-3852 from lp:codership/codership-mysql/5.5-23
</li></ul>
- [Revision #3383](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3383)
  <span class="cstm-style datetime">Tue 2013-02-26 01:03:21 +0200</span>
<ul start="1"><li>References: [MDEV-4179](https://jira.mariadb.org/browse/MDEV-4179), [Bug #1130888](https://bugs.launchpad.net/bugs/1130888), [Bug #1019473](https://bugs.launchpad.net/bugs/1019473) Merged revisions 3847-3850 from lp:codership/codership-mysql/5.5-23
</li></ul>
- [Revision #3382](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3382)
  <span class="cstm-style datetime">Mon 2013-02-25 23:05:31 +0400</span>
<ul start="1"><li>[MDEV-4202](https://jira.mariadb.org/browse/MDEV-4202): innodb.innodb-autoinc fails due to missing wsrep-specific            variable in the result file
</li></ul>
- [Revision #3381](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3381)
  <span class="cstm-style datetime">Thu 2013-02-21 10:16:47 +0200</span>
<ul start="1"><li>References [MDEV-4185](https://jira.mariadb.org/browse/MDEV-4185) - thread terminate was blocked for non-wsrep threads
</li></ul>
- [Revision #3380](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3380)
  <span class="cstm-style datetime">Sun 2013-02-17 00:22:40 +0200</span>
<ul start="1"><li>References [MDEV-4176](https://jira.mariadb.org/browse/MDEV-4176) Avoiding ha_kill_query for aborts initiated by replicator
</li></ul>
- [Revision #3379](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3379)
  <span class="cstm-style datetime">Fri 2013-02-15 15:51:02 +0200</span>
<ul start="1"><li>References [MDEV-4136](https://jira.mariadb.org/browse/MDEV-4136) Fixes to stop wsrep replicator when thread pool scheduler is in use
</li></ul>
- [Revision #3378](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3378)
  <span class="cstm-style datetime">Thu 2013-02-07 19:01:19 +0100</span>
<ul start="1"><li>restore changes that were lost in the merge
</li></ul>
- [Revision #3377](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3377)
  <span class="cstm-style datetime">Thu 2013-02-07 17:40:32 +0200</span>
<ul start="1"><li>References: [MDEV-4142](https://jira.mariadb.org/browse/MDEV-4142) Merged revision 3846 from lp:codership-mysql/5.5-23
</li></ul>
- [Revision #3376](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3376)
  <span class="cstm-style datetime">Wed 2013-02-06 00:46:10 +0200</span>
<ul start="1"><li>wsrep build scripts
</li></ul>
- [Revision #3375](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3375)
  <span class="cstm-style datetime">Wed 2013-02-06 00:10:54 +0200</span>
<ul start="1"><li>References [Bug #1115708](https://bugs.launchpad.net/bugs/1115708) - merged innodb wsrep changes to xtradb between revisions 3809...3845
</li></ul>
- [Revision #3374](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3374)
  <span class="cstm-style datetime">Tue 2013-02-05 22:48:40 +0200</span>
<ul start="1"><li>References [Bug #1115708](https://bugs.launchpad.net/bugs/1115708) - merged with wsrep branch, revision 3845 bzr merge -r3840..3845 lp:codership/codership-mysql/5.5-23
</li></ul>
- [Revision #3373](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3373) [merge]
  <span class="cstm-style datetime">Tue 2013-02-05 20:19:47 +0200</span>
<ul start="1"><li>References [Bug #1115708](https://bugs.launchpad.net/bugs/1115708) - merged with lp:mariadb/5.5 revision 3657
</li><li>[Revision #3334.1.323](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.323)
   <span class="cstm-style datetime">Mon 2013-02-04 15:43:26 +0100</span>
<ul start="1"><li>[MDEV-4127](https://jira.mariadb.org/browse/MDEV-4127) :  Export additional symbols when building RPM, to enable both recompiling mysqli or odbc from sources in addition to drop-in replacement functionality.
</li><li>The case in question is compiling mysqli from sources, that needs client_errors via ER() macro.
</li><li>Previously, we exported it as mysql_client_errors (compatibly to Fedora's style symbol renaming, see [MDEV-3842](https://jira.mariadb.org/browse/MDEV-3842)). However, if MariaDB header files are used when compiling mysqli, client_errors needs to be exported with its original name.
</li></ul>
</li><li>[Revision #3334.1.322](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.322)
   <span class="cstm-style datetime">Sun 2013-02-03 02:53:57 +0400</span>
<ul start="1"><li>[MDEV-4028](https://jira.mariadb.org/browse/MDEV-4028) - Converted rdiff files to uniform [MDEV-11](https://jira.mariadb.org/browse/MDEV-11) - Modifed tests and result files to use explicit column lists           in INSERT and SELECT statements
</li></ul>
</li><li>[Revision #3334.1.321](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.321)
   <span class="cstm-style datetime">Wed 2013-01-30 17:25:02 +0100</span>
<ul start="1"><li>[MDEV-4113](https://jira.mariadb.org/browse/MDEV-4113): Assertion (group-&gt;connection_count &gt; 0) fails with Percona server in replication test.
</li><li>Assertion happens in replication thread during THD destruction, when thread calls my_sync(), which in turn calls  thd_wait_begin() callback. Connection count can be 0, because the counter was decremented before THD destructor.  This assertion currently reproducible only in Percona server  and not in MariaDB, due to differences in replication code.
</li><li>Fixed by moving  code to decrement connection counter after the THD destructor.
</li></ul>
</li><li>[Revision #3334.1.320](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.320)
   <span class="cstm-style datetime">Tue 2013-01-29 12:27:31 +0100</span>
<ul start="1"><li>more changes for fedora18
</li></ul>
</li><li>[Revision #3334.1.319](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.319)
   <span class="cstm-style datetime">Tue 2013-01-29 10:46:05 +0100</span>
<ul start="1"><li>fix 'compat' rpm for fedora18
</li></ul>
</li><li>[Revision #3334.1.318](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.318)
   <span class="cstm-style datetime">Mon 2013-01-28 17:24:50 +0100</span>
<ul start="1"><li>fix embedded build with for cmake 2.6.2 (older cmake could not handle IF(NOT MATCHES)
</li></ul>
</li><li>[Revision #3334.1.317](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.317)
   <span class="cstm-style datetime">Mon 2013-01-28 15:13:39 +0200</span>
<ul start="1"><li>Fix for [MDEV-3948](https://jira.mariadb.org/browse/MDEV-3948), and backport of the following collection of fixes and backports
    from [MariaDB 10.0](/kb/en/what-is-mariadb-100/).
</li><li>The bug in [MDEV-3948](https://jira.mariadb.org/browse/MDEV-3948) was an instance of the problem fixed by Sergey's patch
    in 10.0 - namely that the range optimizer could change <code class="fixed" style="white-space:pre-wrap">table-&gt;[read | write]_set</code>,
    and not restore it.<pre class="fixed">revno: 3471
committer: Sergey Petrunya &lt;psergey@askmonty.org&gt;
branch nick: 10.0-serg-fix-imerge
timestamp: Sat 2012-11-03 12:24:36 +0400
message:
  - MDEV-3817: Wrong result with index_merge+index_merge_intersection, InnoDB table, join, AND and OR conditions
  Reconcile the fixes from:
  -
  - guilhem.bichot@oracle.com-20110805143029-ywrzuz15uzgontr0
  - Fix for BUG#12698916 - "JOIN QUERY GIVES WRONG RESULT AT 2ND EXEC. OR
  - AFTER FLUSH TABLES [-INT VS NULL]"
  -
  - guilhem.bichot@oracle.com-20111209150650-tzx3ldzxe1yfwji6
  - Fix for BUG#12912171 - ASSERTION FAILED: QUICK-&gt;HEAD-&gt;READ_SET == SAVE_READ_SET
  - and
  -
  and related fixes from: BUG#1006164, MDEV-376:
  .
  Now, ROR-merged QUICK_RANGE_SELECT objects make no assumptions about the values
  of table-&gt;read_set and table-&gt;write_set.
  Each QUICK_ROR_SELECT has (and had before) its own column bitmap, but now, all 
  QUICK_ROR_SELECT's functions that care: reset(), init_ror_merged_scan(), and 
  get_next()  will set table-&gt;read_set when invoked and restore it back to what 
  it was before the call before they return.
  .
  This allows to avoid the mess when somebody else modifies table-&gt;read_set for 
  some reason.
</pre>
</li></ul>
</li><li>[Revision #3334.1.316](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.316) [merge]
   <span class="cstm-style datetime">Mon 2013-01-28 13:36:05 +0100</span>
<ul start="1"><li>5.3 merge
</li><li>[Revision #2502.567.65](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.65) [merge]
    <span class="cstm-style datetime">Mon 2013-01-28 09:12:23 +0100</span>
<ul start="1"><li>5.2 merge
</li><li>[Revision #2502.566.42](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.42)
     <span class="cstm-style datetime">Mon 2013-01-28 09:10:01 +0100</span>
<ul start="1"><li>compilation error with -Wuninitialized -Werror
</li></ul>
</li><li>[Revision #2502.566.41](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.41) [merge]
     <span class="cstm-style datetime">Fri 2013-01-25 17:22:21 +0100</span>
<ul start="1"><li>5.1 merge
</li><li>[Revision #2502.565.29](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.29)
      <span class="cstm-style datetime">Fri 2013-01-25 14:29:46 +0100</span>
<ul start="1"><li>[MDEV-729](https://jira.mariadb.org/browse/MDEV-729) [Bug #998028](https://bugs.launchpad.net/bugs/998028) - Server crashes on normal shutdown in closefrm after executing a query from MyISAM table
</li><li>don't write a key value into the record buffer - a key length can be larger then the record length.
</li></ul>
</li><li>[Revision #2502.565.28](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.28)
      <span class="cstm-style datetime">Fri 2013-01-25 12:26:35 +0100</span>
<ul start="1"><li>[MDEV-759](https://jira.mariadb.org/browse/MDEV-759) [Bug #998340](https://bugs.launchpad.net/bugs/998340) - Valgrind complains on simple selects containing expression DAY(FROM_UNIXTIME(-1))
</li><li>check item-&gt;null_value before using the result of item-&gt;val_int()
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #2502.567.64](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.64)
    <span class="cstm-style datetime">Sat 2013-01-26 22:33:18 +0100</span>
<ul start="1"><li>[MDEV-3875](https://jira.mariadb.org/browse/MDEV-3875) Wrong result (missing row) on a DISTINCT query with the same subquery in the SELECT list and GROUP BY
</li><li>fix remove_dup_with_hash_index() and remove_dup_with_compare() to take NULLs into account
</li></ul>
</li><li>[Revision #2502.567.63](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.63)
    <span class="cstm-style datetime">Fri 2013-01-25 16:56:57 +0200</span>
<ul start="1"><li>The problem was that expression with field after transformation (on the first execution) reached by fix_fields() (via reference) before row which it belongs to (on the second execution) and fix_field for row did not follow usual protocol for Items with argument (first check that the item fixed then call fix_fields).
</li><li>Item_row::fix_field fixed.
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.315](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.315)
   <span class="cstm-style datetime">Mon 2013-01-28 13:49:14 +0200</span>
<ul start="1"><li>[MDEV-4091](https://jira.mariadb.org/browse/MDEV-4091): Dynamic columns C functions should be included in libmysqlclient
</li></ul>
</li><li>[Revision #3334.1.314](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.314) [merge]
   <span class="cstm-style datetime">Sat 2013-01-26 22:23:27 +0100</span>
<ul start="1"><li>Merge [MDEV-3842](https://jira.mariadb.org/browse/MDEV-3842), [MDEV-3923](https://jira.mariadb.org/browse/MDEV-3923), [MDEV-3971](https://jira.mariadb.org/browse/MDEV-3971)
</li><li>[Revision #3334.31.4](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.31.4)
    <span class="cstm-style datetime">Fri 2013-01-25 23:34:46 +0100</span>
<ul start="1"><li>fix embedded
</li></ul>
</li><li>[Revision #3334.31.3](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.31.3)
    <span class="cstm-style datetime">Fri 2013-01-25 18:59:30 +0100</span>
<ul start="1"><li>Fix embedded build
</li></ul>
</li><li>[Revision #3334.31.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.31.2)
    <span class="cstm-style datetime">Fri 2013-01-25 17:26:10 +0100</span>
<ul start="1"><li>[MDEV-3842](https://jira.mariadb.org/browse/MDEV-3842),  [MDEV-3923](https://jira.mariadb.org/browse/MDEV-3923) :
</li><li>Miscellaneous workarounds for  drop-in compatibility problems with Linux distributions, arounf versioning of the  MySQL 5.5 client shared library. There seems to be 3 different ways major distributions handle versioning
<ol start="1"><li>Fedora  (also Mageia, and likely  other Redhat descendants) way     old, 5.1 API functions are given version libmysqlclient_16    new API functions  (client plugins, mysql_stmt_next ) are given version libmysqlclient_18    some extra functions beyond API are exported.    some functions are renamed.
</li><li>Debian Wheezy way    all functions are given libmysqlclient_18 version
</li><li>Ubuntu  way (or MySQL/MariaDB download packages)   no versioning
</li></ol>
</li><li>UIp to this fix, MariaDB distributions did not have any versioning in the libraries, this rendered client library incompatible to distributions  thus exchanging  distribution's libmysqlclient.so.18.0.0  with MariaDB's did not work nicely (anywhere but on Ubuntu)
</li><li>THE FIX   is to build libraries the same way as distributions do it  - when building RPMs, use  same version script as Fedora does, Make sure to export extra-symbols, the same as Fedora exports. - when building DEBs, use the same version script as Debian Wheezy - do not use version scripts otherwise
</li><li>Also, makes sure that extensions of  MySQL APIs (asynchronous client functionality) is exported by  the shared libraries.
</li></ul>
</li><li>[Revision #3334.31.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.31.1)
    <span class="cstm-style datetime">Fri 2013-01-25 16:50:14 +0100</span>
<ul start="1"><li>[MDEV-3971](https://jira.mariadb.org/browse/MDEV-3971)  : problems installing MariaDB packages (conflicts with mysql-libs-5.5) FIx  : make "shared" RPM obsolete/provide mysql-libs
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.313](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.313) [merge]
   <span class="cstm-style datetime">Sat 2013-01-26 01:59:27 +0200</span>
<ul start="1"><li>Automatic merge
</li><li>[Revision #3334.30.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.30.1)
    <span class="cstm-style datetime">Fri 2013-01-25 21:40:42 +0200</span>
<ul start="1"><li>Fixed [MDEV-3890](https://jira.mariadb.org/browse/MDEV-3890): Server crash inserting record on a temporary table after truncating it The problem was that a temporary table was re-created as a non-temporary table.
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.312](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.312) [merge]
   <span class="cstm-style datetime">Fri 2013-01-25 11:24:42 +0100</span>
<ul start="1"><li>5.3 merge
</li><li>[Revision #2502.567.62](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.62) [merge]
    <span class="cstm-style datetime">Fri 2013-01-25 10:20:45 +0100</span>
<ul start="1"><li>5.2 merge
</li><li>[Revision #2502.566.40](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.40)
     <span class="cstm-style datetime">Fri 2013-01-25 10:19:35 +0100</span>
<ul start="1"><li>[MDEV-4040](https://jira.mariadb.org/browse/MDEV-4040) Replace deprecated SET OPTION syntax in mysqldump
</li><li>mysqldump.c: s/SET OPTION/SET/ (OPTION was, hm, optional since 3.21, so there's no need to use SET OPTION even in the old compatibility modes)
</li></ul>
</li><li>[Revision #2502.566.39](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.39)
     <span class="cstm-style datetime">Fri 2013-01-25 09:41:26 +0100</span>
<ul start="1"><li>[MDEV-3909](https://jira.mariadb.org/browse/MDEV-3909) remote user enumeration
</li><li>instead of returning Access denied on the incorrect user name, emulate the complete failed logic procedure, possibly with the change plugin packet.
</li></ul>
</li><li>[Revision #2502.566.38](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.38)
     <span class="cstm-style datetime">Fri 2013-01-25 00:20:53 +0100</span>
<ul start="1"><li>report "using password: YES/NO" correctly for the COM_CHANGE_USER failures
</li></ul>
</li><li>[Revision #2502.566.37](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.37)
     <span class="cstm-style datetime">Fri 2013-01-25 00:17:39 +0100</span>
<ul start="1"><li>[MDEV-3915](https://jira.mariadb.org/browse/MDEV-3915) COM_CHANGE_USER allows fast password brute-forcing
</li><li>allow only three failed change_user per connection. successful change_user do NOT reset the counter
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.311](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.311) [merge]
   <span class="cstm-style datetime">Wed 2013-01-23 15:18:05 -0800</span>
<ul start="1"><li>Merge 5.3-&gt;5.5
</li><li>[Revision #2502.567.61](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.61) [merge]
    <span class="cstm-style datetime">Mon 2013-01-21 21:29:19 -0800</span>
<ul start="1"><li>Merge 5.2-&gt;5.3
</li><li>[Revision #2502.566.36](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.36) [merge]
     <span class="cstm-style datetime">Mon 2013-01-21 15:23:40 -0800</span>
<ul start="1"><li>Merge 5.1-&gt;5.2
</li><li>[Revision #2502.565.27](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.27) [merge]
      <span class="cstm-style datetime">Mon 2013-01-21 13:48:34 -0800</span>
<ul start="1"><li>Merge.
</li><li>[Revision #2502.571.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.571.1)
       <span class="cstm-style datetime">Mon 2013-01-21 11:47:45 -0800</span>
<ul start="1"><li>Fixed bug [MDEV-4063](https://jira.mariadb.org/browse/MDEV-4063) (bug #56927). This bug could result in returning 0 for the expressions of the form  &lt;aggregate_function&gt;(distinct field) when the system variable   max_heap_table_size was set to a small enough number. It happened because the method Unique::walk() did not support the case when more than one pass was needed to merge the trees of distinct values saved in an external file.
</li><li>Backported a fix in grant_lowercase.test from [mariadb 5.5](/kb/en/what-is-mariadb-55/).
</li></ul>
</li></ul>
</li><li>[Revision #2502.565.26](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.26)
      <span class="cstm-style datetime">Mon 2013-01-21 10:52:39 +0100</span>
<ul start="1"><li>[MDEV-4029](https://jira.mariadb.org/browse/MDEV-4029) SELECT on information_schema using a subquery locks up the information_schema table due to incorrect mutexes handling
</li><li>Early evaluation of subqueries in the WHERE conditions on I_S.*_STATUS tables, otherwise the subquery on this same table will try to acquire LOCK_status twice.
</li></ul>
</li></ul>
</li><li>[Revision #2502.566.35](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.35)
     <span class="cstm-style datetime">Sat 2013-01-19 23:40:53 -0800</span>
<ul start="1"><li>Corrected the test case for bug [MDEV-3938](https://jira.mariadb.org/browse/MDEV-3938).
</li></ul>
</li><li>[Revision #2502.566.34](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.34)
     <span class="cstm-style datetime">Wed 2013-01-16 11:17:58 -0800</span>
<ul start="1"><li>Corrected the fix for bug [MDEV-3938](https://jira.mariadb.org/browse/MDEV-3938).
</li></ul>
</li><li>[Revision #2502.566.33](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.33)
     <span class="cstm-style datetime">Tue 2013-01-15 16:46:27 -0800</span>
<ul start="1"><li>Fixed bug [MDEV-3938](https://jira.mariadb.org/browse/MDEV-3938). The original patch with the implementation of virtual columns did not support INSERT DELAYED into tables with virtual columns. This patch fixes the problem.
</li></ul>
</li></ul>
</li><li>[Revision #2502.567.60](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.60)
    <span class="cstm-style datetime">Wed 2013-01-16 21:07:26 +0200</span>
<ul start="1"><li>[MDEV-4056](https://jira.mariadb.org/browse/MDEV-4056) fix.
</li><li>The problem was that maybe_null of Item_row and its componetes was unsynced after update_used_tables() (and so pushed_cond_guards was not initialized but then requested).
</li><li>Fix  updates Item_row::maybe_null on update_used_tables().
</li></ul>
</li><li>[Revision #2502.567.59](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.59)
    <span class="cstm-style datetime">Thu 2013-01-17 16:08:05 +0200</span>
<ul start="1"><li>[MDEV-3900](https://jira.mariadb.org/browse/MDEV-3900) Optimizer difference between MySQL and MariaDB with stored functions in WHERE clause of UPDATE or DELETE statements
</li><li>Analysis The reason for the less efficient plan was result of a prior design decision - to limit the eveluation of constant expressions during optimization to only non-expensive ones. With this approach all stored procedures were considered expensive, and were not evaluated during optimization. As a result, SPs didn't participate in range optimization, which resulted in a plan with table scan rather than index range scan.
</li><li>Solution Instead of considering all SPs expensive, consider expensive only those SPs that are non-deterministic. If an SP is deterministic, the optimizer will checj if it is constant, and may eventually evaluate it during optimization.
</li></ul>
</li><li>[Revision #2502.567.58](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.58)
    <span class="cstm-style datetime">Thu 2013-01-17 13:53:15 +0200</span>
<ul start="1"><li>backport of: Don't reset maybe_null in update_used_tables(); This breaks ROLLUP This fixed failing test in group_by.test
</li></ul>
</li><li>[Revision #2502.567.57](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.57)
    <span class="cstm-style datetime">Wed 2013-01-16 15:11:13 +0200</span>
<ul start="1"><li>[MDEV-3988](https://jira.mariadb.org/browse/MDEV-3988) fix.
</li><li>Subquery turned into constant too late to be excluded from grouping list so test for constant added to the create_temp_table().
</li></ul>
</li><li>[Revision #2502.567.56](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.56)
    <span class="cstm-style datetime">Fri 2013-01-11 20:26:34 -0800</span>
<ul start="1"><li>Fixed bug [MDEV-4025](https://jira.mariadb.org/browse/MDEV-4025). The bug could lead to a wrong estimate of the number of expected rows in the output of the EXPLAIN commands for queries with GROUP BY. This could be observed in the test case for LP bug 934348.
</li></ul>
</li><li>[Revision #2502.567.55](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.55)
    <span class="cstm-style datetime">Fri 2013-01-11 12:44:21 +0100</span>
<ul start="1"><li>[MDEV-4020](https://jira.mariadb.org/browse/MDEV-4020) : Make sure strmov symbol is exported by client library  on Linux (even if the server and libraries itself use stpcpy instead of it)
</li><li>It is a workaround that allows myodbc built by certain distributions' (CentOS,Fedora) to peacefully coexist with mariadb client libraries. The problem is that MyODBC in these distros needs strmov() to be exported by mysql client shared library, or else myodbc fails to load.
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.310](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.310)
   <span class="cstm-style datetime">Wed 2013-01-23 14:58:05 +0100</span>
<ul start="1"><li>remove one particularly stupid test
</li></ul>
</li><li>[Revision #3334.1.309](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.309)
   <span class="cstm-style datetime">Mon 2013-01-21 12:20:54 +0100</span>
<ul start="1"><li>[MDEV-4069](https://jira.mariadb.org/browse/MDEV-4069) thd_wait_end does not called in some cases in buf_page_read_low in XtraDB engine
</li></ul>
</li><li>[Revision #3334.1.308](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.308)
   <span class="cstm-style datetime">Tue 2013-01-22 13:29:59 +0200</span>
<ul start="1"><li>Fixed typo in the function name. test suite added.
</li></ul>
</li><li>[Revision #3334.1.307](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.307)
   <span class="cstm-style datetime">Mon 2013-01-21 14:34:39 +0200</span>
<ul start="1"><li>[MDEV-3873](https://jira.mariadb.org/browse/MDEV-3873): fixed functions absend in 5.3.
</li></ul>
</li><li>[Revision #3334.1.306](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.306)
   <span class="cstm-style datetime">Sun 2013-01-20 21:43:11 +0100</span>
<ul start="1"><li>fix a strict aliasing warning - remove a meaningless cast.
</li></ul>
</li><li>[Revision #3334.1.305](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.305)
   <span class="cstm-style datetime">Sun 2013-01-20 21:42:01 +0100</span>
<ul start="1"><li>[MDEV-3952](https://jira.mariadb.org/browse/MDEV-3952) Incompatible change in MariaDB-5.5.28a-client rpm adds mytop when not in MariaDB-5.5.23-client (CentOS 5)
</li><li>Same as for deb: don't add mytop to the client rpm.
</li></ul>
</li><li>[Revision #3334.1.304](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.304)
   <span class="cstm-style datetime">Sun 2013-01-20 14:06:33 +0100</span>
<ul start="1"><li>[MDEV-3934](https://jira.mariadb.org/browse/MDEV-3934) Assertion `((keypart_map+1) &amp; keypart_map) == 0' failed in _mi_pack_key with an index on a POINT column
</li><li>sel_arg_range_seq_next(): set keypart map also for GEOM_FLAG keys
</li></ul>
</li><li>[Revision #3334.1.303](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.303)
   <span class="cstm-style datetime">Sun 2013-01-20 00:46:51 +0100</span>
<ul start="1"><li>[MDEV-4029](https://jira.mariadb.org/browse/MDEV-4029) SELECT on information_schema using a subquery locks up the information_schema table due to incorrect mutexes handling
</li><li>Early evaluation of subqueries in the WHERE conditions on I_S.*_STATUS tables, otherwise the subquery on this same table will try to acquire LOCK_status twice.
</li></ul>
</li><li>[Revision #3334.1.302](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.302)
   <span class="cstm-style datetime">Sat 2013-01-19 14:03:33 +0100</span>
<ul start="1"><li>[MDEV-3832](https://jira.mariadb.org/browse/MDEV-3832) MariaDB conflicts with packages filesystem-3.1-2.fc18.i686 and jre-1.7.0_09-fcs.i586 on Fedora 18
</li><li>fix the rpm packaging to work on Fedora18. Two problems: * conflicts on common directories with other packages. * more auto-generated requirements for mariadb-test.rpm
</li></ul>
</li><li>[Revision #3334.1.301](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.301)
   <span class="cstm-style datetime">Fri 2013-01-18 19:10:20 +0100</span>
<ul start="1"><li>[MDEV-633](https://jira.mariadb.org/browse/MDEV-633) [Bug #1024058](https://bugs.launchpad.net/bugs/1024058) - mysqld XA crash in replication slave
</li><li>initialize cache_mngr and write the Xid into binlog even if binlog is disabled with <code class="fixed" style="white-space:pre-wrap">SQL_LOG_BIN=0</code> or no <code class="fixed" style="white-space:pre-wrap">--log-slave-updates</code> in the slave thread
</li></ul>
</li><li>[Revision #3334.1.300](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.300)
   <span class="cstm-style datetime">Fri 2013-01-18 19:07:59 +0100</span>
<ul start="1"><li>simplify THD::binlog_setup_trx_data() usage
</li></ul>
</li><li>[Revision #3334.1.299](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.299)
   <span class="cstm-style datetime">Fri 2013-01-18 19:04:51 +0100</span>
<ul start="1"><li>[MDEV-3908](https://jira.mariadb.org/browse/MDEV-3908) crash in multi-table delete and mdl
</li><li>Add a test case. The fix comes with MySQL bug#15948123: SERVER WORKS INCORRECT WITH LONG TABLE ALIASES
</li></ul>
</li><li>[Revision #3334.1.298](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.298)
   <span class="cstm-style datetime">Fri 2013-01-18 19:04:23 +0100</span>
<ul start="1"><li>[MDEV-4065](https://jira.mariadb.org/browse/MDEV-4065) thd_kill_statement service
</li></ul>
</li><li>[Revision #3334.1.297](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.297)
   <span class="cstm-style datetime">Fri 2013-01-18 18:49:07 +0100</span>
<ul start="1"><li>Fix Windows installers' bootstrapper scripts ,  after mysql_performance_tables.sql was  split off mysql_system_tables.sql
</li></ul>
</li><li>[Revision #3334.1.296](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.296)
   <span class="cstm-style datetime">Thu 2013-01-17 02:27:10 +0200</span>
<ul start="1"><li>Don't reset maybe_null in update_used_tables(); This breaks ROLLUP This fixed failing test in group_by.test
</li></ul>
</li><li>[Revision #3334.1.295](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.295)
   <span class="cstm-style datetime">Thu 2013-01-17 01:08:49 +0200</span>
<ul start="1"><li>Fixed compiler warning
</li></ul>
</li><li>[Revision #3334.1.294](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.294) [merge]
   <span class="cstm-style datetime">Wed 2013-01-16 11:13:08 +0100</span>
<ul start="1"><li>xtradb merge. Percona-Server-5.5.28-rel29.3
</li><li>[Revision #0.12.59](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/0.12.59)
    <span class="cstm-style datetime">Tue 2013-01-15 22:22:49 +0100</span>
<ul start="1"><li>Percona-Server-5.5.28-rel29.3
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.293](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.293)
   <span class="cstm-style datetime">Tue 2013-01-15 19:16:29 +0100</span>
<ul start="1"><li>Test case and a different fix for MySQL bug#14485479
</li></ul>
</li><li>[Revision #3334.1.292](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.292)
   <span class="cstm-style datetime">Tue 2013-01-15 19:16:18 +0100</span>
<ul start="1"><li>small cleanups
</li></ul>
</li><li>[Revision #3334.1.291](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.291)
   <span class="cstm-style datetime">Tue 2013-01-15 19:15:51 +0100</span>
<ul start="1"><li>backport a test case for a 5.5 bug fix from the 5.6 tree
</li></ul>
</li><li>[Revision #3334.1.290](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.290) [merge]
   <span class="cstm-style datetime">Tue 2013-01-15 19:13:32 +0100</span>
<ul start="1"><li>mysql-5.5.29 merge
</li><li>[Revision #3077.166.19](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.166.19) [merge]
    <span class="cstm-style datetime">Tue 2012-09-11 17:42:22 +0300</span>
<ul start="1"><li>merge
</li></ul>
</li><li>[Revision #3077.166.18](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.166.18)
    <span class="cstm-style datetime">Mon 2012-08-27 15:30:58 +0300</span>
<ul start="1"><li>Bug #13548161: MYSQLD_SAFE IMPROVEMENTS FOR 5.5 ALLWAYS SETS PLUGIN_DIR  TO DEFAULT IGNOR
</li><li>The test in mysqld_safe for the presence of the <code class="fixed" style="white-space:pre-wrap">--plugin-dir</code> and assigning a default value to it were performed before the actual argument parsing. This is wrong, as PLUGIN_DIR mysqld_safe code also uses MY_BASEDIR_VERSION to  look for version specific plugin directory if present. Fixed by moving the PLUGIN_DIR logic after the parse_arguments() call.
</li></ul>
</li><li>[Revision #3077.166.17](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.166.17)
    <span class="cstm-style datetime">Fri 2012-08-24 15:01:31 +0300</span>
<ul start="1"><li>Bug #14181049: MYSQL_INSTALL_DB.PL CREATES EMPTY SYSTEM TABLES FOR MYSQL 
</li><li>The script is different from what's used on unixes. It was not playing the table insertion script (mysql_system_tables_data.sql), although it was checking for the presence of this script. Fixed by re-enabling the lookup for this file and replaying it at bootstrap time. Note that on the Unixes "SELECT @@hostname" does return a fully qualified name, whereas on Windows it returns only a hostname. So by default we're filtering records in the mysql.user table until we ensure this is fixed.
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.289](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.289)
   <span class="cstm-style datetime">Tue 2013-01-15 19:08:49 +0100</span>
<ul start="1"><li>update debian patch to apply
</li></ul>
</li><li>[Revision #3334.1.288](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.288) [merge]
   <span class="cstm-style datetime">Tue 2013-01-15 19:07:46 +0100</span>
<ul start="1"><li>5.3 merge
</li><li>[Revision #2502.567.54](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.54) [merge]
    <span class="cstm-style datetime">Thu 2013-01-10 15:40:21 +0100</span>
<ul start="1"><li>5.2-&gt;5.3 merge
</li><li>[Revision #2502.566.32](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.32) [merge]
     <span class="cstm-style datetime">Thu 2013-01-10 13:54:04 +0100</span>
<ul start="1"><li>5.1 merge
</li><li>[Revision #2502.565.25](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.25) [merge]
      <span class="cstm-style datetime">Wed 2013-01-09 23:51:51 +0100</span>
<ul start="1"><li>mysql-5.1.67 merge
</li></ul>
</li><li>[Revision #2502.565.24](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.24)
      <span class="cstm-style datetime">Tue 2012-12-04 17:08:02 +0100</span>
<ul start="1"><li>proactive s/strmov/strnmov/ in sql_acl.cc and related test cases
</li></ul>
</li></ul>
</li><li>[Revision #2502.566.31](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.31) [merge]
     <span class="cstm-style datetime">Fri 2012-12-21 15:19:08 +0100</span>
<ul start="1"><li>merge
</li><li>[Revision #2502.565.23](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.23)
      <span class="cstm-style datetime">Fri 2012-12-21 15:17:26 +0100</span>
<ul start="1"><li>Support VS2012. Exclude compiler-defined symbols from being exported by mysqld.exe
</li></ul>
</li></ul>
</li><li>[Revision #2502.566.30](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.30) [merge]
     <span class="cstm-style datetime">Fri 2012-12-21 14:04:25 +0100</span>
<ul start="1"><li>merge
</li><li>[Revision #2502.565.22](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.22)
      <span class="cstm-style datetime">Thu 2012-12-06 00:37:06 +0100</span>
<ul start="1"><li>[MDEV-3918](https://jira.mariadb.org/browse/MDEV-3918): myisamchk  bogus error for files larger than 4GB.
</li><li>The failure is caused by failing stat() call . C Runtime function stat() uses old struct with 32bit st_size member, and since Visual Studio 2010 , it returns an error on st_size overflow (i.e on files larger than 4GB)
</li><li>Fix replaces stat() by my_stat(), the later is backed by 64bit-able stat64().
</li></ul>
</li></ul>
</li><li>[Revision #2502.566.29](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.29)
     <span class="cstm-style datetime">Tue 2012-12-11 09:50:48 +0100</span>
<ul start="1"><li>one-byte overflow with old passwords
</li></ul>
</li><li>[Revision #2502.566.28](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.28)
     <span class="cstm-style datetime">Mon 2012-11-26 13:33:24 +0100</span>
<ul start="1"><li>Fix broken feedback plugin after [MDEV-712](https://jira.mariadb.org/browse/MDEV-712).
</li><li>Link feedback plugin with yassl libraries, if with-ssl=bundled is used, since mysqld does not export SSL symbols anymore.
</li></ul>
</li><li>[Revision #2502.566.27](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.27)
     <span class="cstm-style datetime">Fri 2012-11-23 13:50:46 +0100</span>
<ul start="1"><li>[MDEV-712](https://jira.mariadb.org/browse/MDEV-712)  -  [Bug #1024239](https://bugs.launchpad.net/bugs/1024239) - Mysqlclient exports the same symbols as openssl
</li><li>Compile yassl and taocrypt using -fvisibility=hidden, when possible.  This prevent symbols from being exported.
</li></ul>
</li><li>[Revision #2502.566.26](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.26) [merge]
     <span class="cstm-style datetime">Thu 2012-11-22 18:29:53 +0100</span>
<ul start="1"><li>merge 5.1
</li><li>[Revision #2502.565.21](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.21)
      <span class="cstm-style datetime">Thu 2012-11-22 18:27:02 +0100</span>
<ul start="1"><li>Feedback plugin now recognizes  Windows 8 / Windows Server 2012.
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #2502.567.53](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.53)
    <span class="cstm-style datetime">Tue 2013-01-08 21:23:03 +0100</span>
<ul start="1"><li>[MDEV-3942](https://jira.mariadb.org/browse/MDEV-3942) FROM_DAYS(&lt;timestamp column&gt;) returns different result in MariaDB comparing to MySQL: NULL vs 0000-00-00
</li><li>fixed a regression, introduced while fixing [MDEV-456](https://jira.mariadb.org/browse/MDEV-456)
</li></ul>
</li><li>[Revision #2502.567.52](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.52)
    <span class="cstm-style datetime">Fri 2012-12-28 14:41:46 +0200</span>
<ul start="1"><li>[MDEV-3873](https://jira.mariadb.org/browse/MDEV-3873) &amp; [MDEV-3876](https://jira.mariadb.org/browse/MDEV-3876) &amp; [MDEV-3912](https://jira.mariadb.org/browse/MDEV-3912) : Wrong result (extra rows) with ALL subquery from a MERGE view.
</li><li>The problem was in the lost ability to be null for the table of a left join if it is a view/derived table.
</li><li>It hapenned because setup_table_map(), was called earlier then we merged the view or derived.
</li><li>Fixed by propagating new maybe_null flag during Item::update_used_tables().
</li><li>Change in join_outer.test and join_outer_jcl6.test appeared because IS NULL reported no used tables (i.e. constant) for argument which could not be NULL and new maybe_null flag was propagated for IS NULL argument (Item_field) because table the Item_field belonged to changed its maybe_null status.
</li></ul>
</li><li>[Revision #2502.567.51](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.51)
    <span class="cstm-style datetime">Wed 2012-12-19 15:56:57 +0200</span>
<ul start="1"><li>[MDEV-3928](https://jira.mariadb.org/browse/MDEV-3928): Assertion `example' failed in Item_cache::is_expensive_processor with a 2-level IN subquery
</li><li>Analysis: The following call stack shows that it is possible to set Item_cache::value_cached, and the relevant value without setting Item_cache::example.
</li><li>#0 Item_cache_temporal::store_packed at item.cc:8395 #1 get_datetime_value at item_cmpfunc.cc:915 #2 resolve_const_item at item.cc:7987 #3 propagate_cond_constants at sql_select.cc:12264 #4 propagate_cond_constants at sql_select.cc:12227 #5 optimize_cond at sql_select.cc:13026 #6 JOIN::optimize at sql_select.cc:1016 #7 st_select_lex::optimize_unflattened_subqueries at sql_lex.cc:3161 #8 JOIN::optimize_unflattened_subqueries at opt_subselect.cc:4880 #9 JOIN::optimize at sql_select.cc:1554
</li><li>The fix is to set Item_cache_temporal::example even when the value is set directly by Item_cache_temporal::store_packed. This makes the Item_cache_temporal object consistent.
</li></ul>
</li><li>[Revision #2502.567.50](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.50)
    <span class="cstm-style datetime">Wed 2012-12-05 21:06:00 +0200</span>
<ul start="1"><li>[MDEV-3914](https://jira.mariadb.org/browse/MDEV-3914) fix.
</li><li>Fixed algorithm of detecting of first real table in view/subquery-in-the-FROM-clase.
</li></ul>
</li><li>[Revision #2502.567.49](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.49)
    <span class="cstm-style datetime">Fri 2012-11-23 13:11:31 +0100</span>
<ul start="1"><li>bump the version to 5.3.11
</li></ul>
</li><li>[Revision #2502.567.48](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.48) [merge]
    <span class="cstm-style datetime">Thu 2012-11-22 10:30:39 -0800</span>
<ul start="1"><li>Merge
</li><li>[Revision #2502.570.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.570.1)
     <span class="cstm-style datetime">Wed 2012-11-21 21:55:04 -0800</span>
<ul start="1"><li>Fixed LP bug #1002146 (bug [MDEV-645](https://jira.mariadb.org/browse/MDEV-645)). If the setting of system variables does not allow to use join buffer for a join query with GROUP BY &lt;f1,...&gt; / ORDER BY &lt;f1,...&gt; then filesort is not needed if the first joined table is scanned in the order compatible with order specified by the list &lt;f1,...&gt;.
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.287](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.287)
   <span class="cstm-style datetime">Tue 2013-01-15 17:46:46 +0100</span>
<ul start="1"><li>remove thd_mark_as_hard_kill() (because it's conceptually wrong. only the user can decide whether the kill is allowed to leave tables in the inconsistent state, storage engine has no say in that)
</li></ul>
</li><li>[Revision #3334.1.286](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.286)
   <span class="cstm-style datetime">Tue 2013-01-15 14:33:08 +0200</span>
<ul start="1"><li>Fix for bug [MDEV-3992](https://jira.mariadb.org/browse/MDEV-3992), second attempt
</li><li>The previous fix for [MDEV-3992](https://jira.mariadb.org/browse/MDEV-3992) was incomplete, because it still computed incorrectly the number of keyparts of the extended secondary key in the case when columns of the PK participate in the secondary key.
</li><li>This patch by Monty corrects the above problem.
</li></ul>
</li><li>[Revision #3334.1.285](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.285)
   <span class="cstm-style datetime">Mon 2013-01-14 15:05:05 +0200</span>
<ul start="1"><li>Fix for bug [MDEV-3992](https://jira.mariadb.org/browse/MDEV-3992)
</li><li>Analysis:   The crash is a result of incorrect analysis of whether a secondary key   can be extended with a primary in order to compute ORDER BY. The analysis   is done in test_if_order_by_key(). This function doesn't take into account   that the primary key may in fact index the same columns as the secondary   key. For the test query test_if_order_by_key says that there is an extended   key with total 2 keyparts.   At the same time, the condition     if (pkinfo-&gt;key_part[i].field-&gt;key_start.is_set(nr))   in test_if_cheaper_oredring() becomes true for (i == 0), which results in   an invalid access to rec_per_key[-1].
</li><li>Solution:   The best solution would be to reuse KEY::ext_key_parts that is already computed   by open_binary_frm(), however after detailed analysis the conclusion is that   the change would be too intrusive for a GA release.   The solution for 5.5 is to add a guard for the case when the 0-th key part is   considered, and to assume that all keys will be scanned in this case.
</li></ul>
</li><li>[Revision #3334.1.284](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.284)
   <span class="cstm-style datetime">Fri 2013-01-11 02:03:43 +0200</span>
<ul start="1"><li>Buildbot fixes and cleanups: - Added <code class="fixed" style="white-space:pre-wrap">--verbose</code> to BUILD scripts to get make to write out compile commands. - Detect if AM_EXTRA_MAKEFLAGS=VERBOSE=1 was used with build scripts. - Don't write warnings about replication variables when doing bootstrap. - Fixed that mysql_cond_wait() and mysql_cond_timedwait() will report original source file in case of errors. - Ignore some compiler warnings
</li></ul>
</li><li>[Revision #3334.1.283](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.283)
   <span class="cstm-style datetime">Fri 2013-01-11 01:31:50 +0200</span>
<ul start="1"><li>Fixed crashing bug in GROUP_CONCAT with ROLLUP Fixed [MDEV-4002](https://jira.mariadb.org/browse/MDEV-4002): Server crash or valgrind errors in Item_func_group_concat::setup and Item_func_group_concat::add
</li></ul>
</li><li>[Revision #3334.1.282](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.282)
   <span class="cstm-style datetime">Fri 2013-01-11 00:53:07 +0200</span>
<ul start="1"><li>Fixed problem with failing mysql_upgrade when proc table was not correct. Moved out creation of performance schema tables from mysql_system_tables.sql as the performance_tables creation scripts needs a working mysql.proc to work.
</li></ul>
</li><li>[Revision #3334.1.281](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.281)
   <span class="cstm-style datetime">Fri 2013-01-11 00:35:33 +0200</span>
<ul start="1"><li>Fixed [MDEV-4013](https://jira.mariadb.org/browse/MDEV-4013): Password length in replication setup Give error for wrong parameters to CHANGE MASTER Extend MASTER_PASSWORD and MASTER_HOST lengths
</li></ul>
</li><li>[Revision #3334.1.280](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.280)
   <span class="cstm-style datetime">Fri 2013-01-11 00:22:14 +0200</span>
<ul start="1"><li>Fixed some race conditons and bugs related to killed queries KILL now breaks locks inside InnoDB Fixed possible deadlock when running INNODB STATUS Added ha_kill_query() and kill_query() to send kill signal to all storage engines Added reset_killed() to ensure we don't reset killed state while awake() is getting called
</li></ul>
</li><li>[Revision #3334.1.279](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.279)
   <span class="cstm-style datetime">Thu 2013-01-10 23:40:18 +0200</span>
<ul start="1"><li>Fix for [MDEV-4009](https://jira.mariadb.org/browse/MDEV-4009): main.delayed sporadically fails with "query 'REPLACE DELAYED t1 VALUES (5)' failed: 1317: Query execution was interrupted" - Fixed broadcast without a proper mutex - Don't break existing locks if we are just testing if we can get the lock
</li></ul>
</li><li>[Revision #3334.1.278](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.278)
   <span class="cstm-style datetime">Wed 2013-01-09 17:30:20 +0100</span>
<ul start="1"><li>[MDEV-3846](https://jira.mariadb.org/browse/MDEV-3846) REFRESH_CHECKPOINT and REFRESH_TABLE_STATS tokens share the same value
</li></ul>
</li><li>[Revision #3334.1.277](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.277)
   <span class="cstm-style datetime">Wed 2013-01-09 17:29:51 +0100</span>
<ul start="1"><li>[MDEV-3985](https://jira.mariadb.org/browse/MDEV-3985) crash: uninstall soname 'a'
</li></ul>
</li><li>[Revision #3334.1.276](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.276)
   <span class="cstm-style datetime">Tue 2013-01-08 21:23:40 +0100</span>
<ul start="1"><li>[MDEV-3883](https://jira.mariadb.org/browse/MDEV-3883) Show global status not in order
</li></ul>
</li><li>[Revision #3334.1.275](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.275)
   <span class="cstm-style datetime">Tue 2013-01-08 21:21:28 +0100</span>
<ul start="1"><li>[MDEV-3987](https://jira.mariadb.org/browse/MDEV-3987) uninitialized read in Item_cond::fix_fields leads to crash: select .. where .. in ( select ... )
</li><li>change Item_func_group_concat to use max_length according to the expected semantics
</li></ul>
</li><li>[Revision #3334.1.274](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.274)
   <span class="cstm-style datetime">Mon 2013-01-07 20:21:05 +0100</span>
<ul start="1"><li>non-functional cleanup, clarifying CONVERT_IF_BIGGER_TO_BLOB
</li></ul>
</li><li>[Revision #3334.1.273](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.273)
   <span class="cstm-style datetime">Sat 2013-01-05 23:52:25 +0100</span>
<ul start="1"><li>Remove timed mutexes in XtraDB - obsolete feature that does not link on Windows, if plugin is build dynamically It was already  removed from innobase in the past.
</li></ul>
</li><li>[Revision #3334.1.272](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.272)
   <span class="cstm-style datetime">Tue 2013-01-01 15:33:25 +0100</span>
<ul start="1"><li>[MDEV-3993](https://jira.mariadb.org/browse/MDEV-3993)  - add MSI installer option to set character-set-server=utf8
</li></ul>
</li><li>[Revision #3334.1.271](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.271) [merge]
   <span class="cstm-style datetime">Mon 2012-12-31 01:39:26 +0400</span>
<ul start="1"><li>[MDEV-3990](https://jira.mariadb.org/browse/MDEV-3990): engine tests went out of sync with current MariaDB code  Reasons:   - as of 5.5.27, YEAR(2) is deprecated, hence the new warning;   - [MDEV-553](https://jira.mariadb.org/browse/MDEV-553) - different error code/message on out-of-range autoincrement;   - INSERT IGNORE now produces a warning if a duplicate was encountered
</li><li>[Revision #3334.29.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.29.1)
    <span class="cstm-style datetime">Fri 2012-12-28 17:02:33 +0400</span>
<ul start="1"><li>storage_engine tests and upstream engines/* suites went out of sync with current MariaDB code. Reasons:   - as of 5.5.27, YEAR(2) is deprecated, hence the new warning;   - [MDEV-553](https://jira.mariadb.org/browse/MDEV-553) - different error code/message on out-of-range autoincrement;   - INSERT IGNORE now produces a warning if a duplicate was encountered (change pushed along with [MDEV-553](https://jira.mariadb.org/browse/MDEV-553))
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.270](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.270)
   <span class="cstm-style datetime">Thu 2012-12-06 17:30:22 +0100</span>
<ul start="1"><li>typo
</li></ul>
</li><li>[Revision #3334.1.269](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.269)
   <span class="cstm-style datetime">Thu 2012-12-06 16:34:02 +0100</span>
<ul start="1"><li>if the debian package name for 5.5.28 is 5.5.28-mariadb1wheezy then for 5.5.28a it should be 5.5.28a-mariadb1wheezy not 5.5.28-mariadb-a1wheezy
</li></ul>
</li><li>[Revision #3334.1.268](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.268)
   <span class="cstm-style datetime">Fri 2012-12-21 11:18:29 +0200</span>
<ul start="1"><li>[MDEV-3902](https://jira.mariadb.org/browse/MDEV-3902)  Assertion `record_length == m_record_length' failed at Filesort_buffer::alloc_sort_buffer
</li><li>This bug is a duplicate of [MDEV-3899](https://jira.mariadb.org/browse/MDEV-3899) so adding a test case only.
</li></ul>
</li><li>[Revision #3334.1.267](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.267)
   <span class="cstm-style datetime">Fri 2012-12-21 00:12:37 +0100</span>
<ul start="1"><li>[MDEV-3945](https://jira.mariadb.org/browse/MDEV-3945) - do not hold LOCK_thread_count when freeing THD.
</li><li>The patch decreases the duration of LOCK_thread_count, so it is not hold during THD destructor and freeing memory. This mutex  now only protects the integrity of threads list, when removing THD from it,  and thread_count variable.
</li><li>The add_to_status() function that updates global status during client disconnect,  is now correctly protected by the LOCK_status mutex.
</li><li>Benchmark : in a  "non-persistent" sysbench test (oltp_ro with reconnect after each query),  ~ 25% more connects/disconnects were measured
</li></ul>
</li><li>[Revision #3334.1.266](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.266)
   <span class="cstm-style datetime">Thu 2012-12-20 22:38:40 +0200</span>
<ul start="1"><li>[MDEV-3899](https://jira.mariadb.org/browse/MDEV-3899)  Valgrind warnings (blocks are definitely lost) in filesort on IN subquery with SUM and DISTINCT
</li><li>Analysys: In the beginning of JOIN::cleanup there is code that is supposed to free all filesort buffers. The code assumes that the table being sorted is the first non-constant table. To get this table it calls: first_top_level_tab(this, WITHOUT_CONST_TABLES)
</li><li>However, first_top_level_tab() instead returned the wrong table - the first one in the plan, instead of the first non-constant table. There is no other place outside filesort() where sort buffers may be freed. As a result, the sort buffer was not freed, and there was a memory leak.
</li><li>Solution: Change first_top_level_tab(), to test for WITH_CONST_TABLES instead of WITHOUT_CONST_TABLES.
</li></ul>
</li><li>[Revision #3334.1.265](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.265)
   <span class="cstm-style datetime">Wed 2012-12-19 21:58:05 +0200</span>
<ul start="1"><li>Fixed some compiler warnings
</li></ul>
</li><li>[Revision #3334.1.264](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.264)
   <span class="cstm-style datetime">Tue 2012-12-18 12:44:15 +0200</span>
<ul start="1"><li>[MDEV-3818](https://jira.mariadb.org/browse/MDEV-3818): Query against view over IS tables worse than equivalent query without view
</li><li>Fixed the test to be lower-case because it fails on windows with mixed case.
</li></ul>
</li><li>[Revision #3334.1.263](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.263)
   <span class="cstm-style datetime">Mon 2012-12-17 22:34:56 +0200</span>
<ul start="1"><li>Fixed the CREATE TABLE IF EXIST generates warnings instead of errors
</li></ul>
</li><li>[Revision #3334.1.262](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.262)
   <span class="cstm-style datetime">Mon 2012-12-17 15:23:58 +0200</span>
<ul start="1"><li>[MDEV-3818](https://jira.mariadb.org/browse/MDEV-3818): Query against view over IS tables worse than equivalent query without view
</li><li>Analysis: The reason for the suboptimal plan when querying IS tables through a view was that the view columns that participate in an equality are wrapped by an Item_direct_view_ref and were not recognized as being direct column references.
</li><li>Solution: Use the original Item_field objects via the real_item() method.
</li></ul>
</li><li>[Revision #3334.1.261](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.261)
   <span class="cstm-style datetime">Sun 2012-12-16 20:51:48 +0200</span>
<ul start="1"><li>Remember original table row pack type for ALTER TABLE if table is not copied.
</li></ul>
</li><li>[Revision #3334.1.260](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.260)
   <span class="cstm-style datetime">Sun 2012-12-16 20:49:57 +0200</span>
<ul start="1"><li>Removed lock wait timeout warning when using CREATE TABLE IF EXISTS
</li></ul>
</li><li>[Revision #3334.1.259](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.259)
   <span class="cstm-style datetime">Sun 2012-12-16 16:13:17 +0200</span>
<ul start="1"><li>Implemented [MDEV-3941](https://jira.mariadb.org/browse/MDEV-3941): CREATE TABLE xxx IF NOT EXISTS should not block if table exists. - Added option to check_if_table_exists() to quickly check if table exists (either SHARE or .FRM) - Extended lock_table_names() to not wait for meta data locks if CREATE IF NOT EXISTS is used.
</li></ul>
</li><li>[Revision #3334.1.258](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.258) [merge]
   <span class="cstm-style datetime">Sun 2012-12-16 12:04:26 +0200</span>
<ul start="1"><li>Automatic merge
</li><li>[Revision #3334.28.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.28.1)
    <span class="cstm-style datetime">Fri 2012-12-14 20:21:50 +0200</span>
<ul start="1"><li>Removed extra '+' from some lines (remains of old merge)
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.257](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.257)
   <span class="cstm-style datetime">Mon 2012-11-26 21:22:44 +0200</span>
<ul start="1"><li>Fix of [MDEV-3874](https://jira.mariadb.org/browse/MDEV-3874): Server crashes in Item_field::print on a SELECT from a MERGE view with materialization+semijoin, subquery, ORDER BY.
</li><li>The problem was that in debugging binaries it try to print item to assign human readable name to the item. But subquery item was already freed (join_free/cleanup with full cleanup) so Item_field refers to temporary table which memory had been already freed.
</li></ul>
</li><li>[Revision #3334.1.256](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.256)
   <span class="cstm-style datetime">Tue 2012-12-04 16:06:07 -0800</span>
<ul start="1"><li>Fixed bug [MDEV-3888](https://jira.mariadb.org/browse/MDEV-3888). When inserting a record with update on duplicate keys the server calls the ha_index_read_idx_map handler function to look for the record that violates unique key constraints. The third parameter of this call should mark only the base components of the index where the server is searched for the record. Possible hidden components of the primary key are to be unmarked.
</li></ul>
</li><li>[Revision #3334.1.255](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.255)
   <span class="cstm-style datetime">Sat 2012-12-01 18:01:59 +0100</span>
<ul start="1"><li>fix openssl_1 test
</li></ul>
</li><li>[Revision #3334.1.254](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.254)
   <span class="cstm-style datetime">Sat 2012-12-01 16:33:22 +0100</span>
<ul start="1"><li>[MDEV-3901](https://jira.mariadb.org/browse/MDEV-3901): Wrong SSL error messages
</li><li>Fixed typo (missing comma)
</li></ul>
</li></ul>
- [Revision #3372](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3372)
  <span class="cstm-style datetime">Tue 2013-02-05 19:20:47 +0200</span>
<ul start="1"><li>fixes for the merge with codership-mysql, revision 3839
</li></ul>
- [Revision #3371](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3371)
  <span class="cstm-style datetime">Tue 2013-02-05 17:54:42 +0200</span>
<ul start="1"><li>merged with codership-mysql up to revision 3839 bzr merge -r3810..3839 lp:codership-mysql/5.5
</li></ul>
- [Revision #3370](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3370)
  <span class="cstm-style datetime">Tue 2013-02-05 16:54:50 +0200</span>
<ul start="1"><li>remerging wsrep files from lp:codership-mysql
</li></ul>
- [Revision #3369](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3369)
  <span class="cstm-style datetime">Tue 2013-02-05 16:49:51 +0200</span>
<ul start="1"><li>remerging wsrep files from lp:codership-mysql
</li></ul>
- [Revision #3368](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3368)
  <span class="cstm-style datetime">Tue 2013-02-05 15:48:54 +0200</span>
<ul start="1"><li>re-merging wsrep files from lp:codership-mysql
</li></ul>
- [Revision #3367](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3367)
  <span class="cstm-style datetime">Mon 2013-02-04 00:30:15 +0400</span>
<ul start="1"><li>[MDEV-508](https://jira.mariadb.org/browse/MDEV-508) - wsrep revno can be either a number or XXXX, test should            be able to handle both
</li></ul>
- [Revision #3366](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3366)
  <span class="cstm-style datetime">Sun 2013-02-03 04:03:08 +0400</span>
<ul start="1"><li>[MDEV-508](https://jira.mariadb.org/browse/MDEV-508) (Wrong MTR result files in MariaDB-Galera)
</li></ul>