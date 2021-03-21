# MariaDB Galera 5.5.25 Changelog

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.25) | 
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-5525-release-notes) | 
<strong>Changelog</strong> |
[Overview of Galera](/replication/galera-cluster/what-is-mariadb-galera-cluster)

<strong>Release date:</strong> 4 Sep 2012

For the highlights of this release, see the
[release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-5525-release-notes).

The revision number links will take you to the revision's page on Launchpad. On
Launchpad you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #3344](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3344)
  <span class="cstm-style datetime">Thu 2012-08-30 12:22:37 +0300</span>
<ul start="1"><li>Merged in change sets 3772-3779 from lp:codership-mysql/5.5
</li></ul>
- [Revision #3343](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3343) [merge]
  <span class="cstm-style datetime">Thu 2012-08-09 01:47:21 +0300</span>
<ul start="1"><li>References [Bug #1034621](https://bugs.launchpad.net/bugs/1034621) - Merge up to mysql-5.5.25 level
</li><li>merged codership-mysql/5.5 revisions: bzr diff -r3759..3767
</li><li>merged codership-mysql/5.5 revisions: bzr diff -r3768..3771
</li><li>[Revision #3334.1.150](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.150)
   <span class="cstm-style datetime">Mon 2012-08-06 16:33:11 +0300</span>
<ul start="1"><li>Fixed compiler warnings
</li></ul>
</li><li>[Revision #3334.1.149](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.149)
   <span class="cstm-style datetime">Wed 2012-08-01 19:57:36 +0200</span>
<ul start="1"><li>[MDEV-399](https://jira.mariadb.org/browse/MDEV-399) Combinations defined in the base suite cannot be skipped by overlay
</li><li>When appliying parent combinations to the overlay,
    filter them through the %skip_combinations using the overlayed filename
</li></ul>
</li><li>[Revision #3334.1.148](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.148)
   <span class="cstm-style datetime">Thu 2012-08-02 23:17:27 +0200</span>
<ul start="1"><li>fix oqgraph on MSVC
</li></ul>
</li><li>[Revision #3334.1.147](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.147)
   <span class="cstm-style datetime">Thu 2012-08-02 04:48:33 +0400</span>
<ul start="1"><li>[MDEV-369](https://jira.mariadb.org/browse/MDEV-369): Mismatches in MySQL engines test suite
</li><li>Post-merge fixes for mismatches that only affect 5.5 (but not 5.3)
</li></ul>
</li><li>[Revision #3334.1.146](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.146) [merge]
   <span class="cstm-style datetime">Thu 2012-08-02 04:22:43 +0400</span>
<ul start="1"><li>Merge 5.3-&gt;5.5
</li><li>[Revision #2502.561.14](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.561.14) [merge]
    <span class="cstm-style datetime">Thu 2012-08-02 00:58:13 +0400</span>
<ul start="1"><li>[MDEV-369](https://jira.mariadb.org/browse/MDEV-369) (Mismatches in MySQL engines test suite)
</li><li>Following reasons caused mismatches:
<ul start="1"><li>different handling of invalid values;
</li><li>different CAST results with fractional seconds;
</li><li>microseconds support in MariaDB;
</li><li>different algorithm of comparing temporal values;
</li><li>differences in error and warning texts and codes;
</li><li>different approach to truncating datetime values to time;
</li><li>additional collations;
</li><li>different record order for queries without ORDER BY;
</li><li>[MySQL Bug #66034](http://bugs.mysql.com/bug.php?id=66034).
</li></ul>
</li><li>More details in [MDEV-369](https://jira.mariadb.org/browse/MDEV-369) comments.
</li><li>[Revision #2502.564.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.564.2)
     <span class="cstm-style datetime">Mon 2012-07-30 04:16:49 +0400</span>
<ul start="1"><li>[MDEV-369](https://jira.mariadb.org/browse/MDEV-369) (Mismatches in MySQL engines test suite)
</li></ul>
</li><li>[Revision #2502.564.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.564.1)
     <span class="cstm-style datetime">Thu 2012-07-26 23:31:08 +0400</span>
<ul start="1"><li>Result files were wrong due to MySQL bug#66034 
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.145](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.145)
   <span class="cstm-style datetime">Tue 2012-07-31 22:39:33 +0200</span>
<ul start="1"><li>[MDEV-336](https://jira.mariadb.org/browse/MDEV-336) oqgraph 5.5 crashes in buildbot
</li><li>make CMakeLists.txt to detect if the installed boost can be compiled with the
    installed compile and specified set of compiler options.
</li><li>Background: even sufficiently new Boost cannot be compiled with the sufficiently old gcc
    in the presence of -fno-rtti
</li></ul>
</li><li>[Revision #3334.1.144](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.144)
   <span class="cstm-style datetime">Tue 2012-07-31 19:29:07 +0200</span>
<ul start="1"><li>[MDEV-419](https://jira.mariadb.org/browse/MDEV-419) ensure that all HAVE_XXX constants can be set by cmake
</li><li>add missing checks to configure.cmake
</li><li>remove dead code and unused HAVE_xxx constants from the sources
</li></ul>
</li><li>[Revision #3334.1.143](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.143)
   <span class="cstm-style datetime">Tue 2012-07-31 18:32:46 +0200</span>
<ul start="1"><li>[MDEV-375](https://jira.mariadb.org/browse/MDEV-375) Server crashes in THD::print_aborted_warning with log_warnings &gt; 3
</li><li>Don't use ER(xxx) in THD::close_connection(), when current_thd is already reset to NULL.
</li><li>Prefer ER_THD() or ER_DEFAULT() instead.
</li></ul>
</li><li>[Revision #3334.1.142](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.142)
   <span class="cstm-style datetime">Tue 2012-07-31 16:21:53 +0500</span>
<ul start="1"><li>[MDEV-340](https://jira.mariadb.org/browse/MDEV-340) Save replication comments for DROP TABLE.
</li><li>mysql_rm_table_no_locks() function was modified.
</li><li>When we construct log record for the DROP TABLE, now we
    look if there's a comment before the first table name and
    add it to the record if so.
</li><li>per-file comments:
<ul start="1"><li>sql/sql_table.cc
<ul start="1"><li>[MDEV-340](https://jira.mariadb.org/browse/MDEV-340) Save replication comments for DROP TABLE.
<ul start="1"><li>comment_length() function implemented to find comments in the query,
       call it in mysql_rm_table_no_locks() and use the result to form log record.
</li></ul>
</li></ul>
</li><li>mysql-test/suite/binlog/r/binlog_drop_if_exists.result
<ul start="1"><li>[MDEV-340](https://jira.mariadb.org/browse/MDEV-340) Save replication comments for DROP TABLE.
<ul start="1"><li>test result updated.
</li></ul>
</li></ul>
</li><li>mysql-test/suite/binlog/t/binlog_drop_if_exists.test
<ul start="1"><li>[MDEV-340](https://jira.mariadb.org/browse/MDEV-340) Save replication comments for DROP TABLE.
<ul start="1"><li>test case added.
</li></ul>
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.141](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.141)
   <span class="cstm-style datetime">Tue 2012-07-31 11:31:26 +0200</span>
<ul start="1"><li>[MDEV-418](https://jira.mariadb.org/browse/MDEV-418) Feedback plugin statisics problem
</li><li>Add the check for sys/utsname.h to configure.cmake
</li></ul>
</li><li>[Revision #3334.1.140](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.140)
   <span class="cstm-style datetime">Mon 2012-07-30 20:13:23 +0200</span>
<ul start="1"><li>[MDEV-417](https://jira.mariadb.org/browse/MDEV-417) - fix typo that prevented use of atomic instructions on Windows
</li><li>use correct macro for Microsoft compiler. It is _MSC_VER , not _MSV_VER
</li></ul>
</li><li>[Revision #3334.1.139](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.139)
   <span class="cstm-style datetime">Wed 2012-07-25 20:41:48 +0400</span>
<ul start="1"><li>[MDEV-410](https://jira.mariadb.org/browse/MDEV-410): EXPLAIN shows type=range, while SHOW EXPLAIN and userstat show full table scan is used
</li><li>Make Item_subselect::fix_fields() ignore UNCACHEABLE_EXPLAIN flag when deciding whether 
    the subquery item should be marked as constant.
</li></ul>
</li><li>[Revision #3334.1.138](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.138)
   <span class="cstm-style datetime">Tue 2012-07-24 17:50:06 +0300</span>
<ul start="1"><li>Awoiding registering partiton engine underlying tables whan it has no sens.
</li></ul>
</li><li>[Revision #3334.1.137](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.137)
   <span class="cstm-style datetime">Mon 2012-07-23 23:54:57 +0200</span>
<ul start="1"><li>[MDEV-409](https://jira.mariadb.org/browse/MDEV-409) : /etc/my.cnf config file overwritten during RPM installation 
</li><li>Fix : use attribute %config(noreplace)  for /etc/my.cnf , instead of (automatically generated) %config
</li></ul>
</li><li>[Revision #3334.1.136](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.136) [merge]
   <span class="cstm-style datetime">Thu 2012-07-19 13:24:24 +0200</span>
<ul start="1"><li>merged in [MDEV-11](https://jira.mariadb.org/browse/MDEV-11) "Generic storage engine test suite"
</li><li>[Revision #3334.22.4](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.22.4) [merge]
    <span class="cstm-style datetime">Thu 2012-07-19 13:21:53 +0200</span>
<ul start="1"><li>merged with maria/5.5
</li></ul>
</li><li>[Revision #3334.22.3](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.22.3)
    <span class="cstm-style datetime">Mon 2012-07-16 06:17:56 +0400</span>
<ul start="1"><li>[MDEV-11](https://jira.mariadb.org/browse/MDEV-11): Generic storage engine test suite
</li></ul>
</li><li>[Revision #3334.22.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.22.2)
    <span class="cstm-style datetime">Mon 2012-07-16 06:14:53 +0400</span>
<ul start="1"><li>Allow multiple error codes inside a variable in <code class="fixed" style="white-space:pre-wrap">--error</code> command
</li><li>[Revision #3334.22.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.22.1)
     <span class="cstm-style datetime">Mon 2012-07-16 06:12:11 +0400</span>
<ul start="1"><li>Export sys_errno and errno to variables
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.135](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.135) [merge]
   <span class="cstm-style datetime">Wed 2012-07-18 22:40:15 +0400</span>
<ul start="1"><li>Merge 5.3-&gt;5.5
</li><li>[Revision #2502.561.13](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.561.13)
    <span class="cstm-style datetime">Wed 2012-07-18 15:03:05 +0400</span>
<ul start="1"><li>[MDEV-398](https://jira.mariadb.org/browse/MDEV-398): Sergv related to spacial queries
</li><li>index_merge/intersection is unable to work on GIS indexes, because:
<ol start="1"><li>index scans have no Rowid-Ordered-Retrieval property
</li><li>When one does an index-only read over a GIS index, they do not 
      get the index tuple, because index only contains bounding box of the geometry.
      This is why key_copy() call crashed.
</li></ol>
</li><li>This patch fixes #1, which makes the problem go away. Theoretically, it would 
     be nice to check #2, too, but SE API semantics is not sufficiently precise to do it.
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.134](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.134) [merge]
   <span class="cstm-style datetime">Wed 2012-07-18 22:36:20 +0400</span>
<ul start="1"><li>Merge bug#1007622 from 5.3 to 5.5
</li><li>[Revision #2502.561.12](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.561.12)
    <span class="cstm-style datetime">Tue 2012-06-26 21:43:34 +0300</span>
<ul start="1"><li>Fix for [Bug #1007622](https://bugs.launchpad.net/bugs/1007622)
</li><li>TABLE_LIST::check_single_table made aware about fact that now if table attached to a merged view it can be (unopened) temporary table
     (in 5.2 it was always leaf table or non (in case of several tables)).
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.133](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.133)
   <span class="cstm-style datetime">Mon 2012-07-16 10:48:03 +0300</span>
<ul start="1"><li>fix to satisfy compiler.
</li></ul>
</li><li>[Revision #3334.1.132](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.132)
   <span class="cstm-style datetime">Fri 2012-07-13 22:17:32 +0300</span>
<ul start="1"><li>fixed [MySQL Bug #53775](http://bugs.mysql.com/bug.php?id=53775):
</li><li>Now partition engine adds underlying tables to the QC and ask underlying tables engine permittion to cache the query and return result of the query.
</li><li>Incorrect QC cleanup in case of table registration failure fixe.
</li><li>Unified interface for myisammrg &amp; partitioned engnes for QC.
</li></ul>
</li><li>[Revision #3334.1.131](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.131)
   <span class="cstm-style datetime">Thu 2012-07-12 15:32:35 +0200</span>
<ul start="1"><li>[MDEV-393](https://jira.mariadb.org/browse/MDEV-393). Remove <code class="fixed" style="white-space:pre-wrap">--loose-pbxt=OFF/loose-skip-pbxt</code> from bootstrapper calls to avoid "unknown parameter" warning
</li></ul>
</li><li>[Revision #3334.1.130](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.130)
   <span class="cstm-style datetime">Wed 2012-07-11 16:19:05 +0200</span>
<ul start="1"><li>[Bug #1023404](https://bugs.launchpad.net/bugs/1023404) problems with savepoints and tokudb with 5.5
</li><li>fix incorrect merge
</li></ul>
</li><li>[Revision #3334.1.129](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.129)
   <span class="cstm-style datetime">Tue 2012-07-10 09:02:12 +0300</span>
<ul start="1"><li>Fixed [MDEV-385](https://jira.mariadb.org/browse/MDEV-385): mysqltest running with continue-on-error crashes on a non-SQL command producing an error 
</li></ul>
</li><li>[Revision #3334.1.128](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.128) [merge]
   <span class="cstm-style datetime">Thu 2012-07-05 14:39:01 +0400</span>
<ul start="1"><li>Merge fix for [MDEV-376](https://jira.mariadb.org/browse/MDEV-376)
</li><li>[Revision #3334.21.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.21.1)
    <span class="cstm-style datetime">Wed 2012-07-04 14:34:45 +0400</span>
<ul start="1"><li>[MDEV-376](https://jira.mariadb.org/browse/MDEV-376): Wrong result (missing rows) with index_merge+index_merge_intersection, join
</li><li>Let QUICK_RANGE_SELECT::init_ror_merged_scan() call  quick-&gt;reset() only 
     after we've set the column read bitmaps. 
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.127](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.127)
   <span class="cstm-style datetime">Thu 2012-07-05 09:29:34 +0200</span>
<ul start="1"><li>The variable "table_cache" is deprecated, use the new name "table_open_cache" instead.
</li><li>Thanks to Ivoz for pointing this out.
</li></ul>
</li><li>[Revision #3334.1.126](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.126)
   <span class="cstm-style datetime">Wed 2012-06-27 17:13:12 +0300</span>
<ul start="1"><li>Don't abort InnoDB/XtraDB if one can't allocate resources for AIO
</li><li>Better error messages
</li><li>This fixes that one again can run the test systems with many threads without having to increase fs.aio-max-nr.
</li></ul>
</li><li>[Revision #3334.1.125](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.125)
   <span class="cstm-style datetime">Mon 2012-06-25 18:17:24 +0200</span>
<ul start="1"><li>fix compile error, when building with oqgraph
</li></ul>
</li><li>[Revision #3334.1.124](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.124) [merge]
   <span class="cstm-style datetime">Sun 2012-06-24 09:10:11 -0700</span>
<ul start="1"><li>Merge 5.3-&gt;5.5.
</li><li>[Revision #2502.561.11](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.561.11) [merge]
    <span class="cstm-style datetime">Sat 2012-06-23 15:00:05 -0700</span>
<ul start="1"><li>Merge 5.2-&gt;5.3
</li><li>[Revision #2502.562.9](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.562.9)
     <span class="cstm-style datetime">Sat 2012-06-23 12:19:07 -0700</span>
<ul start="1"><li>Fixed bug [MDEV-360](https://jira.mariadb.org/browse/MDEV-360).
              The bug was the result of the incomplete fix for bug lp bug 1008293.
</li></ul>
</li><li>[Revision #2502.562.8](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.562.8)
     <span class="cstm-style datetime">Mon 2012-06-18 22:32:17 -0700</span>
<ul start="1"><li>Fixed bug [MDEV-354](https://jira.mariadb.org/browse/MDEV-354).
</li><li>Virtual columns of ENUM and SET data types were not supported properly
      in the original patch that introduced virtual columns into [MariaDB 5.2](/kb/en/what-is-mariadb-52/).
</li><li>The problem was that for any  virtual column the patch used the 
      interval_id field of the definition of the column in the frm file as
      a reference to the virtual column expression.
</li><li>The fix stores the optional interval_id of the virtual column in the
      extended header of the virtual column expression. 
</li></ul>
</li></ul>
</li><li>[Revision #2502.561.10](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.561.10)
    <span class="cstm-style datetime">Fri 2012-06-22 14:14:22 +0400</span>
<ul start="1"><li>Added comment about QUICK_RANGE_SELECT::free_cond being unused.
</li></ul>
</li><li>[Revision #2502.561.9](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.561.9)
    <span class="cstm-style datetime">Thu 2012-06-21 14:33:36 +0400</span>
<ul start="1"><li>Update test results (checked)
</li></ul>
</li><li>[Revision #2502.561.8](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.561.8)
    <span class="cstm-style datetime">Wed 2012-06-20 22:30:24 +0400</span>
<ul start="1"><li>Update test results.
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.123](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.123)
   <span class="cstm-style datetime">Sat 2012-06-23 20:12:54 +0400</span>
<ul start="1"><li>Add back testcase for [Bug #817966](https://bugs.launchpad.net/bugs/817966) (was lost in the merge)
</li></ul>
</li><li>[Revision #3334.1.122](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.122)
   <span class="cstm-style datetime">Fri 2012-06-22 10:42:55 +0200</span>
<ul start="1"><li>[MDEV-342](https://jira.mariadb.org/browse/MDEV-342): fix two race conditions in the test case that could occasionally cause spurious failures.
</li></ul>
</li><li>[Revision #3334.1.121](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.121)
   <span class="cstm-style datetime">Thu 2012-06-21 21:17:34 +0200</span>
<ul start="1"><li>[MDEV-342](https://jira.mariadb.org/browse/MDEV-342): Do not mark old binlog file as cleanly closed during rotate until
    the new file is fully synced to disk and binlog index. This fixes a window
    where a crash would leave next server restart unable to detect that a crash
    occured, causing recovery to fail.
</li></ul>
</li><li>[Revision #3334.1.120](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.120)
   <span class="cstm-style datetime">Thu 2012-06-21 19:02:53 +0200</span>
<ul start="1"><li>[MDEV-359](https://jira.mariadb.org/browse/MDEV-359): Fix another case where switch-off semisync could cause a race that ended with server crash.
</li><li>This one was when the code releases and reaquires the lock with pthread_cond_wait() - and semisync is switched off meanwhile.
</li></ul>
</li><li>[Revision #3334.1.119](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.119)
   <span class="cstm-style datetime">Thu 2012-06-21 17:39:21 +0200</span>
<ul start="1"><li>Use the portable form of INSTALL PLUGIN in rpl_mdev359.test
</li></ul>
</li><li>[Revision #3334.1.118](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.118)
   <span class="cstm-style datetime">Thu 2012-06-21 14:00:19 +0200</span>
<ul start="1"><li>fixing the order of includes in the rpl_mdev359.test
</li></ul>
</li><li>[Revision #3334.1.117](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.117)
   <span class="cstm-style datetime">Thu 2012-06-21 11:52:54 +0200</span>
<ul start="1"><li>[MDEV-359](https://jira.mariadb.org/browse/MDEV-359): Server crash when SET GLOBAL rpl_semi_sync_master_enabled = OFF
</li><li>The semisync code does a fast-but-unsafe check for enabled or not without lock,
    followed by a slow-but-safe check under lock. However, if the slow check failed,
    the code still referenced not valid data (in an assert() expression), causing a
    crash.
</li><li>Fixed by not running the incorrect assert when semisync is disabled.
</li></ul>
</li><li>[Revision #3334.1.116](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.116)
   <span class="cstm-style datetime">Thu 2012-06-21 11:26:53 +0200</span>
<ul start="1"><li>[MDEV-349](https://jira.mariadb.org/browse/MDEV-349) 5.5 xtradb innodb_prefix_index_liftedlimit crash with valgrind
</li><li>This is XtraDB [Bug #1015109](https://bugs.launchpad.net/bugs/1015109), introduced by innodb_split_buf_pool_mutex.patch
</li><li>Comment the offending assertion, until the fixed XtraDB is available
</li></ul>
</li></ul>
- [Revision #3342](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3342)
  <span class="cstm-style datetime">Mon 2012-07-23 11:15:59 +0300</span>
<ul start="1"><li>New version of mysqld_multi. Building Galera tree fully first time in buildbot
</li></ul>
- [Revision #3341](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3341)
  <span class="cstm-style datetime">Wed 2012-06-13 00:23:32 +0300</span>
<ul start="1"><li>References [Bug #1011983](https://bugs.launchpad.net/bugs/1011983)
</li><li>Merged from codership-mysql/5.5 revision 3758
</li></ul>
- [Revision #3340](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3340) [merge]
  <span class="cstm-style datetime">Tue 2012-06-12 16:34:18 +0300</span>
<ul start="1"><li>references [Bug #1011983](https://bugs.launchpad.net/bugs/1011983)
</li><li>Merged latest MariaDB development in: bzr merge lp:maria/5.5
<ul start="1" style="list-style: none"><li>=&gt; 
</li><li>Text conflict in CMakeLists.txt
</li><li>Text conflict in sql/handler.h
</li><li>Text conflict in support-files/CMakeLists.txt
</li><li>3 conflicts
</li></ul>
</li></ul>
- [Revision #3339](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3339)
  <span class="cstm-style datetime">Tue 2012-06-12 10:55:11 +0300</span>
<ul start="1"><li>References [Bug #1011983](https://bugs.launchpad.net/bugs/1011983)
</li><li>Merged from codership-mysql/5.5 changes revisions 3743-3756
</li></ul>
- [Revision #3338](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3338)
  <span class="cstm-style datetime">Thu 2012-04-26 20:18:30 +0300</span>
<ul start="1"><li>Merged changes from lp:codership-mysql up to rev 3743
<ul start="1" style="list-style: none"><li>-r3725..3737
</li><li>-r3738..3740
</li><li>-r3741..3743
</li></ul>
</li></ul>
- [Revision #3337](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3337) [merge]
  <span class="cstm-style datetime">Thu 2012-04-26 13:59:35 +0300</span>
<ul start="1"><li>Merge with [mariaDB 5.5.23](/kb/en/mariadb-5523-release-notes/): bzr merge lp:maria/5.5
</li></ul>
- [Revision #3336](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3336)
  <span class="cstm-style datetime">Thu 2012-04-26 13:09:06 +0300</span>
<ul start="1"><li>Added wsrep specific files
</li></ul>
- [Revision #3335](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3335)
  <span class="cstm-style datetime">Fri 2012-04-13 01:33:24 +0300</span>
<ul start="1"><li>Initial push of codership-wsrep API implementation for MariaDB. 
</li><li>Merge of:
<ul start="1" style="list-style: none"><li>lp:maria/5.5, #3334: [http://bazaar.launchpad.net/~maria-captains/maria/5.5/revision/3334](http://bazaar.launchpad.net/~maria-captains/maria/5.5/revision/3334)
</li><li>lp:codership-mysql/5.5, #3725: [http://bazaar.launchpad.net/~codership/codership-mysql/wsrep-5.5/revision/3725](http://bazaar.launchpad.net/~codership/codership-mysql/wsrep-5.5/revision/3725)
</li></ul>
</li></ul>