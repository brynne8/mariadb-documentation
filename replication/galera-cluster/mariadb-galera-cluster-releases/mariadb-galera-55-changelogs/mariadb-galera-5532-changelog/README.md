# MariaDB Galera 5.5.32 Changelog

The most recent [MariaDB Galera Cluster 5.5](/kb/en/galera/) release is:<br>
<span class="cstm-style lead"><strong>[MariaDB Galera Cluster 5.5.63](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-cluster-5563-release-notes/)</strong> [Download<span>&nbsp;</span>Now](https://downloads.mariadb.org/mariadb-galera/5.5)</span>

[Download](http://downloads.mariadb.org/mariadb-galera/5.5.32) |
[Release Notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-5532-release-notes/) |
<strong>Changelog</strong> |
[Overview of Galera](/replication/galera-cluster/what-is-mariadb-galera-cluster/)

<strong>Release date:</strong> 30 Aug 2013

For the highlights of this release, see the [release notes](/replication/galera-cluster/mariadb-galera-cluster-releases/mariadb-galera-55-release-notes/mariadb-galera-5532-release-notes/).

The revision number links will take you to the revision's page on Launchpad. On
Launchpad you can view more details of the revision and view diffs of the code
modified in that revision.

- [Revision #3417](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3417)
  <span class="cstm-style datetime">Tue 2013-08-27 23:40:49 +0300</span>
<ul start="1"><li>References: [MDEV-4953](https://jira.mariadb.org/browse/MDEV-4953)  Calling ha_start_of_new_statement() for all table handlers under partition engine. This will enable innodb transactions to be declared as read/write.
</li></ul>
- [Revision #3416](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3416)
  <span class="cstm-style datetime">Tue 2013-08-27 09:06:04 +0300</span>
<ul start="1"><li>References [Bug #1216904](https://bugs.launchpad.net/bugs/1216904) - guaranteeing native "create table like.." processing when running without wsrep provider Merged the fix from lp:codership/codership-mysql/5.5-23, rev 3906
</li></ul>
- [Revision #3415](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3415)
  <span class="cstm-style datetime">Fri 2013-08-23 12:21:00 +0300</span>
<ul start="1"><li>References: [Bug #1206129](https://bugs.launchpad.net/bugs/1206129) - Merged revision 3904 from lp:codership/codership-mysql/5.5-23
</li></ul>
- [Revision #3414](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3414)
  <span class="cstm-style datetime">Wed 2013-08-21 17:17:30 +0300</span>
<ul start="1"><li>Merged with lp:codership/codership-mysql/5.5-23, up to revision 3903
</li></ul>
- [Revision #3413](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3413)
  <span class="cstm-style datetime">Wed 2013-08-21 16:37:22 +0300</span>
<ul start="1"><li>References [MDEV-4404](https://jira.mariadb.org/browse/MDEV-4404) - Added log message to catch information of log event corruption
</li></ul>
- [Revision #3412](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3412) [merge]
  <span class="cstm-style datetime">Wed 2013-08-21 16:34:31 +0300</span>
<ul start="1"><li>Merge with [mariadb 5.5](/kb/en/what-is-mariadb-55/): <code class="fixed" style="white-space:pre-wrap">bzr merge lp:maria/5.5 --rtag:mariadb-5.5.32</code>
</li><li>[Revision #3334.1.504](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.504)
   <span class="cstm-style datetime">Wed 2013-07-17 17:03:59 +0300</span>
<ul start="1"><li>Revert of marko.makela@oracle.com-20130430103950-j353faze84zzk9xf for xtradb (fix of [http://bugs.mysql.com/bug.php?id=69623](http://bugs.mysql.com/bug.php?id=69623))
</li></ul>
</li><li>[Revision #3334.1.503](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.503)
   <span class="cstm-style datetime">Wed 2013-07-17 16:42:13 +0300</span>
<ul start="1"><li>Fix for [MDEV-4219](https://jira.mariadb.org/browse/MDEV-4219) A simple select query returns random data (upstream bug#68473)
</li></ul>
</li><li>[Revision #3334.1.502](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.502) [merge]
   <span class="cstm-style datetime">Tue 2013-07-16 19:30:39 +0200</span>
<ul start="1"><li>merge Percona-Server-5.5.32-rel31.0.tar.gz
</li><li>[Revision #0.12.63](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/0.12.63)
    <span class="cstm-style datetime">Tue 2013-07-16 14:55:47 +0200</span>
<ul start="1"><li>Percona-Server-5.5.32-rel31.0.tar.gz
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.501](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.501) [merge]
   <span class="cstm-style datetime">Tue 2013-07-16 19:09:54 +0200</span>
<ul start="1"><li>mysql-5.5.32 merge
</li><li>[Revision #3077.187.102](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.102)
    <span class="cstm-style datetime">Thu 2013-05-16 17:33:32 +0200</span>
<ul start="1"><li>Fix for BUG# 16812255: Removing the <code class="fixed" style="white-space:pre-wrap">--random-password</code> option which is supported only for MYSQL server versions 5.6 and above.
</li></ul>
</li><li>[Revision #3077.187.101](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.101)
    <span class="cstm-style datetime">Thu 2013-05-16 10:24:26 +0200</span>
<ul start="1"><li>Changes to verify the solaris upgrade issue.
</li></ul>
</li><li>[Revision #3077.187.100](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.100)
    <span class="cstm-style datetime">Wed 2013-05-15 16:29:31 +0200</span>
<ul start="1"><li>Fixing the RPM-ULN build issue by ignoring the postinstall_check.sh.
</li></ul>
</li><li>[Revision #3077.187.99](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.99)
    <span class="cstm-style datetime">Wed 2013-05-15 15:37:20 +0200</span>
<ul start="1"><li>Bug 16812255 - 5.5.32 PKG INSTALLATION FAILED DURING MYSQL_INSTALL_DB EXECUTION
</li></ul>
</li><li>[Revision #3077.187.98](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.98)
    <span class="cstm-style datetime">Mon 2013-05-13 10:21:09 +0200</span>
<ul start="1"><li>Updated copyright year information
</li></ul>
</li><li>[Revision #3077.187.97](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.97)
    <span class="cstm-style datetime">Mon 2013-05-13 09:46:44 +0200</span>
<ul start="1"><li>Adding fix for Bug#16798868
</li></ul>
</li><li>[Revision #3077.187.96](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.96)
    <span class="cstm-style datetime">Wed 2013-05-08 12:08:20 +0200</span>
<ul start="1"><li>Bug#16779374: NEW ERROR MESSAGE ADDED TO 5.5 AFTER 5.6 GA - REUSING               NUMBER ALREADY USED BY 5.6
</li></ul>
</li><li>[Revision #3077.187.95](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.95)
    <span class="cstm-style datetime">Tue 2013-05-07 14:36:46 +0200</span>
<ul start="1"><li>ULN-RPMs bug fix for BR16298542
</li></ul>
</li><li>[Revision #3077.187.94](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.94)
    <span class="cstm-style datetime">Mon 2013-05-06 20:31:26 +0530</span>
<ul start="1"><li>Bug #16722314 FOREIGN KEY ID MODIFIED DURING EXPORT Bug #16754901 PARS_INFO_FREE NOT CALLED IN DICT_CREATE_ADD_FOREIGN_TO_DICTIONARY
</li></ul>
</li><li>[Revision #3077.187.93](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.93)
    <span class="cstm-style datetime">Mon 2013-05-06 16:06:32 +0200</span>
<ul start="1"><li>Bug#16757869: INNODB: POSSIBLE REGRESSION IN 5.5.31, BUG#16004999
</li></ul>
</li><li>[Revision #3077.187.92](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.92)
    <span class="cstm-style datetime">Mon 2013-05-06 15:19:37 +0200</span>
<ul start="1"><li>Updated spec file for Bug16488773
</li></ul>
</li><li>[Revision #3077.187.91](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.91)
    <span class="cstm-style datetime">Fri 2013-05-03 16:39:17 +0300</span>
</li><li>[Revision #3077.187.90](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.90) [merge]
    <span class="cstm-style datetime">Tue 2013-04-30 20:40:38 +0200</span>
<ul start="1"><li>merge from mysql-5.1
</li><li>[Revision #2661.848.26](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.26)
     <span class="cstm-style datetime">Tue 2013-04-30 20:39:12 +0200</span>
<ul start="1"><li>Bug#16405422 - RECOVERY FAILURE, ASSERT !RECV_NO_LOG_WRITE
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.89](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.89) [merge]
    <span class="cstm-style datetime">Tue 2013-04-30 22:46:37 +0530</span>
<ul start="1"><li>BUG#16222245 - CRASH WITH EXPLAIN FOR A QUERY WITH LOOSE SCAN FOR  GROUP BY, MYISAM 
</li><li>[Revision #2661.848.25](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.25)
     <span class="cstm-style datetime">Tue 2013-04-30 22:38:34 +0530</span>
<ul start="1"><li>BUG#16222245 - CRASH WITH EXPLAIN FOR A QUERY WITH LOOSE SCAN FOR  GROUP BY, MYISAM 
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.88](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.88)
    <span class="cstm-style datetime">Tue 2013-04-30 13:39:50 +0300</span>
<ul start="1"><li>Bug#16720368 INNODB IGNORES *.IBD FILE BREAKAGE AT STARTUP
</li></ul>
</li><li>[Revision #3077.187.87](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.87)
    <span class="cstm-style datetime">Sat 2013-04-27 16:04:54 +0800</span>
<ul start="1"><li>Bug #13004581 	BLACKHOLE BINARY LOG WITH ROW IGNORES UPDATE AND DELETE STATEMENTS
</li></ul>
</li><li>[Revision #3077.187.86](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.86)
    <span class="cstm-style datetime">Thu 2013-04-25 11:56:26 +0530</span>
<ul start="1"><li>BUG#16698172-CANNOT DO POINT-IN-TIME RECOVERY FOR SINGLE DATABASE; MYSQLBINLOG
</li></ul>
</li><li>[Revision #3077.187.85](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.85)
    <span class="cstm-style datetime">Wed 2013-04-24 17:21:42 +0300</span>
<ul start="1"><li>Bug #16680313: CLIENT DOESN'T READ PLUGIN-DIR FROM MY.CNF SET BY       MYSQL_READ_DEFAULT_FILE Parsing of the plugin-dir config file option was not working due to a typo. Fixed the typo. No test case can be added due to lack of support for  defaults-exitra-file testing in mysql-test-run.pl. Thanks to Sinisa for contributing the fix.
</li></ul>
</li><li>[Revision #3077.187.84](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.84) [merge]
    <span class="cstm-style datetime">Wed 2013-04-24 13:34:11 +0530</span>
<ul start="1"><li>[Revision #2661.848.24](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.24)
     <span class="cstm-style datetime">Wed 2013-04-24 13:31:10 +0530</span>
</li></ul>
</li><li>[Revision #3077.187.83](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.83) [merge]
    <span class="cstm-style datetime">Wed 2013-04-24 08:48:34 +0200</span>
<ul start="1"><li>Null merge from mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.848.23](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.23)
     <span class="cstm-style datetime">Wed 2013-04-24 08:47:30 +0200</span>
<ul start="1"><li>Bug #15973904 INNODB PARTITION CODE HOLDS LOCK_OPEN AND SLEEPS WHILE  OPENING MISSING PARTITION
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.82](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.82)
    <span class="cstm-style datetime">Wed 2013-04-24 08:42:59 +0200</span>
<ul start="1"><li>Merge from mysql-5.1 to mysql-5.5
</li></ul>
</li><li>[Revision #3077.187.81](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.81) [merge]
    <span class="cstm-style datetime">Mon 2013-04-22 14:30:47 +0200</span>
<ul start="1"><li>Upmerge of the 5.1.69 build
</li><li>[Revision #2661.848.22](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.22)
     <span class="cstm-style datetime">Mon 2013-04-22 14:01:07 +0200</span>
<ul start="1"><li>Merge from mysql-5.1.69-release
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.80](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.80) [merge]
    <span class="cstm-style datetime">Sat 2013-04-20 12:36:11 +0530</span>
<ul start="1"><li>Bug#16073689 : CRASH IN ITEM_FUNC_MATCH::INIT_SEARCH 
</li><li>[Revision #2661.848.21](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.21)
     <span class="cstm-style datetime">Sat 2013-04-20 12:28:22 +0530</span>
<ul start="1"><li>Bug#16073689 : CRASH IN ITEM_FUNC_MATCH::INIT_SEARCH 
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.79](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.79) [merge]
    <span class="cstm-style datetime">Thu 2013-04-18 12:52:59 +0200</span>
<ul start="1"><li>Merge from mysql-5.5.31-release
</li></ul>
</li><li>[Revision #3077.187.78](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.78)
    <span class="cstm-style datetime">Wed 2013-04-17 09:26:51 +0200</span>
<ul start="1"><li>Bug#16626742 IN MY_MD5FINAL IN MYSYS/MD5.C, CTX IS NOT PROPERLY ZEROED AS INTENDED
</li></ul>
</li><li>[Revision #3077.187.77](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.77)
    <span class="cstm-style datetime">Tue 2013-04-16 16:26:45 +0530</span>
<ul start="1"><li>Bug #16632543 - INCORRECT VALUE OF BOGOMIPS IN MYSQLTEST
</li></ul>
</li><li>[Revision #3077.187.76](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.76) [merge]
    <span class="cstm-style datetime">Tue 2013-04-16 12:17:18 +0200</span>
<ul start="1"><li>Merging the changes for Bug 16633169 - MYSQL.INFO CONTAINS OUTDATED INFORMATION.
</li><li>[Revision #2661.848.20](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.20)
     <span class="cstm-style datetime">Tue 2013-04-16 12:12:18 +0200</span>
<ul start="1"><li>Bug 16633169 - MYSQL.INFO CONTAINS OUTDATED INFORMATION.
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.75](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.75) [merge]
    <span class="cstm-style datetime">Sun 2013-04-14 08:09:56 +0530</span>
<ul start="1"><li>Merge from 5.1 to 5.5
</li><li>[Revision #2661.848.19](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.19)
     <span class="cstm-style datetime">Sun 2013-04-14 07:30:49 +0530</span>
<ul start="1"><li>Bug#16347426:ASSERTION FAILED: (SELECT_INSERT &amp;&amp;              !TABLES-&gt;NEXT_NAME_RESOLUTION_TABLE) || !TAB
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.74](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.74)
    <span class="cstm-style datetime">Fri 2013-04-12 14:18:21 +0530</span>
<ul start="1"><li>BUG#16615117 MYSQLDUMP PRODUCES A CHANGE MASTER STATEMENT WITH A PORT NUMBER ENCLOSED IN QUOTES
</li></ul>
</li><li>[Revision #3077.187.73](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.73)
    <span class="cstm-style datetime">Fri 2013-04-12 09:39:56 +0200</span>
<ul start="1"><li>Bug#16540042: WRONG QUERY RESULT WHEN USING RANGE OVER                PARTIAL INDEX
</li></ul>
</li><li>[Revision #3077.187.72](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.72)
    <span class="cstm-style datetime">Thu 2013-04-11 10:50:50 +0800</span>
<ul start="1"><li>Bug	:#16005310 FIx bug - INNODB_ROW_LOCK_TIME_MAX SEEMS TO HAVE AN OVERFLOW
</li></ul>
</li><li>[Revision #3077.187.71](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.71)
    <span class="cstm-style datetime">Wed 2013-04-10 16:43:09 +0200</span>
<ul start="1"><li>Bug#16395606 SCRIPTS MISSING EXECUTE BIT
</li></ul>
</li><li>[Revision #3077.187.70](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.70)
    <span class="cstm-style datetime">Wed 2013-04-10 11:50:41 +0530</span>
<ul start="1"><li>BUG#16402143 - STACK CORRUPTION IN DBUG_EXPLAIN DESCRIPTION AND FIX: DBUG_EXPLAIN result in buffer overflow when the DEBUG variable values length exceed 255. In _db_explain_ function which call macro str_to_buf incorrectly passes the length of buf avaliable to strnmov as len+1. The fix calculates the avaliable space in buf and passes it to strnxmov.
</li></ul>
</li><li>[Revision #3077.187.69](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.69) [merge]
    <span class="cstm-style datetime">Tue 2013-04-09 14:03:35 +0530</span>
<ul start="1"><li>local merge.
</li><li>[Revision #2661.848.18](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.18)
     <span class="cstm-style datetime">Tue 2013-04-09 14:00:05 +0530</span>
<ul start="1"><li>Backporting patch for bug#15852074.
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.68](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.68) [merge]
    <span class="cstm-style datetime">Mon 2013-04-08 18:53:24 +0530</span>
<ul start="1"><li>null merge
</li><li>[Revision #2661.848.17](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.17)
     <span class="cstm-style datetime">Mon 2013-04-08 18:48:57 +0530</span>
</li></ul>
</li><li>[Revision #3077.187.67](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.67) [merge]
    <span class="cstm-style datetime">Mon 2013-04-08 18:14:06 +0530</span>
<ul start="1"><li>[Revision #2661.848.16](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.16)
     <span class="cstm-style datetime">Mon 2013-04-08 18:12:39 +0530</span>
</li></ul>
</li><li>[Revision #3077.187.66](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.66)
    <span class="cstm-style datetime">Mon 2013-04-08 15:25:45 +0530</span>
<ul start="1"><li>BUG#15978766 - TEST VALGRIND_REPORT FAILS INNODB TESTS 
</li></ul>
</li><li>[Revision #3077.187.65](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.65)
    <span class="cstm-style datetime">Thu 2013-04-04 14:54:16 +0530</span>
<ul start="1"><li>Bug #16401597 - MTR V1 RETURNS INCORRECT PATH TO VARIABLE @@BASEDIR
</li></ul>
</li><li>[Revision #3077.187.64](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.64)
    <span class="cstm-style datetime">Wed 2013-04-03 18:09:37 +0200</span>
<ul start="1"><li>Bug 16534721 - MYSQL_INSTALL_DB RUNS AGAIN DURING UPGRADE EVEN DATA DIRECTORY EXISTS
</li></ul>
</li><li>[Revision #3077.187.63](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.63) [merge]
    <span class="cstm-style datetime">Tue 2013-04-02 16:20:49 +0200</span>
<ul start="1"><li>merge 5.1 =&gt; 5.5
</li><li>[Revision #2661.848.15](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.15)
     <span class="cstm-style datetime">Tue 2013-04-02 16:05:10 +0200</span>
<ul start="1"><li>Bug#14700180 CRASH IN COPY_FUNCS This is a backport of the fix for Bug#13966809 CRASH IN COPY_FUNCS WHEN GROUPING BY OUTER QUERY BLOB FIELD IN SUBQUERY
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.62](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.62)
    <span class="cstm-style datetime">Tue 2013-04-02 11:14:39 +0200</span>
<ul start="1"><li>Bug#11765629 CMAKE: CAN SUPPRESS INSTALLATION OF SQL-BENCH, BUT NOT MYSQL-TEST
</li></ul>
</li><li>[Revision #3077.187.61](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.61) [merge]
    <span class="cstm-style datetime">Tue 2013-04-02 11:17:06 +0530</span>
<ul start="1"><li>[Revision #2661.848.14](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.14)
     <span class="cstm-style datetime">Tue 2013-04-02 11:16:26 +0530</span>
</li></ul>
</li><li>[Revision #3077.187.60](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.60) [merge]
    <span class="cstm-style datetime">Mon 2013-04-01 13:45:27 +0530</span>
<ul start="1"><li>[Revision #2661.848.13](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.13)
     <span class="cstm-style datetime">Mon 2013-04-01 12:26:55 +0530</span>
</li></ul>
</li><li>[Revision #3077.187.59](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.59) [merge]
    <span class="cstm-style datetime">Sun 2013-03-31 06:52:16 +0530</span>
<ul start="1"><li>Merge from 5.1 to 5.5
</li><li>[Revision #2661.848.12](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.12)
     <span class="cstm-style datetime">Sun 2013-03-31 06:48:30 +0530</span>
<ul start="1"><li>Bug #16347343 : CRASH, GROUP_CONCAT, DERIVED TABLES
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.58](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.58)
    <span class="cstm-style datetime">Sat 2013-03-30 19:24:54 +0530</span>
<ul start="1"><li>Bug#14261010: ON DUPLICATE KEY UPDATE CRASHES THE SERVER
</li></ul>
</li><li>[Revision #3077.187.57](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.57) [merge]
    <span class="cstm-style datetime">Fri 2013-03-29 22:11:33 +0530</span>
<ul start="1"><li>Merge from mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.848.11](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.11)
     <span class="cstm-style datetime">Fri 2013-03-29 22:01:10 +0530</span>
<ul start="1"><li>Bug #16244691 SERVER GONE AWAY ERROR OCCURS DEPENDING ON THE NUMBER OF TABLE/KEY RELATIONS
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.56](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.56)
    <span class="cstm-style datetime">Fri 2013-03-29 16:33:33 +0530</span>
<ul start="1"><li>Bug #16402124 - MTR PROCESSES CERTAIN ASSIGNED VARDIR VALUES WRONG
</li></ul>
</li><li>[Revision #3077.187.55](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.55) [merge]
    <span class="cstm-style datetime">Fri 2013-03-29 15:14:38 +0530</span>
<ul start="1"><li>[Revision #2661.848.10](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.10)
     <span class="cstm-style datetime">Fri 2013-03-29 15:09:14 +0530</span>
</li></ul>
</li><li>[Revision #3077.187.54](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.54)
    <span class="cstm-style datetime">Fri 2013-03-29 11:44:42 +0530</span>
</li><li>[Revision #3077.187.53](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.53)
    <span class="cstm-style datetime">Fri 2013-03-29 09:28:31 +0530</span>
<ul start="1"><li>Bug#15948818-SEMI-SYNC ENABLED MASTER CRASHES WHEN EVENT SCHEDULER DROPS EVENTS
</li></ul>
</li><li>[Revision #3077.187.52](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.52) [merge]
    <span class="cstm-style datetime">Thu 2013-03-28 17:41:22 +0200</span>
<ul start="1"><li>merge
</li><li>[Revision #2661.848.9](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.9)
     <span class="cstm-style datetime">Thu 2013-03-28 17:37:29 +0200</span>
<ul start="1"><li>Addendum #1 to the fix for bug #16451878 : GEOMETRY QUERY CRASHES SERVER
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.51](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.51) [merge]
    <span class="cstm-style datetime">Thu 2013-03-28 19:17:28 +0530</span>
<ul start="1"><li>Merge from 5.1 to 5.5
</li><li>[Revision #2661.848.8](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.8)
     <span class="cstm-style datetime">Thu 2013-03-28 19:11:26 +0530</span>
<ul start="1"><li>BUG#11753852: IF() VALUES ARE EVALUATED DIFFERENTLY IN A               REGULAR SQL VS PREPARED STATEMENT
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.50](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.50) [merge]
    <span class="cstm-style datetime">Thu 2013-03-28 14:18:51 +0530</span>
<ul start="1"><li>Merge from mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.848.7](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.7)
     <span class="cstm-style datetime">Thu 2013-03-28 14:14:39 +0530</span>
<ul start="1"><li>Bug#14324766:PARTIALLY WRITTEN INSERT STATEMENT IN BINLOG NO ERRORS REPORTED
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.49](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.49)
    <span class="cstm-style datetime">Thu 2013-03-28 11:47:43 +0530</span>
<ul start="1"><li>Bug #16403186 - MTR ON WINDOWS SHOULD NOT TRY TO START CDB IF RUNNING WITH PARALLEL
</li></ul>
</li><li>[Revision #3077.187.48](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.48) [merge]
    <span class="cstm-style datetime">Thu 2013-03-28 10:43:50 +0530</span>
<ul start="1"><li>Null merge from mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.848.6](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.6)
     <span class="cstm-style datetime">Thu 2013-03-28 10:42:42 +0530</span>
<ul start="1"><li>Bug #16244691 SERVER GONE AWAY ERROR OCCURS DEPENDING ON THE NUMBER OF  TABLE/KEY RELATIONS
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.47](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.47) [merge]
    <span class="cstm-style datetime">Thu 2013-03-28 10:25:23 +0530</span>
<ul start="1"><li>Merge from mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.849.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.849.1)
     <span class="cstm-style datetime">Wed 2013-03-27 11:11:38 +0530</span>
<ul start="1"><li>Bug #16244691 SERVER GONE AWAY ERROR OCCURS DEPENDING ON THE NUMBER OF  TABLE/KEY RELATIONS
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.46](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.46) [merge]
    <span class="cstm-style datetime">Wed 2013-03-27 16:06:33 +0200</span>
<ul start="1"><li>merge 5.1-&gt;5.5
</li><li>[Revision #2661.848.5](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.5)
     <span class="cstm-style datetime">Wed 2013-03-27 16:03:00 +0200</span>
<ul start="1"><li>Bug #16451878: GEOMETRY QUERY CRASHES SERVER
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.45](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.45) [merge]
    <span class="cstm-style datetime">Wed 2013-03-27 11:22:25 +0000</span>
<ul start="1"><li>BUG#16541422: LOG-SLAVE-UPDATES + REPLICATE-WILD-IGNORE-TABLE FAILS FOR USER VARIABLES
</li><li>[Revision #2661.848.4](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.4)
     <span class="cstm-style datetime">Wed 2013-03-27 11:19:29 +0000</span>
<ul start="1"><li>BUG#16541422: LOG-SLAVE-UPDATES + REPLICATE-WILD-IGNORE-TABLE FAILS FOR USER VARIABLES
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.44](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.44) [merge]
    <span class="cstm-style datetime">Wed 2013-03-27 11:59:40 +0530</span>
<ul start="1"><li>Merge from mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.848.3](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.3)
     <span class="cstm-style datetime">Wed 2013-03-27 11:53:01 +0530</span>
<ul start="1"><li>Bug#11829838: ALTER TABLE NOT BINLOGGED WITH <code class="fixed" style="white-space:pre-wrap">--BINLOG-IGNORE-DB</code> AND FULLY QUALIFIED TABLE
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.43](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.43) [merge]
    <span class="cstm-style datetime">Tue 2013-03-26 23:11:55 +0200</span>
<ul start="1"><li>merge from 5.1-&gt;5.5 repo.
</li><li>[Revision #2661.848.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.2) [merge]
     <span class="cstm-style datetime">Tue 2013-03-26 23:10:42 +0200</span>
<ul start="1"><li>merge from 5.1 repo.
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.42](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.42)
    <span class="cstm-style datetime">Tue 2013-03-26 21:45:39 +0200</span>
</li><li>[Revision #3077.187.41](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.41) [merge]
    <span class="cstm-style datetime">Tue 2013-03-26 20:52:01 +0200</span>
<ul start="1"><li>merge from 5.1
</li><li>[Revision #2661.848.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.848.1)
     <span class="cstm-style datetime">Tue 2013-03-26 19:24:01 +0200</span>
<ul start="1"><li>Bug#16541422 LOG-SLAVE-UPDATES + REPLICATE-WILD-IGNORE-TABLE FAILS FOR USER VARIABLES
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.40](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.40) [merge]
    <span class="cstm-style datetime">Tue 2013-03-26 08:24:11 +0100</span>
<ul start="1"><li>NULL merge 5.1 =&gt; 5.5
</li><li>[Revision #2661.844.69](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.69)
     <span class="cstm-style datetime">Tue 2013-03-26 08:22:45 +0100</span>
<ul start="1"><li>Bug#62856 Check for "stack overrun" doesn't work with gcc-4.6, server crashes  Bug#13243248 CHECK FOR "STACK OVERRUN" DOESN'T WORK WITH GCC-4.6, SERVER CRASHES
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.39](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.39)
    <span class="cstm-style datetime">Mon 2013-03-25 11:27:12 +0530</span>
<ul start="1"><li>BUG#16438800 - SLAVE_MAX_ALLOWED_PACKET NOT HONORED ON SLAVE IO CONNECT
</li></ul>
</li><li>[Revision #3077.187.38](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.38) [merge]
    <span class="cstm-style datetime">Fri 2013-03-22 20:16:53 +0530</span>
<ul start="1"><li>local merge.
</li><li>[Revision #2661.844.68](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.68)
     <span class="cstm-style datetime">Fri 2013-03-22 20:00:40 +0530</span>
<ul start="1"><li>Bug#12671635 : Updating embedded tests.
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.37](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.37) [merge]
    <span class="cstm-style datetime">Fri 2013-03-22 15:33:59 +0530</span>
<ul start="1"><li>local merge.
</li><li>[Revision #2661.844.67](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.67)
     <span class="cstm-style datetime">Fri 2013-03-22 15:29:57 +0530</span>
<ul start="1"><li>Bug#12671635 : Fixing test cases.
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.36](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.36)
    <span class="cstm-style datetime">Fri 2013-03-22 14:55:30 +0530</span>
<ul start="1"><li>Bug#16500013 : post-fix
</li></ul>
</li><li>[Revision #3077.187.35](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.35) [merge]
    <span class="cstm-style datetime">Thu 2013-03-21 23:40:25 +0530</span>
<ul start="1"><li>Merge of patch for Bug#12671635 from mysql-5.1.
</li><li>[Revision #2661.844.66](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.66)
     <span class="cstm-style datetime">Thu 2013-03-21 23:36:02 +0530</span>
<ul start="1"><li>Bug#12671635 HELP-TABLEFORMAT DOESN'T MATCH HELP-FILES
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.34](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.34)
    <span class="cstm-style datetime">Thu 2013-03-21 22:51:40 +0530</span>
<ul start="1"><li>Bug#16500013 : ADD VERSION CHECK TO MYSQL_UPGRADE
</li></ul>
</li><li>[Revision #3077.187.33](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.33)
    <span class="cstm-style datetime">Thu 2013-03-21 11:40:43 +0530</span>
<ul start="1"><li>Bug #16051728 SERVER CRASHES IN ADD_IDENTIFIER ON CONCURRENT ALTER TABLE AND SHOW ENGINE INNOD
</li></ul>
</li><li>[Revision #3077.187.32](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.32) [merge]
    <span class="cstm-style datetime">Wed 2013-03-20 17:52:15 +0100</span>
<ul start="1"><li>Null merge from 5.1 for permission changes.
</li><li>[Revision #2661.844.65](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.65)
     <span class="cstm-style datetime">Wed 2013-03-20 17:49:30 +0100</span>
<ul start="1"><li>Correcting the permissions of executable files.
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.31](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.31)
    <span class="cstm-style datetime">Wed 2013-03-20 17:50:15 +0100</span>
<ul start="1"><li>Correcting the permissions of the executable files.
</li></ul>
</li><li>[Revision #3077.187.30](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.30)
    <span class="cstm-style datetime">Tue 2013-03-19 17:09:17 +0100</span>
<ul start="1"><li>Bug#13009341 CRASH IN STR_TO_DATETIME AFTER MISBEHAVING "BLOB" VALUE COMPARISON
</li></ul>
</li><li>[Revision #3077.187.29](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.29)
    <span class="cstm-style datetime">Wed 2013-03-20 11:20:12 +0100</span>
<ul start="1"><li>Bug#16394084: LOOSE INDEX SCAN WITH QUOTED INT PREDICATE                RETURNS RANDOM DATA
</li></ul>
</li><li>[Revision #3077.187.28](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.28)
    <span class="cstm-style datetime">Tue 2013-03-19 15:08:19 +0100</span>
<ul start="1"><li>Bug#16359402 CRASH WITH AGGREGATES: ASSERTION FAILED: N &lt; M_SIZE
</li></ul>
</li><li>[Revision #3077.187.27](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.27)
    <span class="cstm-style datetime">Tue 2013-03-19 15:53:48 +0100</span>
<ul start="1"><li>Fix for Bug 16395495 - OLD FSF ADDRESS IN GPL HEADER
</li></ul>
</li><li>[Revision #3077.187.26](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.26) [merge]
    <span class="cstm-style datetime">Tue 2013-03-19 13:36:34 +0100</span>
<ul start="1"><li>Upmerging the changes for Bug 16395495 from 5.1
</li><li>[Revision #2661.844.64](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.64)
     <span class="cstm-style datetime">Tue 2013-03-19 13:29:12 +0100</span>
<ul start="1"><li>Bug 16395495 - OLD FSF ADDRESS IN GPL HEADER
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.25](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.25)
    <span class="cstm-style datetime">Mon 2013-03-18 17:20:30 +0200</span>
<ul start="1"><li>Fix Bug#16400412 UNNECESSARY DICT_UPDATE_STATISTICS DURING CONCURRENT UPDATES
</li></ul>
</li><li>[Revision #3077.187.24](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.24) [merge]
    <span class="cstm-style datetime">Tue 2013-03-19 05:35:30 +0100</span>
<ul start="1"><li>Upmerging the changes for Bug 16401147 from 5.1
</li><li>[Revision #2661.844.63](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.63)
     <span class="cstm-style datetime">Tue 2013-03-19 05:19:31 +0100</span>
<ul start="1"><li>Bug 16401147 - CRLF INSTEAD OF LF IN README
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.23](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.23)
    <span class="cstm-style datetime">Tue 2013-03-19 05:24:03 +0100</span>
<ul start="1"><li>Bug 16401147 - CRLF INSTEAD OF LF IN README
</li></ul>
</li><li>[Revision #3077.187.22](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.22) [merge]
    <span class="cstm-style datetime">Mon 2013-03-18 15:03:54 +0530</span>
<ul start="1"><li>merge from mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.844.62](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.62)
     <span class="cstm-style datetime">Mon 2013-03-18 15:01:16 +0530</span>
<ul start="1"><li>Bug#14771299 OUT-OF-BOUND READS WRITE IN MYSQLBINLOG
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.21](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.21)
    <span class="cstm-style datetime">Mon 2013-03-18 13:48:53 +0530</span>
<ul start="1"><li>Bug #16076289 : BACKPORT FIX FOR BUG #14786792 TO 5.5
</li></ul>
</li><li>[Revision #3077.187.20](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.20) [merge]
    <span class="cstm-style datetime">Mon 2013-03-18 12:46:06 +0530</span>
<ul start="1"><li>Merge of patch for bug#14685362 from mysql-5.1.
</li><li>[Revision #2661.844.61](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.61)
     <span class="cstm-style datetime">Mon 2013-03-18 12:44:38 +0530</span>
<ul start="1"><li>Bug#14685362 : MEMORY LEAKS IN MYSQL CLIENT IN   INTERACTIVE MODE
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.19](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.19) [merge]
    <span class="cstm-style datetime">Fri 2013-03-15 08:57:59 +0530</span>
<ul start="1"><li>Bug#16056813-MEMORY LEAK ON FILTERED SLAVE Null merge from mysql-5.1
</li><li>[Revision #2661.844.60](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.60)
     <span class="cstm-style datetime">Fri 2013-03-15 08:56:20 +0530</span>
<ul start="1"><li>Bug#16056813-MEMORY LEAK ON FILTERED SLAVE
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.18](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.18)
    <span class="cstm-style datetime">Thu 2013-03-14 15:33:25 +0100</span>
<ul start="1"><li>Bug#16359402 CRASH WITH AGGREGATES: ASSERTION FAILED: N &lt; M_SIZE
</li></ul>
</li><li>[Revision #3077.187.17](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.17) [merge]
    <span class="cstm-style datetime">Thu 2013-03-14 11:22:08 +0300</span>
<ul start="1"><li>5.1 -&gt; 5.5 merge
</li><li>[Revision #2661.844.59](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.59)
     <span class="cstm-style datetime">Thu 2013-03-14 11:11:17 +0300</span>
<ul start="1"><li>Bug#16075310 SERVER CRASH OR VALGRIND ERRORS IN ITEM_FUNC_GROUP_CONCAT::SETUP AND ::ADD Item_func_group_concat::copy_or_same() creates a copy of original object. It also creates a copy of ORDER structure because ORDER struct elements may be modified in find_order_in_list() called from Item_func_group_concat::setup(). As ORDER copy is created using memcpy, ORDER::next elements point to original ORDER structs. Thus find_order_in_list() called from EXECUTE stmt modifies ordinal ORDER item pointers so they point to runtime items, these items are freed after execution, so original ORDER structure becomes invalid. The fix is to properly update ORDER::next fields so that they point to new ORDER elements.
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.16](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.16) [merge]
    <span class="cstm-style datetime">Wed 2013-03-13 16:29:11 +0530</span>
<ul start="1"><li>BUG#14593883-REPLICATION BREAKS WHEN SET DATA TYPE                               COLUMNS ARE USED INSIDE A STORED PROCEDURE  Merging post-push fix from mysql-5.1
</li><li>[Revision #2661.844.58](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.58)
     <span class="cstm-style datetime">Wed 2013-03-13 16:24:35 +0530</span>
<ul start="1"><li>BUG#14593883-REPLICATION BREAKS WHEN SET DATA TYPE                               COLUMNS ARE USED INSIDE A STORED PROCEDURE
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.15](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.15)
    <span class="cstm-style datetime">Wed 2013-03-13 11:43:21 +0530</span>
<ul start="1"><li>Bug#16268289 LOCK_REC_VALIDATE_PAGE() MAY DEREFERENCE A POINTER TO A              FREED LOCK
</li></ul>
</li><li>[Revision #3077.187.14](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.14) [merge]
    <span class="cstm-style datetime">Wed 2013-03-13 09:43:50 +0530</span>
<ul start="1"><li>Bug#16084346: SSL_CONNECT_DEBUG.TEST FAILURE IN 5.1
</li><li>[Revision #2661.844.57](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.57)
     <span class="cstm-style datetime">Wed 2013-03-13 09:42:07 +0530</span>
</li></ul>
</li><li>[Revision #3077.187.13](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.13) [merge]
    <span class="cstm-style datetime">Tue 2013-03-12 22:44:32 +0530</span>
<ul start="1"><li>BUG#14593883-REPLICATION BREAKS WHEN SET DATA TYPE                               COLUMNS ARE USED INSIDE A STORED PROCEDURE                                      
</li><li>[Revision #2661.844.56](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.56)
     <span class="cstm-style datetime">Tue 2013-03-12 22:36:13 +0530</span>
<ul start="1"><li>BUG#14593883-REPLICATION BREAKS WHEN SET DATA TYPE                               COLUMNS ARE USED INSIDE A STORED PROCEDURE                                      
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.12](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.12)
    <span class="cstm-style datetime">Tue 2013-03-12 13:58:10 +0200</span>
<ul start="1"><li>Bug#16409715 ASSERT SYNC_THREAD_LEVELS_G(ARRAY, LEVEL - 1, TRUE), IBUF, FREE SPACE MANAGEMENT
</li></ul>
</li><li>[Revision #3077.187.11](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.11) [merge]
    <span class="cstm-style datetime">Tue 2013-03-12 13:57:02 +0200</span>
<ul start="1"><li>Merge mysql-5.1 to mysql-5.5.
</li><li>[Revision #2661.844.55](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.55)
     <span class="cstm-style datetime">Tue 2013-03-12 13:42:12 +0200</span>
<ul start="1"><li>Bug#16463505 PESSIMISTIC PAGE_ZIP_AVAILABLE() MAY CAUSE INFINITE PAGE SPLIT
</li></ul>
</li><li>[Revision #2661.844.54](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.54)
     <span class="cstm-style datetime">Tue 2013-03-12 13:37:00 +0200</span>
</li></ul>
</li><li>[Revision #3077.187.10](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.10)
    <span class="cstm-style datetime">Mon 2013-03-11 16:46:11 +0100</span>
<ul start="1"><li>Bug#11766815 INVALID SYSTEM CHECK TIME_T_UNSIGNED
</li></ul>
</li><li>[Revision #3077.187.9](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.9)
    <span class="cstm-style datetime">Mon 2013-03-11 12:03:26 +0530</span>
</li><li>[Revision #3077.187.8](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.8)
    <span class="cstm-style datetime">Fri 2013-03-08 14:55:41 +0530</span>
</li><li>[Revision #3077.187.7](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.7)
    <span class="cstm-style datetime">Thu 2013-03-07 14:44:35 +0530</span>
<ul start="1"><li>BUG#16069598 - SERVER CRASH BY NULL POINTER DEREFERENCING IN                 MEM_HEAP_CREATE_BLOCK() 
</li></ul>
</li><li>[Revision #3077.187.6](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.6)
    <span class="cstm-style datetime">Fri 2013-03-01 13:25:59 +0100</span>
<ul start="1"><li>Bug#11765489 CMAKE BUILD ON MAC OS X DOES NOT DETERMINE CPU TYPE
</li></ul>
</li><li>[Revision #3077.187.5](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.5)
    <span class="cstm-style datetime">Thu 2013-03-07 12:12:58 +0530</span>
<ul start="1"><li>Bug#16169063: SECURITY CONCERN BECAUSE OF INSUFFICIENT LOGGING
</li></ul>
</li><li>[Revision #3077.187.4](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.4)
    <span class="cstm-style datetime">Wed 2013-03-06 11:49:57 +0530</span>
<ul start="1"><li>Bug #16133801 UNEXPLAINABLE INNODB UNIQUE INDEX LOCKS ON DELETE + INSERT WITH SAME VALUES
</li></ul>
</li><li>[Revision #3077.187.3](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.3) [merge]
    <span class="cstm-style datetime">Wed 2013-03-06 06:52:18 +0100</span>
<ul start="1"><li>NULL Merge for release 5.1.69
</li><li>[Revision #2661.844.53](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.53)
     <span class="cstm-style datetime">Tue 2013-03-05 16:09:54 +0100</span>
<ul start="1"><li>Raise version number after cloning 5.1.69
</li></ul>
</li></ul>
</li><li>[Revision #3077.187.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.2)
    <span class="cstm-style datetime">Tue 2013-03-05 10:47:49 -0500</span>
<ul start="1"><li>Bug#16068056 INNODB CALLS BUF_VALIDATE() TOO OFTEN WITH UNIV_DEBUG
</li></ul>
</li><li>[Revision #3077.187.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.187.1)
    <span class="cstm-style datetime">Tue 2013-03-05 12:19:07 +0100</span>
<ul start="1"><li>Raise version number after cloning 5.5.31
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.500](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.500) [merge]
   <span class="cstm-style datetime">Tue 2013-07-16 19:03:06 +0200</span>
<ul start="1"><li>5.3 merge
</li><li>[Revision #2502.567.114](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.114) [merge]
    <span class="cstm-style datetime">Mon 2013-07-15 18:32:25 +0200</span>
<ul start="1"><li>5.2 merge
</li><li>[Revision #2502.566.51](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.51)
     <span class="cstm-style datetime">Tue 2013-07-09 22:24:57 +0200</span>
<ul start="1"><li>[MDEV-4409](https://jira.mariadb.org/browse/MDEV-4409) - Fix deadlock in MySQL key cache code, that can happen if there is a key cache resize running in parallel with an update.
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.499](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.499) [merge]
   <span class="cstm-style datetime">Tue 2013-07-16 15:59:30 +0400</span>
<ul start="1"><li>Automatic merge
</li><li>[Revision #3334.36.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.36.1)
    <span class="cstm-style datetime">Tue 2013-07-16 15:57:27 +0400</span>
<ul start="1"><li>[MDEV-4782](https://jira.mariadb.org/browse/MDEV-4782): Valgrind warnings (Conditional jump or move depends on uninitialised value) with InnoDB, semijoin - in sub_select(): don't call table-&gt;file-&gt;position() when reading the first record   produced an error.
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.498](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.498)
   <span class="cstm-style datetime">Tue 2013-07-16 17:26:25 +0400</span>
<ul start="1"><li>Update test results after the last cset.
</li></ul>
</li><li>[Revision #3334.1.497](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.497)
   <span class="cstm-style datetime">Tue 2013-07-16 10:56:42 +0400</span>
<ul start="1"><li>[MDEV-4778](https://jira.mariadb.org/browse/MDEV-4778): Incorrect results from Aria/MyISAM SELECT using index with prefix length on TEXT column Backport the fix olav.sandstaa@sun.com-20101102184747-qfuntqwj021imy9r: "Fix for Bug#52660 Perf. regr. using ICP for MyISAM on range queries on an index containing TEXT"  (together with further fixes in that code) into MyISAM and Aria.
</li></ul>
</li><li>[Revision #3334.1.496](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.496)
   <span class="cstm-style datetime">Tue 2013-07-16 09:22:17 +0400</span>
<ul start="1"><li>[MDEV-4173](https://jira.mariadb.org/browse/MDEV-4173): Wrong result (extra row) with semijoin=on, joins in outer query, LEFT JOIN in the subquery Apply the patch from Patryk Pomykalski: - create_internal_tmp_table_from_heap() will now return information whether   the last row that we tried to write was a duplicate row. (mysql-5.6 also has this change)
</li></ul>
</li><li>[Revision #3334.1.495](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.495)
   <span class="cstm-style datetime">Mon 2013-07-15 18:51:52 +0400</span>
<ul start="1"><li>[MDEV-4536](https://jira.mariadb.org/browse/MDEV-4536), [MDEV-4042](https://jira.mariadb.org/browse/MDEV-4042) - Make JOIN::cleanup(true) also work correctly when the query is KILLed   after join optimization was started but before a query plan was produced
</li></ul>
</li><li>[Revision #3334.1.494](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.494)
   <span class="cstm-style datetime">Thu 2013-07-11 19:27:39 +0400</span>
<ul start="1"><li>[MDEV-4042](https://jira.mariadb.org/browse/MDEV-4042): Assertion `table-&gt;key_read == 0' fails in close_thread_table on EXPLAIN [MDEV-4536](https://jira.mariadb.org/browse/MDEV-4536): ...sql/sql_base.cc:1598: bool close_thread_table(THD*, TABLE<strong>): Assertion - Make JOIN::cleanup(full=true) always free join optimization tabs.</strong>
</li></ul>
</li><li>[Revision #3334.1.493](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.493)
   <span class="cstm-style datetime">Thu 2013-07-11 15:12:50 +0400</span>
<ul start="1"><li>[MDEV-4556](https://jira.mariadb.org/browse/MDEV-4556) Server crashes in SEL_ARG::rb_insert with index_merge+index_merge_sort_union, FORCE INDEX - merge_same_index_scans() may put the same SEL_ARG tree in multiple result plans.   make it call incr_refs() on the SEL_ARG trees that it does key_or() on, because    key_or(sel_arg_tree_1, sel_arg_tree_2) call may invalidate SEL_ARG trees pointed    by sel_arg_tree_1 and sel_arg_tree_2.
</li></ul>
</li><li>[Revision #3334.1.492](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.492) [merge]
   <span class="cstm-style datetime">Wed 2013-07-10 02:05:06 +0400</span>
<ul start="1"><li>Merge from 5.3
</li><li>[Revision #2502.567.113](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.113) [merge]
    <span class="cstm-style datetime">Tue 2013-07-09 11:02:56 +0400</span>
<ul start="1"><li>Merge from 5.2
</li><li>[Revision #2502.566.50](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.50) [merge]
     <span class="cstm-style datetime">Tue 2013-07-09 10:54:47 +0400</span>
<ul start="1"><li>Merge from 5.1
</li><li>[Revision #2502.565.51](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.51)
      <span class="cstm-style datetime">Sat 2013-07-06 15:28:11 +0200</span>
<ul start="1"><li>Bug #69682 - mysqld crashes after uninstall of plugin with "first" status var
</li></ul>
</li><li>[Revision #2502.565.50](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.50)
      <span class="cstm-style datetime">Fri 2013-05-24 17:35:30 +0200</span>
<ul start="1"><li>[MDEV-4575](https://jira.mariadb.org/browse/MDEV-4575) MySQL client doesn't strip off 5.5.5- prefix while connecting to 10.x server
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #2502.567.112](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.112)
    <span class="cstm-style datetime">Fri 2013-07-05 20:45:42 +0200</span>
<ul start="1"><li>[MDEV-4610](https://jira.mariadb.org/browse/MDEV-4610) SQL query crashes MariaDB with derived_with_keys [MDEV-4643](https://jira.mariadb.org/browse/MDEV-4643) MariaDB crashes consistently when trying a SELECT on VIEW with a UNION and an additional JOIN in SELECT
</li></ul>
</li><li>[Revision #2502.567.111](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.111)
    <span class="cstm-style datetime">Fri 2013-07-05 17:54:25 +0200</span>
<ul start="1"><li>[MDEV-4665](https://jira.mariadb.org/browse/MDEV-4665) crash when referencing missing function in a subquery
</li></ul>
</li><li>[Revision #2502.567.110](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.110)
    <span class="cstm-style datetime">Fri 2013-07-05 16:02:02 +0200</span>
<ul start="1"><li>[MDEV-4257](https://jira.mariadb.org/browse/MDEV-4257) Assertion `!table || (!table-&gt;read_set || bitmap_is_set(table-&gt;read_set, field_index))' fails on FROM subquery with fulltext search, derived_merge=on
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.491](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.491) [merge]
   <span class="cstm-style datetime">Mon 2013-07-08 16:49:42 +0400</span>
<ul start="1"><li>Merging from 5.3
</li><li>[Revision #2502.567.109](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.109)
    <span class="cstm-style datetime">Wed 2013-07-03 09:46:20 +0200</span>
<ul start="1"><li>[MDEV-4667](https://jira.mariadb.org/browse/MDEV-4667) DATE('string') incompability between mysql and mariadb
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.490](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.490)
   <span class="cstm-style datetime">Thu 2013-07-04 18:37:55 +0300</span>
<ul start="1"><li>[MDEV-4752](https://jira.mariadb.org/browse/MDEV-4752): Segfault during parsing of illegal query
</li></ul>
</li><li>[Revision #3334.1.489](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.489)
   <span class="cstm-style datetime">Mon 2013-07-01 17:54:24 +0200</span>
<ul start="1"><li>[MDEV-4718](https://jira.mariadb.org/browse/MDEV-4718) Test "outfile_loaddata" fails on bigendian arches (ppc64)
</li></ul>
</li><li>[Revision #3334.1.488](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.488)
   <span class="cstm-style datetime">Mon 2013-07-01 12:03:10 +0200</span>
<ul start="1"><li>[MDEV-4670](https://jira.mariadb.org/browse/MDEV-4670) THD::awake bug with my_sleep call
</li></ul>
</li><li>[Revision #3334.1.487](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.487)
   <span class="cstm-style datetime">Mon 2013-07-01 12:02:44 +0200</span>
<ul start="1"><li>[MDEV-4683](https://jira.mariadb.org/browse/MDEV-4683) query start_time not reset when going to sleep
</li></ul>
</li><li>[Revision #3334.1.486](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.486) [merge]
   <span class="cstm-style datetime">Fri 2013-06-28 16:27:22 +0400</span>
<ul start="1"><li>Merge
</li><li>[Revision #2502.567.108](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.108)
    <span class="cstm-style datetime">Fri 2013-06-28 16:25:06 +0400</span>
<ul start="1"><li>A clean-up for [MDEV-4634](https://jira.mariadb.org/browse/MDEV-4634)
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.485](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.485) [merge]
   <span class="cstm-style datetime">Fri 2013-06-28 15:20:40 +0400</span>
<ul start="1"><li>Merge from 5.3
</li><li>[Revision #2502.567.107](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.107)
    <span class="cstm-style datetime">Fri 2013-06-28 12:00:25 +0400</span>
<ul start="1"><li>[MDEV-4634](https://jira.mariadb.org/browse/MDEV-4634) Crash in CONVERT_TZ Item_func_min_max::get_date() did not check the returned value against the fuzzy_date flags, so it could return a bad value to the caller that expects a good date (e.h. CONVERT_TZ).
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.484](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.484)
   <span class="cstm-style datetime">Thu 2013-06-27 14:19:04 +0200</span>
<ul start="1"><li>[MDEV-4720](https://jira.mariadb.org/browse/MDEV-4720) : fix my_context.h for use with x32 ABI. Do not use x64  assembler implementation in  x32.
</li></ul>
</li><li>[Revision #3334.1.483](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.483)
   <span class="cstm-style datetime">Sat 2013-06-22 14:02:03 +0200</span>
<ul start="1"><li>[MDEV-4685](https://jira.mariadb.org/browse/MDEV-4685) Compile error on LFS
</li></ul>
</li><li>[Revision #3334.1.482](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.482) [merge]
   <span class="cstm-style datetime">Tue 2013-06-18 13:14:46 +0400</span>
<ul start="1"><li>Merging [MDEV-4635](https://jira.mariadb.org/browse/MDEV-4635) from 5.3.
</li><li>[Revision #2502.567.106](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.106)
    <span class="cstm-style datetime">Mon 2013-06-17 19:25:55 +0400</span>
<ul start="1"><li>[MDEV-4635](https://jira.mariadb.org/browse/MDEV-4635) Crash in UNIX_TIMESTAMP(STR_TO_DATE('2020','%Y'))
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.481](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.481)
   <span class="cstm-style datetime">Mon 2013-06-17 19:18:14 +0200</span>
<ul start="1"><li>[MDEV-4503](https://jira.mariadb.org/browse/MDEV-4503) : Installation fails if TEMP directory contains "" subdirectory.
</li></ul>
</li><li>[Revision #3334.1.480](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.480)
   <span class="cstm-style datetime">Mon 2013-06-17 17:58:53 +0200</span>
<ul start="1"><li>unit test case for [MDEV-4576](https://jira.mariadb.org/browse/MDEV-4576)
</li></ul>
</li><li>[Revision #3334.1.479](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.479)
   <span class="cstm-style datetime">Sun 2013-06-16 22:13:26 +0200</span>
<ul start="1"><li>[MDEV-4576](https://jira.mariadb.org/browse/MDEV-4576) : Aria storage engine's  temporary files might not be deleted (Errcode : 13) See also MySQL Bug #39750  and similar ones.
</li></ul>
</li><li>[Revision #3334.1.478](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.478)
   <span class="cstm-style datetime">Sat 2013-06-15 14:22:03 +0200</span>
<ul start="1"><li>[MDEV-4601](https://jira.mariadb.org/browse/MDEV-4601) : Allow MariaDB to be build without non-blocking client.
</li></ul>
</li><li>[Revision #3334.1.477](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.477) [merge]
   <span class="cstm-style datetime">Mon 2013-06-17 20:33:36 +0300</span>
<ul start="1"><li>5.3 -&gt; 5.5 Merge
</li><li>[Revision #2502.567.105](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.105)
    <span class="cstm-style datetime">Mon 2013-06-17 17:04:51 +0400</span>
<ul start="1"><li>[MDEV-4651](https://jira.mariadb.org/browse/MDEV-4651) Crash in my_decimal2decimal in a ORDER BY query
</li></ul>
</li><li>[Revision #2502.567.104](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.104)
    <span class="cstm-style datetime">Thu 2013-06-06 23:33:40 +0300</span>
<ul start="1"><li>[MDEV-4593](https://jira.mariadb.org/browse/MDEV-4593): p_s: crash in simplify_joins with delete using subselect from view
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.476](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.476)
   <span class="cstm-style datetime">Sat 2013-06-15 16:02:43 +0200</span>
<ul start="1"><li>[MDEV-4466](https://jira.mariadb.org/browse/MDEV-4466) Partitioned Aria table created by a previous version is recognized as TEST_SQL_DISCOVERY
</li></ul>
</li><li>[Revision #3334.1.475](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.475)
   <span class="cstm-style datetime">Fri 2013-06-14 14:04:58 +0200</span>
<ul start="1"><li>[MDEV-4006](https://jira.mariadb.org/browse/MDEV-4006) mysql_plugin.1 is removed from source which is not necessary
</li></ul>
</li><li>[Revision #3334.1.474](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.474)
   <span class="cstm-style datetime">Thu 2013-06-13 20:19:32 +0200</span>
<ul start="1"><li>[MDEV-4578](https://jira.mariadb.org/browse/MDEV-4578) information_schema.processlist reports incorrect value for Time (2147483647)
</li></ul>
</li><li>[Revision #3334.1.473](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.473)
   <span class="cstm-style datetime">Thu 2013-06-13 20:19:11 +0200</span>
<ul start="1"><li>[MDEV-4529](https://jira.mariadb.org/browse/MDEV-4529) Assertion `tmp-&gt;state == 4' fails on mix of INSTALL SONAME / UNINSTALL PLUGIN
</li></ul>
</li><li>[Revision #3334.1.472](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.472)
   <span class="cstm-style datetime">Thu 2013-06-13 20:18:40 +0200</span>
<ul start="1"><li>[MDEV-4519](https://jira.mariadb.org/browse/MDEV-4519) SHOW EVENTS and SHOW PROCEDURE STATUS truncate long user names
</li></ul>
</li><li>[Revision #3334.1.471](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.471)
   <span class="cstm-style datetime">Thu 2013-06-13 15:33:02 +0200</span>
<ul start="1"><li>[MDEV-4515](https://jira.mariadb.org/browse/MDEV-4515) Long user names are truncated to 48 symbols in error messages
</li></ul>
</li><li>[Revision #3334.1.470](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.470)
   <span class="cstm-style datetime">Thu 2013-06-13 15:13:13 +0200</span>
<ul start="1"><li>[MDEV-4444](https://jira.mariadb.org/browse/MDEV-4444) Server crashes with "safe_mutex: Trying to destroy a mutex share-&gt;mutex that was locked" on attempt to recover an archive table
</li></ul>
</li><li>[Revision #3334.1.469](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.469)
   <span class="cstm-style datetime">Thu 2013-06-13 14:32:57 +0200</span>
<ul start="1"><li>[MDEV-703](https://jira.mariadb.org/browse/MDEV-703) [Bug #870310](https://bugs.launchpad.net/bugs/870310) - killall -9 in init-script
</li></ul>
</li><li>[Revision #3334.1.468](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.468)
   <span class="cstm-style datetime">Thu 2013-06-13 14:14:47 +0200</span>
<ul start="1"><li>[MDEV-4573](https://jira.mariadb.org/browse/MDEV-4573) UNINSTALL PLUGIN misleading error message for non-dynamic plugins
</li></ul>
</li><li>[Revision #3334.1.467](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.467)
   <span class="cstm-style datetime">Thu 2013-06-13 00:13:23 +0200</span>
<ul start="1"><li>[MDEV-4614](https://jira.mariadb.org/browse/MDEV-4614) Man pages fixes
</li></ul>
</li><li>[Revision #3334.1.466](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.466)
   <span class="cstm-style datetime">Wed 2013-06-12 22:12:09 +0200</span>
<ul start="1"><li>[MDEV-4604](https://jira.mariadb.org/browse/MDEV-4604) Wrong server status when sending out parameters
</li></ul>
</li><li>[Revision #3334.1.465](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.465)
   <span class="cstm-style datetime">Wed 2013-06-12 20:38:22 +0200</span>
<ul start="1"><li>[MDEV-4509](https://jira.mariadb.org/browse/MDEV-4509) mysql init script should accept arguments
</li></ul>
</li><li>[Revision #3334.1.464](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.464)
   <span class="cstm-style datetime">Wed 2013-06-12 20:29:19 +0200</span>
<ul start="1"><li>[MDEV-4422](https://jira.mariadb.org/browse/MDEV-4422) SHOW PROCESSLIST reference to THD::db not protected against simultaneous updates
</li></ul>
</li><li>[Revision #3334.1.463](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.463)
   <span class="cstm-style datetime">Tue 2013-06-11 12:53:35 +0200</span>
<ul start="1"><li>[MDEV-4636](https://jira.mariadb.org/browse/MDEV-4636) use mysql_cleartext_plugin from auth_pam
</li></ul>
</li><li>[Revision #3334.1.462](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.462)
   <span class="cstm-style datetime">Tue 2013-06-11 11:11:05 +0200</span>
<ul start="1"><li>[MDEV-4574](https://jira.mariadb.org/browse/MDEV-4574) Missing connection option MYSQL_ENABLE_CLEARTEXT_PLUGIN
</li></ul>
</li><li>[Revision #3334.1.461](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.461)
   <span class="cstm-style datetime">Mon 2013-06-10 21:45:30 +0200</span>
<ul start="1"><li>[MDEV-4297](https://jira.mariadb.org/browse/MDEV-4297) <code class="fixed" style="white-space:pre-wrap">mysql --binary-mode</code>
</li></ul>
</li><li>[Revision #3334.1.460](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.460)
   <span class="cstm-style datetime">Wed 2013-06-12 05:09:28 +0400</span>
<ul start="1"><li>[MDEV-4629](https://jira.mariadb.org/browse/MDEV-4629) MTR tests main.variables and some of sys_vars.* fail on 32-bit builds
</li></ul>
</li><li>[Revision #3334.1.459](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.459)
   <span class="cstm-style datetime">Tue 2013-06-11 13:49:43 +0300</span>
<ul start="1"><li>Fixed tests that failed on 32 bit because of my earlier fixes of 32 bit limits.
</li></ul>
</li><li>[Revision #3334.1.458](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.458)
   <span class="cstm-style datetime">Fri 2013-06-07 15:35:13 +0200</span>
<ul start="1"><li>[MDEV-4468](https://jira.mariadb.org/browse/MDEV-4468) Assertion `error != 0' fails or timeout occurs on select from a FEDERATED table which points at a non-existent table
</li></ul>
</li><li>[Revision #3334.1.457](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.457)
   <span class="cstm-style datetime">Fri 2013-06-07 15:34:59 +0200</span>
<ul start="1"><li>[MDEV-4480](https://jira.mariadb.org/browse/MDEV-4480) Assertion `inited == NONE' fails on closing a connection with open handler on temporary table
</li></ul>
</li><li>[Revision #3334.1.456](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.456)
   <span class="cstm-style datetime">Fri 2013-06-07 10:02:50 +0200</span>
<ul start="1"><li>[MDEV-4564](https://jira.mariadb.org/browse/MDEV-4564) ALTER on a temporary table generates an audit event
</li></ul>
</li><li>[Revision #3334.1.455](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.455)
   <span class="cstm-style datetime">Sun 2013-06-09 13:26:10 +0300</span>
<ul start="1"><li>- Added -Wno-uninitialized to avoid warnings in release builds   (uninitalized variables are detected by DBUG builds) - Fixed wrong declaration which cased compile failure on 32 bit
</li></ul>
</li><li>[Revision #3334.1.454](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.454)
   <span class="cstm-style datetime">Thu 2013-06-06 15:14:23 +0300</span>
<ul start="1"><li>Fixed some cache variables that could be set to higher value than what the code supported (size_t) Fixed some cases that didn't work with &gt; 4G buffers. Fixed compiler warnings
</li></ul>
</li><li>[Revision #3334.1.453](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.453)
   <span class="cstm-style datetime">Wed 2013-06-05 23:53:35 +0300</span>
<ul start="1"><li>-Run test suite with smaller aria keybuffer size (to make it possible to run more tests in parallel) -Added test and extra code to ensure we don't leave keyread on for a handler table. -Create on disk temporary files always with long data pointers if SQL_SMALL_RESULT is not used. This ensures that we can handle temporary files bigger than 4G.
</li></ul>
</li><li>[Revision #3334.1.452](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.452)
   <span class="cstm-style datetime">Sat 2013-06-01 21:33:26 +0200</span>
<ul start="1"><li>Fix a compile warning on NetBSD
</li></ul>
</li><li>[Revision #3334.1.451](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.451)
   <span class="cstm-style datetime">Sat 2013-06-01 21:30:33 +0200</span>
<ul start="1"><li>[MDEV-4607](https://jira.mariadb.org/browse/MDEV-4607) : libreadline-related  compilation problems on NetBSD.
</li></ul>
</li><li>[Revision #3334.1.450](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.450)
   <span class="cstm-style datetime">Thu 2013-05-30 08:23:49 +0300</span>
<ul start="1"><li>[MDEV-4520](https://jira.mariadb.org/browse/MDEV-4520): Assertion `0' fails in Query_cache::end_of_result on concurrent drop event and event executio
</li></ul>
</li><li>[Revision #3334.1.449](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.449)
   <span class="cstm-style datetime">Tue 2013-05-28 21:25:59 +0200</span>
<ul start="1"><li>followup for revision 3751 "centos5 gcc 4.1 asm bug" remove the workaround from cmake/os/FreeBSD.cmake
</li></ul>
</li><li>[Revision #3334.1.448](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.448)
   <span class="cstm-style datetime">Thu 2013-05-23 17:05:31 +0300</span>
<ul start="1"><li>[MDEV-4520](https://jira.mariadb.org/browse/MDEV-4520): Assertion `0' fails in Query_cache::end_of_result on concurrent drop event and event execution
</li></ul>
</li><li>[Revision #3334.1.447](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.447)
   <span class="cstm-style datetime">Wed 2013-05-22 16:44:44 +0200</span>
<ul start="1"><li>[MDEV-4548](https://jira.mariadb.org/browse/MDEV-4548) - compile sphinx.so/dll and include into packages
</li></ul>
</li><li>[Revision #3334.1.446](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.446)
   <span class="cstm-style datetime">Mon 2013-05-27 16:35:42 +0200</span>
<ul start="1"><li>[MDEV-4553](https://jira.mariadb.org/browse/MDEV-4553) - Fixes for compilation under NetBSD.
</li></ul>
</li><li>[Revision #3334.1.445](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.445)
   <span class="cstm-style datetime">Fri 2013-05-24 14:33:04 +0200</span>
<ul start="1"><li>[MDEV-4516](https://jira.mariadb.org/browse/MDEV-4516) SELECT from I_S.QUERY_CACHE_INFO produces ER_UNKNOWN_ERROR when query cache size is 0
</li></ul>
</li></ul>
- [Revision #3411](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3411)
  <span class="cstm-style datetime">Wed 2013-08-07 00:17:16 +0300</span>
<ul start="1"><li>Merged FreeBSD compatibility changes (up to revision 3893 in  lp:codership/codership-mysql/5.5-23)
</li></ul>
- [Revision #3410](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3410)
  <span class="cstm-style datetime">Mon 2013-08-05 18:01:05 +0300</span>
<ul start="1"><li>References [Bug #1208493](https://bugs.launchpad.net/bugs/1208493) [MDEV-4830](https://jira.mariadb.org/browse/MDEV-4830) Enabling slave applier thread to send COND_thread_count
</li></ul>
- [Revision #3409](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3409)
  <span class="cstm-style datetime">Thu 2013-06-20 21:51:11 +0300</span>
<ul start="1"><li>References [Bug #1193079](https://bugs.launchpad.net/bugs/1193079) - bumped wsrep version to 7.5
</li></ul>
- [Revision #3408](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3408)
  <span class="cstm-style datetime">Wed 2013-06-19 10:35:40 +0300</span>
<ul start="1"><li>References [Bug #1191778](https://bugs.launchpad.net/bugs/1191778) - merged xtrabackup SST fixes from PXC
</li></ul>
- [Revision #3407](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3407)
  <span class="cstm-style datetime">Sun 2013-06-16 20:38:02 +0300</span>
<ul start="1"><li>References [Bug #1134892](https://bugs.launchpad.net/bugs/1134892) - WSREP_DEBUG_PRINT was left on by mistake
</li></ul>
- [Revision #3406](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3406)
  <span class="cstm-style datetime">Sat 2013-06-15 16:16:38 +0300</span>
<ul start="1"><li>References [Bug #1087368](https://bugs.launchpad.net/bugs/1087368) - merged fix from wsrep-5.5 branch.      Note this is compatible only with new wsrep provider #23 libraries, which understand 'bootstrap' address in connecting.
</li></ul>
- [Revision #3405](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3405)
  <span class="cstm-style datetime">Sat 2013-06-15 16:15:45 +0300</span>
<ul start="1"><li>References: [MDEV-4572](https://jira.mariadb.org/browse/MDEV-4572) - merge with lp:codership-mysql/5.5-23 revisions 3874..3878
</li></ul>
- [Revision #3404](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3404)
  <span class="cstm-style datetime">Sat 2013-06-15 16:15:17 +0300</span>
<ul start="1"><li>References [Bug #1108035](https://bugs.launchpad.net/bugs/1108035) - merged fix from [http://bazaar.launchpad.net/~percona-core/percona-xtradb-cluster/release-5.5.31/revision/394](http://bazaar.launchpad.net/~percona-core/percona-xtradb-cluster/release-5.5.31/revision/394)
</li></ul>
- [Revision #3403](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3403)
  <span class="cstm-style datetime">Fri 2013-06-14 22:01:18 +0500</span>
<ul start="1"><li>[MDEV-4656](https://jira.mariadb.org/browse/MDEV-4656) MariaDB-Galera deb packages cannot be built, expected files are missing.
</li></ul>
- [Revision #3402](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3402)
  <span class="cstm-style datetime">Thu 2013-06-13 10:22:37 +0300</span>
<ul start="1"><li>References: [Bug #1182441](https://bugs.launchpad.net/bugs/1182441) - merged fix from revision: [http://bazaar.launchpad.net/~percona-core/percona-xtradb-cluster/release-5.5.31/revision/416](http://bazaar.launchpad.net/~percona-core/percona-xtradb-cluster/release-5.5.31/revision/416)
</li></ul>
- [Revision #3401](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3401)
  <span class="cstm-style datetime">Thu 2013-06-13 09:55:28 +0300</span>
<ul start="1"><li>References [Bug #1169326](https://bugs.launchpad.net/bugs/1169326) - merged fix from LP wsrep-5.5-23 Now at revision 3874 in  lp:codership/codership-mysql/5.5-23
</li></ul>
- [Revision #3400](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3400)
  <span class="cstm-style datetime">Thu 2013-06-13 09:49:48 +0300</span>
<ul start="1"><li>References: [Bug #1134892](https://bugs.launchpad.net/bugs/1134892) [MDEV-4624](https://jira.mariadb.org/browse/MDEV-4624) - merged fix from LP wsrep-5.5-23
</li></ul>
- [Revision #3399](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3399)
  <span class="cstm-style datetime">Thu 2013-06-13 09:44:34 +0300</span>
<ul start="1"><li>References: [Bug #1187526](https://bugs.launchpad.net/bugs/1187526) - merged fix from wsrep-5.5-23
</li></ul>
- [Revision #3398](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3398)
  <span class="cstm-style datetime">Wed 2013-06-12 17:07:48 +0500</span>
<ul start="1"><li>[MDEV-4600](https://jira.mariadb.org/browse/MDEV-4600) mariadb-galera-server-5.5 on ubuntu has no dependency to galera while debian has.         dependency on galera added to the Ubuntu packaging script.
</li></ul>
- [Revision #3397](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3397)
  <span class="cstm-style datetime">Mon 2013-05-27 23:03:08 +0300</span>
<ul start="1"><li>References: [MDEV-3924](https://jira.mariadb.org/browse/MDEV-3924) [Bug #1088267](https://bugs.launchpad.net/bugs/1088267) - merged fix from lp:codership-mysql
</li></ul>
- [Revision #3396](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3396)
  <span class="cstm-style datetime">Mon 2013-05-27 22:51:22 +0300</span>
<ul start="1"><li>References [Bug #1012138](https://bugs.launchpad.net/bugs/1012138) - merged fix from lp:codership-mysql
</li></ul>
- [Revision #3395](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3395) [merge]
  <span class="cstm-style datetime">Sun 2013-05-26 11:26:58 +0300</span>
<ul start="1"><li>References: [MDEV-4572](https://jira.mariadb.org/browse/MDEV-4572) - merge with [mariaDB 5.5.31](/kb/en/mariadb-5531-release-notes/) bzr merge lp:maria/5.5 -rtag:mariadb-5.5.31
</li><li>[Revision #3334.1.444](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.444)
   <span class="cstm-style datetime">Tue 2013-05-21 18:56:35 +0200</span>
<ul start="1"><li>fix for compiled-in FederatedX
</li></ul>
</li><li>[Revision #3334.1.443](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.443)
   <span class="cstm-style datetime">Tue 2013-05-21 13:03:37 +0200</span>
<ul start="1"><li>[MDEV-388](https://jira.mariadb.org/browse/MDEV-388) Creating a federated table with a non-existing server returns a random error code (part 2)
</li></ul>
</li><li>[Revision #3334.1.442](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.442) [merge]
   <span class="cstm-style datetime">Tue 2013-05-21 09:43:34 +0200</span>
<ul start="1"><li>5.3 merge
</li><li>[Revision #2502.567.103](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.103)
    <span class="cstm-style datetime">Tue 2013-05-21 09:42:10 +0200</span>
<ul start="1"><li>fixes for buildbot
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.441](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.441)
   <span class="cstm-style datetime">Mon 2013-05-20 23:58:44 +0200</span>
<ul start="1"><li>[MDEV-388](https://jira.mariadb.org/browse/MDEV-388) Creating a federated table with a non-existing server returns a random error code
</li></ul>
</li><li>[Revision #3334.1.440](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.440)
   <span class="cstm-style datetime">Mon 2013-05-20 13:41:03 +0200</span>
<ul start="1"><li>increase MAX_HA (number of simultaneously installed storage engines) to 64
</li></ul>
</li><li>[Revision #3334.1.439](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.439) [merge]
   <span class="cstm-style datetime">Mon 2013-05-20 12:36:30 +0200</span>
<ul start="1"><li>5.3 merge. change maria.distinct to use a function that doesn't require ssl-enabled  builds
</li><li>[Revision #2502.567.102](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.102) [merge]
    <span class="cstm-style datetime">Mon 2013-05-20 11:13:07 +0200</span>
<ul start="1"><li>5.2 merge
</li><li>[Revision #2502.566.49](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.49) [merge]
     <span class="cstm-style datetime">Mon 2013-05-20 10:53:04 +0200</span>
<ul start="1"><li>5.1 merge
</li><li>[Revision #2502.565.49](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.49)
      <span class="cstm-style datetime">Sat 2013-05-11 20:23:57 +0300</span>
<ul start="1"><li>Fixed compiler failure on solaris
</li></ul>
</li><li>[Revision #2502.565.48](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.48)
      <span class="cstm-style datetime">Sat 2013-05-11 18:57:06 +0300</span>
<ul start="1"><li>Fixed compiler warning
</li></ul>
</li><li>[Revision #2502.565.47](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.47)
      <span class="cstm-style datetime">Sat 2013-05-11 15:55:11 +0300</span>
<ul start="1"><li>[MDEV-4280](https://jira.mariadb.org/browse/MDEV-4280): Assertion `empty_size == empty_size_on_page' failure in ma_blockrec.c or ER_NOT_KEYFILE on query with DISTINCT and GROUP BY This could happen when using Aria for internal temporary files (default case) and using DISTINCT. _ma_scan_restore_block_record() didn't work correctly if there was rows inserted, updated or deleted on the handler between calls to _ma_scan_remember_block_record() and _ma_scan_restore_block_record(). The effect was that some DISTINCT queries that used remove_dup_with_compare() could fail.
</li></ul>
</li><li>[Revision #2502.565.46](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.46)
      <span class="cstm-style datetime">Tue 2013-04-09 09:58:51 +0300</span>
<ul start="1"><li>[MDEV-4326](https://jira.mariadb.org/browse/MDEV-4326) fix.
</li></ul>
</li></ul>
</li><li>[Revision #2502.566.48](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.48)
     <span class="cstm-style datetime">Sun 2013-05-19 16:38:56 +0200</span>
<ul start="1"><li>[MDEV-4544](https://jira.mariadb.org/browse/MDEV-4544) - update MSI to include HeidiSQL 8.0
</li></ul>
</li><li>[Revision #2502.566.47](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.47)
     <span class="cstm-style datetime">Sun 2013-05-19 16:22:33 +0200</span>
<ul start="1"><li>Fix cpack error - safe_process.pl does not exist anymore.
</li></ul>
</li><li>[Revision #2502.566.46](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.46)
     <span class="cstm-style datetime">Wed 2013-05-08 14:32:32 +0200</span>
<ul start="1"><li>[MDEV-4462](https://jira.mariadb.org/browse/MDEV-4462) mysqld gets SIGFPE when mysql.user table is empty
</li></ul>
</li></ul>
</li><li>[Revision #2502.567.101](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.101)
    <span class="cstm-style datetime">Fri 2013-05-03 16:07:13 +0300</span>
<ul start="1"><li>[MDEV-4290](https://jira.mariadb.org/browse/MDEV-4290): Fix agregate function resolution in derived tables (no name resolution over a derived table border)
</li></ul>
</li><li>[Revision #2502.567.100](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.100) [merge]
    <span class="cstm-style datetime">Sun 2013-05-05 05:32:55 +0400</span>
<ul start="1"><li>Merge
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.438](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.438)
   <span class="cstm-style datetime">Sun 2013-05-19 17:42:30 +0200</span>
<ul start="1"><li>remove start menu shortcut to upgrade wizard
</li></ul>
</li><li>[Revision #3334.1.437](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.437)
   <span class="cstm-style datetime">Sun 2013-05-19 17:41:22 +0200</span>
<ul start="1"><li>[MDEV-4544](https://jira.mariadb.org/browse/MDEV-4544) : Update MSI installer to use latest HeidiSQL 8.0
</li></ul>
</li><li>[Revision #3334.1.436](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.436)
   <span class="cstm-style datetime">Fri 2013-05-17 10:16:56 +0400</span>
<ul start="1"><li>Bug#[MDEV-4518](https://jira.mariadb.org/browse/MDEV-4518) Server crashes in is_white_space when it's run with query cache, charset ucs2 and collation ucs2_unicode_ci
</li></ul>
</li><li>[Revision #3334.1.435](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.435)
   <span class="cstm-style datetime">Wed 2013-05-15 16:28:12 +0300</span>
<ul start="1"><li>- Solaris fixes:   - Fixed that wait_timeout_func and wait_timeout tests works on solaris   - We have to compile without NO_ALARM on Solaris as Solaris doesn't support timeouts on sockets with setsockopt(.. SO_RCVTIMEO).   - Fixed that compile-solaris-amd64-debug works (before that we got a wrong ELF class: ELFCLASS64 on linkage) - Fixed some compiler warnings - Fixed some failing tests
</li></ul>
</li><li>[Revision #3334.1.434](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.434)
   <span class="cstm-style datetime">Wed 2013-05-15 02:36:37 +0500</span>
<ul start="1"><li>[MDEV-4266](https://jira.mariadb.org/browse/MDEV-4266) Server upgrade via apt-get install does not work.         Now empty 'highlevel' packages strictly depend on the same versions of files.         These are mariadb-server, mariadb-client, mariadb-test
</li></ul>
</li><li>[Revision #3334.1.433](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.433)
   <span class="cstm-style datetime">Wed 2013-05-15 02:33:29 +0500</span>
<ul start="1"><li>[MDEV-4521](https://jira.mariadb.org/browse/MDEV-4521) MBRContains, MBRWithin no longer work with geometries of different type.         get_mm_leaf function can store all sorts of spatial features in         one type of field it receives from an Item_field.         So we just allow that by setting the type of this field to GEOMETRY.
</li></ul>
</li><li>[Revision #3334.1.432](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.432)
   <span class="cstm-style datetime">Tue 2013-05-14 18:32:16 +0300</span>
<ul start="1"><li>When one does 'REPAIR TABLE', update uuid() to the current system
</li></ul>
</li><li>[Revision #3334.1.431](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.431)
   <span class="cstm-style datetime">Tue 2013-05-14 14:49:52 +0200</span>
<ul start="1"><li>Fix test failure in plugins.unix_socket when running tests as user root.
</li></ul>
</li><li>[Revision #3334.1.430](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.430)
   <span class="cstm-style datetime">Mon 2013-05-13 16:11:39 +0200</span>
<ul start="1"><li>[MDEV-4514](https://jira.mariadb.org/browse/MDEV-4514) After increasing user name length mysql.db is reported broken and event scheduler does not start
</li></ul>
</li><li>[Revision #3334.1.429](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.429)
   <span class="cstm-style datetime">Mon 2013-05-13 15:49:48 +0200</span>
<ul start="1"><li>[MDEV-4505](https://jira.mariadb.org/browse/MDEV-4505) Buffer overrun when processing <code class="fixed" style="white-space:pre-wrap">--log-bin</code> parameter without file name
</li></ul>
</li><li>[Revision #3334.1.428](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.428)
   <span class="cstm-style datetime">Mon 2013-05-13 15:49:27 +0200</span>
<ul start="1"><li>[MDEV-4199](https://jira.mariadb.org/browse/MDEV-4199) Installing postfix on CentOS 5.9 requires MariaDB-server
</li></ul>
</li><li>[Revision #3334.1.427](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.427)
   <span class="cstm-style datetime">Mon 2013-05-13 15:46:58 +0200</span>
<ul start="1"><li>fix test cases
</li></ul>
</li><li>[Revision #3334.1.426](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.426)
   <span class="cstm-style datetime">Mon 2013-05-13 00:43:46 +0300</span>
<ul start="1"><li>Fixed [MDEV-4291](https://jira.mariadb.org/browse/MDEV-4291): Assertion `trid &gt;= info-&gt;s-&gt;state.create_trid' failure or data corruption (key points to record outside datafile) on INSERT into an Aria table.
</li></ul>
</li><li>[Revision #3334.1.425](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.425)
   <span class="cstm-style datetime">Sun 2013-05-12 11:29:16 +0300</span>
<ul start="1"><li>[MDEV-3999](https://jira.mariadb.org/browse/MDEV-3999): Valgrind errors 'invalid write' or assorted server crashes on concurrent flow with partitioned Aria tables [MDEV-3989](https://jira.mariadb.org/browse/MDEV-3989): Server crashes on import from MariaDB mysqldump export with partitioned Aria table.
</li></ul>
</li><li>[Revision #3334.1.424](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.424)
   <span class="cstm-style datetime">Sat 2013-05-11 20:31:50 +0300</span>
<ul start="1"><li>Fixed that SHOW PROCESSLIST and information_schema.processlist uses the right length for user names. Fixed some failing tests
</li></ul>
</li><li>[Revision #3334.1.423](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.423)
   <span class="cstm-style datetime">Sat 2013-05-11 12:20:21 +0300</span>
<ul start="1"><li>[MDEV-4231](https://jira.mariadb.org/browse/MDEV-4231): Possible bug in function _ma_apply_undo_row_insert() Added comment to clearify the code.
</li></ul>
</li><li>[Revision #3334.1.422](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.422)
   <span class="cstm-style datetime">Thu 2013-05-09 23:25:57 +0200</span>
<ul start="1"><li>Fix compile error
</li></ul>
</li><li>[Revision #3334.1.421](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.421)
   <span class="cstm-style datetime">Thu 2013-05-09 22:21:07 +0200</span>
<ul start="1"><li>Small mysql_install_db.exe fixes - Use lc-messages-dir instead of deprecated <code class="fixed" style="white-space:pre-wrap">--language</code> when running mysqld in bootstrap mode. - Add some verbosity to mysql_install_db.exe when it runs in course of MSI installation.
</li></ul>
</li><li>[Revision #3334.1.420](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.420)
   <span class="cstm-style datetime">Wed 2013-05-08 20:37:17 +0200</span>
<ul start="1"><li>[MDEV-4206](https://jira.mariadb.org/browse/MDEV-4206) : log all slow statements (do not use filters),  if  log_slow_filter is empty.
</li></ul>
</li><li>[Revision #3334.1.419](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.419)
   <span class="cstm-style datetime">Wed 2013-05-08 13:36:17 +0400</span>
<ul start="1"><li>The bug [MDEV-4489](https://jira.mariadb.org/browse/MDEV-4489) "Replication of big5, cp932, gbk, sjis strings makes wrong values on slave" has been fixed.
</li></ul>
</li><li>[Revision #3334.1.418](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.418) [merge]
   <span class="cstm-style datetime">Wed 2013-05-08 10:12:21 +0200</span>
<ul start="1"><li>Merge with XtraDB as of Percona-Server-5.5.30-rel30.2
</li><li>[Revision #0.12.62](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/0.12.62)
    <span class="cstm-style datetime">Wed 2013-05-08 09:52:54 +0200</span>
<ul start="1"><li>Percona-Server-5.5.30-rel30.2.tar.gz
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.417](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.417)
   <span class="cstm-style datetime">Tue 2013-05-07 18:28:36 +0200</span>
<ul start="1"><li>centos5 gcc 4.1 asm bug
</li></ul>
</li><li>[Revision #3334.1.416](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.416)
   <span class="cstm-style datetime">Tue 2013-05-07 18:26:22 +0200</span>
<ul start="1"><li>Compilation warnings. openssl compilation problem.
</li></ul>
</li><li>[Revision #3334.1.415](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.415) [merge]
   <span class="cstm-style datetime">Tue 2013-05-07 13:05:09 +0200</span>
<ul start="1"><li>mysql-5.5.31 merge
</li><li>[Revision #3077.184.104](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.104)
    <span class="cstm-style datetime">Fri 2013-04-12 12:11:38 +0200</span>
<ul start="1"><li>Updated mysql.spec.sh for rpm-uln
</li></ul>
</li><li>[Revision #3077.184.103](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.103)
    <span class="cstm-style datetime">Mon 2013-03-25 13:50:21 +0100</span>
<ul start="1"><li>Reverted MySQL Release Engineering mail address
</li></ul>
</li><li>[Revision #3077.184.102](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.102)
    <span class="cstm-style datetime">Thu 2013-03-21 14:59:57 +0100</span>
<ul start="1"><li>Added SuSE RPM Build fix
</li></ul>
</li><li>[Revision #3077.184.101](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.101)
    <span class="cstm-style datetime">Fri 2013-03-08 15:51:20 +0530</span>
</li><li>[Revision #3077.184.100](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.100) [merge]
    <span class="cstm-style datetime">Wed 2013-03-06 17:05:32 +0100</span>
<ul start="1"><li>Added fix for Bug#16445097
</li><li>[Revision #3077.175.120](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.120)
     <span class="cstm-style datetime">Wed 2013-03-06 16:33:26 +0100</span>
<ul start="1"><li>Added fix for Bug#16445097
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.99](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.99) [merge]
    <span class="cstm-style datetime">Tue 2013-03-05 16:34:14 +0100</span>
<ul start="1"><li>Updated Code for Bug#16235828 and Bug#16298542
</li><li>[Revision #3077.175.119](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.119)
     <span class="cstm-style datetime">Tue 2013-03-05 16:16:34 +0100</span>
<ul start="1"><li>Updated Code for Bug#16235828
</li></ul>
</li><li>[Revision #3077.175.118](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.118)
     <span class="cstm-style datetime">Fri 2013-03-01 14:11:24 +0100</span>
<ul start="1"><li>Updated mysql.spec.sh file for br16298542
</li></ul>
</li><li>[Revision #3077.175.117](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.117)
     <span class="cstm-style datetime">Thu 2013-02-28 14:49:54 +0100</span>
<ul start="1"><li>Updated release number in mysql.spec.sh file for br16298542
</li></ul>
</li><li>[Revision #3077.175.116](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.116)
     <span class="cstm-style datetime">Thu 2013-02-28 14:36:00 +0100</span>
<ul start="1"><li>Updated mysql.spec.sh file for br16298542
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.98](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.98) [merge]
    <span class="cstm-style datetime">Fri 2013-03-01 12:10:09 +0100</span>
<ul start="1"><li>L0ocal merge
</li><li>[Revision #3077.186.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.186.2)
     <span class="cstm-style datetime">Fri 2013-03-01 15:01:32 +0530</span>
<ul start="1"><li>BUG#11753923-SQL THREAD CRASHES ON DISK FULL Fixing post push issue Simulator name used needs to be changed to make it work properly.
</li></ul>
</li><li>[Revision #3077.186.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.186.1)
     <span class="cstm-style datetime">Thu 2013-02-28 14:52:47 +0100</span>
<ul start="1"><li>Bug#16385711: HANDLER, CREATE TABLE IF NOT EXISTS,               PROBLEM AFTER MYSQL_HA_FIND
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.97](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.97)
    <span class="cstm-style datetime">Thu 2013-02-28 13:19:15 +0100</span>
<ul start="1"><li>Bug#16414644 ASSERTION FAILED: SIZE == PFS_ALLOCATED_MEMORY
</li></ul>
</li><li>[Revision #3077.184.96](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.96)
    <span class="cstm-style datetime">Thu 2013-02-28 14:50:42 +0530</span>
</li><li>[Revision #3077.184.95](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.95) [merge]
    <span class="cstm-style datetime">Thu 2013-02-28 09:54:27 +0530</span>
<ul start="1"><li>[Revision #2661.844.52](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.52) [merge]
     <span class="cstm-style datetime">Thu 2013-02-28 09:52:55 +0530</span>
<ul start="1"><li>[Revision #2661.847.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.847.1)
      <span class="cstm-style datetime">Fri 2012-09-14 19:19:21 +0530</span>
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.94](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.94) [merge]
    <span class="cstm-style datetime">Thu 2013-02-28 01:33:00 +0400</span>
<ul start="1"><li>Manual up-merge (16311231 backport)
</li><li>[Revision #2661.844.51](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.51)
     <span class="cstm-style datetime">Wed 2013-02-27 23:21:34 +0400</span>
<ul start="1"><li>Bug #16311231: MISSING DATA ON SUBQUERY WITH WHERE + XOR IN IN-CLAUSE USING MYISAM OR MEMORY ENGINE
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.93](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.93)
    <span class="cstm-style datetime">Wed 2013-02-27 12:44:58 -0600</span>
<ul start="1"><li>Bug #16305265 	HANG IN RENAME TABLE
</li></ul>
</li><li>[Revision #3077.184.92](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.92) [merge]
    <span class="cstm-style datetime">Wed 2013-02-27 10:04:43 +0200</span>
<ul start="1"><li>Merge mysql-5.1 to mysql-5.5.
</li><li>[Revision #2661.844.50](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.50)
     <span class="cstm-style datetime">Wed 2013-02-27 10:00:50 +0200</span>
<ul start="1"><li>Bug#16400920 INNODB TRIES TO PASS EMPTY BUFFER TO ZLIB, GETS Z_BUF_ERROR
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.91](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.91) [merge]
    <span class="cstm-style datetime">Tue 2013-02-26 21:29:43 +0530</span>
<ul start="1"><li>Bug#16372927: STACK OVERFLOW WITH LONG DATABASE NAME IN               GRANT STATEMENT
</li><li>[Revision #2661.844.49](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.49)
     <span class="cstm-style datetime">Tue 2013-02-26 21:23:06 +0530</span>
<ul start="1"><li>Bug#16372927: STACK OVERFLOW WITH LONG DATABASE NAME IN               GRANT STATEMENT
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.90](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.90)
    <span class="cstm-style datetime">Mon 2013-02-25 10:42:40 +0100</span>
<ul start="1"><li>Bug#16062056 	REMOVE THE "DUMMY.BAK" FILE FROM THE TEST DATABASE, AND ADD DB.OPT
</li></ul>
</li><li>[Revision #3077.184.89](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.89)
    <span class="cstm-style datetime">Tue 2013-02-26 17:57:05 +0530</span>
<ul start="1"><li>Bug#14653504  CRASH WHEN TRUNCATING PARTITIONS FROM A VIEW!
</li></ul>
</li><li>[Revision #3077.184.88](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.88) [merge]
    <span class="cstm-style datetime">Tue 2013-02-26 06:35:17 +0100</span>
<ul start="1"><li>Updated/added copyright headers
</li><li>[Revision #2661.844.48](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.48)
     <span class="cstm-style datetime">Mon 2013-02-25 15:26:00 +0100</span>
<ul start="1"><li>Updated/added copyright headers.
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.87](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.87)
    <span class="cstm-style datetime">Mon 2013-02-25 19:37:46 +0530</span>
<ul start="1"><li>Bug#16103072 TEST MYSQL_PLUGIN USES UNSAFE WRITE_FILE TO WRITE                                                  TO EXPECT FILE
</li></ul>
</li><li>[Revision #3077.184.86](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.86)
    <span class="cstm-style datetime">Mon 2013-02-25 13:45:00 +0100</span>
</li><li>[Revision #3077.184.85](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.85)
    <span class="cstm-style datetime">Mon 2013-02-25 10:28:25 +0530</span>
<ul start="1"><li>Bug #16044655 CRASH: SETTING DEFAULT VALUE FOR SOME VARIABLES
</li></ul>
</li><li>[Revision #3077.184.84](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.84) [merge]
    <span class="cstm-style datetime">Sat 2013-02-23 10:47:30 +0100</span>
<ul start="1"><li>Upmerging the changes from 5.1 for copyright changes.
</li><li>[Revision #2661.844.47](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.47)
     <span class="cstm-style datetime">Sat 2013-02-23 10:38:28 +0100</span>
</li></ul>
</li><li>[Revision #3077.184.83](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.83)
    <span class="cstm-style datetime">Sat 2013-02-23 10:40:23 +0100</span>
</li><li>[Revision #3077.184.82](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.82)
    <span class="cstm-style datetime">Sat 2013-02-23 00:16:36 +0530</span>
<ul start="1"><li>Testcase fix for Bug#14147491
</li></ul>
</li><li>[Revision #3077.184.81](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.81)
    <span class="cstm-style datetime">Fri 2013-02-22 15:22:15 +0100</span>
<ul start="1"><li>Bug #13619394 - MAKE TEST FAILS ON MY_VSNPRINTF
</li></ul>
</li><li>[Revision #3077.184.80](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.80) [merge]
    <span class="cstm-style datetime">Fri 2013-02-22 12:32:29 +0100</span>
<ul start="1"><li>merge
</li><li>[Revision #3077.185.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.185.1)
     <span class="cstm-style datetime">Wed 2013-02-20 12:41:43 +0100</span>
<ul start="1"><li>Bug #13071597: MYSQL SERVER COMMUNITY TO ADVANCED USING MSI THE INSTALLER
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.79](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.79) [merge]
    <span class="cstm-style datetime">Fri 2013-02-22 15:15:14 +0530</span>
<ul start="1"><li>Merge from mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.844.46](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.46)
     <span class="cstm-style datetime">Fri 2013-02-22 14:56:17 +0530</span>
<ul start="1"><li>Bug #14211565 CRASH WHEN ATTEMPTING TO SET SYSTEM VARIABLE TO RESULT OF VALUES()
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.78](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.78)
    <span class="cstm-style datetime">Wed 2013-02-20 11:24:16 +0100</span>
<ul start="1"><li>Bug#14300733 CMAKE DOES NOT CHECK FOR ZLIB VERSION
</li></ul>
</li><li>[Revision #3077.184.77](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.77)
    <span class="cstm-style datetime">Thu 2013-02-21 12:16:59 +0530</span>
<ul start="1"><li>Testcase fix for Bug#14147491
</li></ul>
</li><li>[Revision #3077.184.76](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.76)
    <span class="cstm-style datetime">Wed 2013-02-20 18:25:18 +0530</span>
<ul start="1"><li>Testcase fix for BUG#14147491
</li></ul>
</li><li>[Revision #3077.184.75](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.75) [merge]
    <span class="cstm-style datetime">Tue 2013-02-19 14:36:30 +0530</span>
<ul start="1"><li>Merge from mysq-5.1 to mysql-5.5
</li><li>[Revision #2661.844.45](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.45)
     <span class="cstm-style datetime">Tue 2013-02-19 14:31:11 +0530</span>
<ul start="1"><li>Bug#11746817:MYSQL_INSTALL_DB CREATES WILDCARD GRANTS WHEN HOST HAS '_' IN THE HOSTNAME
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.74](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.74) [merge]
    <span class="cstm-style datetime">Tue 2013-02-19 12:19:10 +0530</span>
<ul start="1"><li>Bug#16235681: TURN OFF DEFAULT COMPRESSION WHILE USING               OPENSSL
</li><li>[Revision #2661.844.44](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.44)
     <span class="cstm-style datetime">Tue 2013-02-19 12:17:31 +0530</span>
<ul start="1"><li>Bug#16235681: TURN OFF DEFAULT COMPRESSION WHILE USING               OPENSSL
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.73](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.73) [merge]
    <span class="cstm-style datetime">Tue 2013-02-19 10:59:45 +0530</span>
<ul start="1"><li>Null merge from mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.844.43](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.43)
     <span class="cstm-style datetime">Tue 2013-02-19 10:55:55 +0530</span>
</li></ul>
</li><li>[Revision #3077.184.72](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.72)
    <span class="cstm-style datetime">Tue 2013-02-19 01:58:57 +0530</span>
<ul start="1"><li>BUG#15965353- RPL.RPL_ROW_UNTIL FAILS ON PB2,               PLATFORM= MACOSX10.6 X86_64 MAX
</li></ul>
</li><li>[Revision #3077.184.71](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.71) [merge]
    <span class="cstm-style datetime">Mon 2013-02-18 17:06:00 +0000</span>
<ul start="1"><li>BUG#13545447: RPL_ROTATE_LOGS FAILS DUE TO CONCURRENCY ISSUES IN REP. CODE
</li><li>[Revision #2661.844.42](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.42)
     <span class="cstm-style datetime">Mon 2013-02-18 17:02:26 +0000</span>
<ul start="1"><li>BUG#13545447: RPL_ROTATE_LOGS FAILS DUE TO CONCURRENCY ISSUES IN REP. CODE
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.70](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.70)
    <span class="cstm-style datetime">Mon 2013-02-18 19:13:06 +0530</span>
<ul start="1"><li>Bug #12546953   "SHOW VARIABLES LIKE 'DATADIR';" RETURN EMPTY. Problem: =========================================================== If mysqld daemon is started without a <code class="fixed" style="white-space:pre-wrap">--datadir</code> option option, and we issue the SHOW VARIABLES LIKE 'DATADIR';SQL command at the client it returns an empty path. This is because mysql_real_data_home_ptr is being reset to NULL by Sys_var_charptr constructor call when the datadir is not given either through configuration file (no-defaults) or through mysqld parameters.
</li></ul>
</li><li>[Revision #3077.184.69](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.69)
    <span class="cstm-style datetime">Mon 2013-02-18 11:12:24 +0100</span>
<ul start="1"><li>BUG#13545447: RPL_ROTATE_LOGS FAILS DUE TO CONCURRENCY ISSUES IN REP. CODE Post-push fix, broken build: sql/rpl_master.cc:1049:70: error: converting ‘false’ to pointer type ‘bool*’ [-Werror=conversion-null]
</li></ul>
</li><li>[Revision #3077.184.68](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.68)
    <span class="cstm-style datetime">Mon 2013-02-18 11:13:48 +0530</span>
</li><li>[Revision #3077.184.67](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.67) [merge]
    <span class="cstm-style datetime">Sun 2013-02-17 01:45:10 +0530</span>
<ul start="1"><li>BUG#15965353- RPL.RPL_ROW_UNTIL FAILS ON PB2,               PLATFORM= MACOSX10.6 X86_64 MAX
</li><li>[Revision #2661.844.41](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.41)
     <span class="cstm-style datetime">Sun 2013-02-17 01:42:28 +0530</span>
<ul start="1"><li>BUG#15965353- RPL.RPL_ROW_UNTIL FAILS ON PB2,               PLATFORM= MACOSX10.6 X86_64 MAX
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.66](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.66) [merge]
    <span class="cstm-style datetime">Fri 2013-02-15 22:18:37 +0000</span>
<ul start="1"><li>BUG#13545447: RPL_ROTATE_LOGS FAILS DUE TO CONCURRENCY ISSUES IN REP. CODE
</li><li>[Revision #2661.844.40](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.40)
     <span class="cstm-style datetime">Fri 2013-02-15 21:57:35 +0000</span>
<ul start="1"><li>BUG#13545447: RPL_ROTATE_LOGS FAILS DUE TO CONCURRENCY ISSUES IN REP. CODE
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.65](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.65)
    <span class="cstm-style datetime">Fri 2013-02-15 16:01:37 +0400</span>
<ul start="1"><li>Bug#16056537: MYSQLD CRASHES IN ITEM_FUNC_GET_USER_VAR::FIX_LENGTH_AND_DEC()
</li></ul>
</li><li>[Revision #3077.184.64](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.64) [merge]
    <span class="cstm-style datetime">Fri 2013-02-15 12:37:21 +0530</span>
<ul start="1"><li>Bug#16218104: MYSQL YASSL - LUCKY THIRTEEN: BREAKING THE               TLS AND DTLS RECORD PROTOCOLS
</li><li>[Revision #2661.844.39](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.39)
     <span class="cstm-style datetime">Fri 2013-02-15 12:35:54 +0530</span>
<ul start="1"><li>Bug#16218104: MYSQL YASSL - LUCKY THIRTEEN: BREAKING THE               TLS AND DTLS RECORD PROTOCOLS
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.63](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.63) [merge]
    <span class="cstm-style datetime">Fri 2013-02-15 00:40:32 +0530</span>
<ul start="1"><li>BUG#12359942- REPLICATION TEST FROM ENGINE SUITE RPL_ROW_UNTIL TIMES OUT
</li><li>[Revision #2661.844.38](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.38)
     <span class="cstm-style datetime">Fri 2013-02-15 00:38:42 +0530</span>
<ul start="1"><li>BUG#12359942- REPLICATION TEST FROM ENGINE SUITE RPL_ROW_UNTIL TIMES OUT
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.62](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.62)
    <span class="cstm-style datetime">Thu 2013-02-14 17:03:49 +0100</span>
<ul start="1"><li>Bug#16274455: CAN NOT ACESS PARTITIONED TABLES WHEN DOWNGRADED FROM 5.6.11 TO 5.6.10
</li></ul>
</li><li>[Revision #3077.184.61](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.61) [merge]
    <span class="cstm-style datetime">Thu 2013-02-14 16:35:40 +0530</span>
<ul start="1"><li>Merge from mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.844.37](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.37)
     <span class="cstm-style datetime">Thu 2013-02-14 16:33:31 +0530</span>
<ul start="1"><li>For the error code ER_TOO_LONG_IDENT, the identifier is expected in the my_error call.  So removing this line from here.
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.60](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.60) [merge]
    <span class="cstm-style datetime">Tue 2013-02-12 15:35:56 +0530</span>
<ul start="1"><li>Merge from mysql-5.1 to mysql-5.5.
</li><li>[Revision #2661.844.36](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.36)
     <span class="cstm-style datetime">Tue 2013-02-12 14:52:48 +0530</span>
<ul start="1"><li>Bug #11753153 INNODB GENERATES SYMBOLS THAT ARE TOO LONG, INVALID DDL FROM SHOW CREATE
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.59](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.59)
    <span class="cstm-style datetime">Mon 2013-02-04 14:09:48 +0100</span>
<ul start="1"><li>post-push test result update for bug#14521864.
</li></ul>
</li><li>[Revision #3077.184.58](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.58) [merge]
    <span class="cstm-style datetime">Fri 2013-02-08 16:36:47 +0530</span>
<ul start="1"><li>BUG#16247322-MTR NOT RUNNING SYS_VARS TEST SUITE FOR 5.1  Null merge from mysql-5.1
</li><li>[Revision #2661.844.35](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.35)
     <span class="cstm-style datetime">Fri 2013-02-08 16:34:32 +0530</span>
<ul start="1"><li>BUG#16247322-MTR NOT RUNNING SYS_VARS TEST SUITE FOR 5.1 
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.57](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.57) [merge]
    <span class="cstm-style datetime">Fri 2013-02-08 15:42:36 +0530</span>
<ul start="1"><li>BUG#16247322-MTR NOT RUNNING SYS_VARS TEST SUITE FOR 5.1  Null merge from mysql-5.1
</li><li>[Revision #2661.844.34](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.34)
     <span class="cstm-style datetime">Fri 2013-02-08 15:41:18 +0530</span>
<ul start="1"><li>BUG#16247322-MTR NOT RUNNING SYS_VARS TEST SUITE FOR 5.1
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.56](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.56) [merge]
    <span class="cstm-style datetime">Fri 2013-02-08 09:33:21 +0200</span>
<ul start="1"><li>Merge mysql-5.1 to mysql-5.5.
</li><li>[Revision #2661.844.33](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.33)
     <span class="cstm-style datetime">Fri 2013-02-08 09:23:12 +0200</span>
<ul start="1"><li>Add missing linkage specifiers, so that ha_innodb_plugin.so will not export internal symbols.
</li></ul>
</li><li>[Revision #2661.844.32](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.32)
     <span class="cstm-style datetime">Fri 2013-02-08 09:22:46 +0200</span>
<ul start="1"><li>Bug#16292043 RACE CONDITION IN SRV_EXPORT_INNODB_STATUS() WHEN ACCESSING PURGE_SYS-&gt;VIEW
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.55](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.55) [merge]
    <span class="cstm-style datetime">Thu 2013-02-07 19:46:45 +0200</span>
<ul start="1"><li>Null-merge from mysql-5.1
</li><li>[Revision #2661.844.31](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.31)
     <span class="cstm-style datetime">Thu 2013-02-07 19:46:08 +0200</span>
<ul start="1"><li>bug#14163155 COM_CHANGE_USER DOESN'T WORK WITH CHARACTER-SET-SERVER=UCS2 IN              5.1 SERVER
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.54](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.54) [merge]
    <span class="cstm-style datetime">Thu 2013-02-07 17:08:59 +0100</span>
<ul start="1"><li>merge 5.1 =&gt; 5.5
</li><li>[Revision #2661.844.30](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.30)
     <span class="cstm-style datetime">Thu 2013-02-07 17:05:07 +0100</span>
<ul start="1"><li>Bug#16192219 CRASH IN TEST_IF_SKIP_SORT_ORDER ON SELECT DISTINCT WITH ORDER BY
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.53](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.53) [merge]
    <span class="cstm-style datetime">Thu 2013-02-07 17:27:32 +0530</span>
<ul start="1"><li>Bug #16247322 MTR NOT RUNNING SYS_VARS TEST SUITE FOR 5.1 Null-Merge from mysql-5.5
</li><li>[Revision #2661.844.29](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.29)
     <span class="cstm-style datetime">Thu 2013-02-07 17:23:37 +0530</span>
<ul start="1"><li>Bug#16247322- MTR NOT RUNNING SYS_VARS TEST SUITE FOR 5.1
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.52](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.52)
    <span class="cstm-style datetime">Wed 2013-02-06 13:49:56 -0600</span>
<ul start="1"><li>Bug#16263506 - INNODB; USE ABORT() ON ALL PLATFORMS INSTEAD OF                DEREFERENCING UT_DBG_NULL_PTR The abort() call is standard C but InnoDB only uses it in GCC environments.  UT_DBG_USE_ABORT is not defined the code crashed by dereferencing a null pointer instead of calling abort(). Other code throughout MySQL including ndb, sql, mysys and other places call abort() directly.
</li></ul>
</li><li>[Revision #3077.184.51](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.51)
    <span class="cstm-style datetime">Wed 2013-02-06 13:52:32 +0530</span>
<ul start="1"><li>Bug#14711808 MSI INSTALLATION / UPGRADE CAN CORRUPT EXISTING INSTALLATION
</li></ul>
</li><li>[Revision #3077.184.50](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.50) [merge]
    <span class="cstm-style datetime">Wed 2013-02-06 13:04:41 +0530</span>
<ul start="1"><li>13625278 5.1 =&gt; 5.5
</li><li>[Revision #2661.844.28](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.28)
     <span class="cstm-style datetime">Wed 2013-02-06 13:02:14 +0530</span>
<ul start="1"><li>BUG #13625278 - PB2 SHOULD PROVIDE MORE USEFUL INFORMATION FOR TIMEOUTS
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.49](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.49) [merge]
    <span class="cstm-style datetime">Tue 2013-02-05 21:29:49 +0100</span>
<ul start="1"><li>Upmerge of the 5.1.68 build
</li><li>[Revision #2661.844.27](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.27) [merge]
     <span class="cstm-style datetime">Tue 2013-02-05 20:47:45 +0100</span>
<ul start="1"><li>Merge from mysql-5.1.68-release
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.48](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.48) [merge]
    <span class="cstm-style datetime">Tue 2013-02-05 10:50:02 +0100</span>
<ul start="1"><li>Merge from mysql-5.5.30-release
</li></ul>
</li><li>[Revision #3077.184.47](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.47) [merge]
    <span class="cstm-style datetime">Tue 2013-02-05 13:42:56 +0530</span>
<ul start="1"><li>13625278 5.1=&gt; 5.5
</li><li>[Revision #2661.844.26](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.26)
     <span class="cstm-style datetime">Tue 2013-02-05 11:58:21 +0530</span>
<ul start="1"><li>BUG #13625278 - PB2 SHOULD PROVIDE MORE USEFUL INFORMATION FOR TIMEOUTS
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.46](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.46)
    <span class="cstm-style datetime">Tue 2013-02-05 11:06:38 +0530</span>
<ul start="1"><li>BUG#16196591 - CLIENTS CANNOT CONNECT TO MYSQL PROBLEM: When large number of connections are continuously made with wait_timeout of 600 seconds for  some hours, some connections remain after wait_timeout expired and also new connections get struck under the configuration and the scenario reported in bug#16196591. FIX: The cause of this bug is the issue identified  and fixed in the BUG#16088658 in 5.6.Also LOCK_thread_count contention  issue fixed in BUG#15921866 in 5.6 need to be in 5.5 as  well. Since the issue is not reproducible, it has been verified at customer configuration the issue could not be reproduced after a 48-hour test with a non-debug build                which includes the above two fixes backported.
</li></ul>
</li><li>[Revision #3077.184.45](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.45) [merge]
    <span class="cstm-style datetime">Mon 2013-02-04 20:41:25 +0530</span>
<ul start="1"><li>16190704 5.1 =&gt; 5.5 nullmerge
</li><li>[Revision #2661.844.25](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.25)
     <span class="cstm-style datetime">Mon 2013-02-04 20:25:30 +0530</span>
<ul start="1"><li>Bug #16190704: MTR STILL LOSES THE FAILED RUN LOGS AT RETRY-FAIL
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.44](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.44)
    <span class="cstm-style datetime">Fri 2013-02-01 09:47:16 -0500</span>
<ul start="1"><li>Bug#16249505 INNODB REPORTS THAT IT'S GOING TO WAIT FOR I/O BUT THE I/O IS ASYNC
</li></ul>
</li><li>[Revision #3077.184.43](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.43)
    <span class="cstm-style datetime">Fri 2013-02-01 19:53:20 +0530</span>
<ul start="1"><li>BUG #16190704 - MTR STILL LOSES THE FAILED RUN LOGS AT RETRY-FAIL
</li></ul>
</li><li>[Revision #3077.184.42](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.42)
    <span class="cstm-style datetime">Fri 2013-02-01 09:49:27 +0530</span>
<ul start="1"><li>BUG#16207679: MISSING ERROR WHEN RESIGNAL TO MYSQL_ERROR=5
</li></ul>
</li><li>[Revision #3077.184.41](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.41) [merge]
    <span class="cstm-style datetime">Thu 2013-01-31 09:13:42 +0400</span>
<ul start="1"><li>Bug #11827369: ASSERTION FAILED: !THD-&gt;LEX-&gt;CONTEXT_ANALYSIS_ONLY Manual up-merge from 5.1 to 5.5.
</li><li>[Revision #2661.844.24](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.24) [merge]
     <span class="cstm-style datetime">Thu 2013-01-31 08:46:30 +0400</span>
<ul start="1"><li>Bug #11827369: ASSERTION FAILED: !THD-&gt;LEX-&gt;CONTEXT_ANALYSIS_ONLY
</li><li>[Revision #2661.846.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.846.1)
      <span class="cstm-style datetime">Wed 2013-01-23 09:51:50 +0400</span>
<ul start="1"><li>Bug #11827369: ASSERTION FAILED: !THD-&gt;LEX-&gt;CONTEXT_ANALYSIS_ONLY
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.40](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.40) [merge]
    <span class="cstm-style datetime">Thu 2013-01-31 12:45:08 +0900</span>
<ul start="1"><li>merge to mysql-5.5 from mysql-5.1
</li><li>[Revision #2661.844.23](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.23)
     <span class="cstm-style datetime">Thu 2013-01-31 12:42:43 +0900</span>
<ul start="1"><li>Bug #16220051 : INNODB_BUG12400341 FAILS ON VALGRIND WITH TOO MANY ACTIVE CONCURRENT TRANSACTION innodb_bug12400341.test is disabled for valgrind daily test. It might be affected by the previous test's undo slots existing, because of slower execution.
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.39](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.39) [merge]
    <span class="cstm-style datetime">Thu 2013-01-31 07:06:30 +0530</span>
<ul start="1"><li>Bug#14096619: UNABLE TO RESTORE DATABASE DUMP
</li><li>[Revision #2661.844.22](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.22)
     <span class="cstm-style datetime">Thu 2013-01-31 06:39:15 +0530</span>
<ul start="1"><li>Bug#14096619: UNABLE TO RESTORE DATABASE DUMP
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.38](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.38) [merge]
    <span class="cstm-style datetime">Wed 2013-01-30 17:51:52 +0100</span>
<ul start="1"><li>Bug#14521864: MYSQL 5.1 TO 5.5 BUGS PARTITIONING
</li><li>[Revision #2661.844.21](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.21) [merge]
     <span class="cstm-style datetime">Wed 2013-01-30 15:17:19 +0100</span>
<ul start="1"><li>[Revision #2661.845.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.845.1)
      <span class="cstm-style datetime">Fri 2013-01-04 18:32:42 +0100</span>
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.37](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.37)
    <span class="cstm-style datetime">Wed 2013-01-30 14:32:52 +0530</span>
</li><li>[Revision #3077.184.36](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.36)
    <span class="cstm-style datetime">Wed 2013-01-30 10:53:43 +0530</span>
<ul start="1"><li>Bug#14756795  SELECT FROM NEW INNODB I_S TABLES CRASHES SERVER               WITH <code class="fixed" style="white-space:pre-wrap">--SKIP-INNODB</code>
</li></ul>
</li><li>[Revision #3077.184.35](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.35) [merge]
    <span class="cstm-style datetime">Wed 2013-01-30 08:27:33 +0530</span>
<ul start="1"><li>- BUG#1608883:  KILLING A QUERY INSIDE INNODB CAUSES IT TO EVENTUALLY CRASH   WITH AN ASSERTION   Null merge from mysql-5.1
</li><li>[Revision #2661.844.20](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.20)
     <span class="cstm-style datetime">Wed 2013-01-30 08:17:24 +0530</span>
<ul start="1"><li>- BUG#1608883:  KILLING A QUERY INSIDE INNODB CAUSES IT TO EVENTUALLY CRASH   WITH AN ASSERTION
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.34](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.34) [merge]
    <span class="cstm-style datetime">Tue 2013-01-29 10:06:31 +0530</span>
<ul start="1"><li>Bug#16208709 - CRASH IN GET_SEL_ARG_FOR_KEYPART ON SELECT DISTINCT  	       ON COL WITH COMPOSITE INDEX
</li><li>[Revision #2661.844.19](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.19)
     <span class="cstm-style datetime">Tue 2013-01-29 10:05:00 +0530</span>
<ul start="1"><li>Bug#16208709 - CRASH IN GET_SEL_ARG_FOR_KEYPART ON SELECT DISTINCT  	       ON COL WITH COMPOSITE INDEX
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.33](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.33) [merge]
    <span class="cstm-style datetime">Mon 2013-01-28 19:08:50 +0000</span>
<ul start="1"><li>BUG#16200555: EMPTY NAME FOR USER VARIABLE IS ALLOWED AND BREAKS STATEMENT BINARY LOGGING
</li><li>[Revision #2661.844.18](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.18)
     <span class="cstm-style datetime">Mon 2013-01-28 19:05:09 +0000</span>
<ul start="1"><li>BUG#16200555: EMPTY NAME FOR USER VARIABLE IS ALLOWED AND BREAKS STATEMENT BINARY LOGGING
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.32](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.32)
    <span class="cstm-style datetime">Mon 2013-01-28 20:13:44 +0530</span>
<ul start="1"><li>Bug#16183892 - INNODB PURGE BUFFERING IS NOT CRASH-SAFE
</li></ul>
</li><li>[Revision #3077.184.31](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.31) [merge]
    <span class="cstm-style datetime">Mon 2013-01-28 14:58:55 +0530</span>
<ul start="1"><li>Bug#16084594 USER_VAR ITEM IN 'LOAD FILE QUERY' WAS NOT PROPERLY QUOTED IN BINLOG FILE Merging fix from mysql-5.1
</li><li>[Revision #2661.844.17](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.17)
     <span class="cstm-style datetime">Mon 2013-01-28 14:41:54 +0530</span>
<ul start="1"><li>Bug#16084594 USER_VAR ITEM IN 'LOAD FILE QUERY' WAS NOT PROPERLY QUOTED IN BINLOG FILE Problem: In load data file query, User variables are allowed inside "Into_list" and "Set_list". These user variables used inside these two lists are not properly guarded with backticks while server is writting into binlog. Hence user variable names like a` cannot be used in this context.
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.30](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.30)
    <span class="cstm-style datetime">Sat 2013-01-26 15:03:01 +0530</span>
<ul start="1"><li>Bug#16056813-MEMORY LEAK ON FILTERED SLAVE
</li></ul>
</li><li>[Revision #3077.184.29](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.29) [merge]
    <span class="cstm-style datetime">Thu 2013-01-24 15:05:15 +0530</span>
<ul start="1"><li>[Revision #2661.844.16](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.16)
     <span class="cstm-style datetime">Thu 2013-01-24 14:56:12 +0530</span>
<ul start="1"><li>BUG#11908153 CRASH AND/OR VALGRIND ERRORS IN FIELD_BLOB::GET_KEY_IMAGE
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.28](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.28) [merge]
    <span class="cstm-style datetime">Thu 2013-01-24 14:13:42 +0530</span>
<ul start="1"><li>Bug #11752803  SERVER CRASHES IF MAX_CONNECTIONS DECREASED BELOW                 CERTAIN LEVEL
</li><li>[Revision #2661.844.15](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.15)
     <span class="cstm-style datetime">Thu 2013-01-24 14:02:54 +0530</span>
<ul start="1"><li>Bug #11752803  SERVER CRASHES IF MAX_CONNECTIONS DECREASED BELOW                 CERTAIN LEVEL
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.27](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.27)
    <span class="cstm-style datetime">Thu 2013-01-24 10:35:07 +0530</span>
<ul start="1"><li>BUG#14798572: REMOVE UNUSED VARIABLE BINLOG_CAN_BE_CORRUPTED FROM MYSQL_BINLOG_SEND
</li></ul>
</li><li>[Revision #3077.184.26](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.26) [merge]
    <span class="cstm-style datetime">Wed 2013-01-23 15:00:46 +0900</span>
<ul start="1"><li>Merge mysql-5.1 to mysql-5.5.
</li><li>[Revision #2661.844.14](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.14)
     <span class="cstm-style datetime">Wed 2013-01-23 14:59:36 +0900</span>
<ul start="1"><li>Bug #16089381 : POSSIBLE NUMBER UNDERFLOW AROUND CALLING PAGE_ZIP_EMPTY_SIZE()
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.25](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.25) [merge]
    <span class="cstm-style datetime">Mon 2013-01-21 15:19:18 +0200</span>
<ul start="1"><li>Merge mysql-5.1 to mysql-5.5.
</li><li>[Revision #2661.844.13](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.13)
     <span class="cstm-style datetime">Mon 2013-01-21 14:59:49 +0200</span>
<ul start="1"><li>Bug#16067973 DROP TABLE SLOW WHEN IT DECOMPRESS COMPRESSED-ONLY PAGES
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.24](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.24) [merge]
    <span class="cstm-style datetime">Sat 2013-01-19 06:07:08 +0530</span>
<ul start="1"><li>BUG#11752707-SLAVE CRASHES IF RBR HAS AS DESTINATION A VIEW RATHER THAN A TABLE. Merging fix from mysql-5.1
</li><li>[Revision #2661.844.12](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.12)
     <span class="cstm-style datetime">Sat 2013-01-19 06:01:46 +0530</span>
<ul start="1"><li>Bug#11752707-SLAVE CRASHES IF RBR HAS AS DESTINATION A VIEW RATHER THAN A TABLE
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.23](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.23)
    <span class="cstm-style datetime">Fri 2013-01-18 18:26:02 +0530</span>
<ul start="1"><li>BUG#11761680 disabled binlog_spurious_ddl_errors on mysql-5.5
</li></ul>
</li><li>[Revision #3077.184.22](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.22)
    <span class="cstm-style datetime">Fri 2013-01-18 14:13:59 +0200</span>
</li><li>[Revision #3077.184.21](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.21)
    <span class="cstm-style datetime">Fri 2013-01-18 12:32:37 +0530</span>
<ul start="1"><li>Description       The test, binlog.binlog_spurious_ddl_errors was failing on pb2 at the statement       "UNINSTALL PLUGIN example;" with this warning:       "Warning	1620	Plugin is busy and will be uninstalled on shutdown "
</li></ul>
</li><li>[Revision #3077.184.20](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.20)
    <span class="cstm-style datetime">Thu 2013-01-17 17:30:13 +0200</span>
<ul start="1"><li>Bug#16138582 MTR_MEMO_RELEASE AND DYN_ARRAY TOGETHER ARE VERY INEFFICIENT
</li></ul>
</li><li>[Revision #3077.184.19](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.19) [merge]
    <span class="cstm-style datetime">Wed 2013-01-16 18:28:28 +0530</span>
<ul start="1"><li>BUG#14117025: UNABLE TO RESTORE DUMP Null Merge from 5.1 to 5.5
</li><li>[Revision #2661.844.11](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.11)
     <span class="cstm-style datetime">Wed 2013-01-16 18:26:27 +0530</span>
<ul start="1"><li>BUG#14117025: UNABLE TO RESTORE DUMP Problem: When a view, with a specific character set and collation,  is created on another view with a different character set and collation the  dump restoration results in an illegal mix of collations error. SOLUTION: To avoid this confusion of collations, the create table datatype  being used is hardcoded as "tinyint NOT NULL". This will not matter as the table  created will be dropped at runtime and specifically tinyint is used to  avoid hitting the row size conflicts.
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.18](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.18) [merge]
    <span class="cstm-style datetime">Wed 2013-01-16 15:11:49 +0530</span>
<ul start="1"><li>Bug#11751794 MYSQL GIVES THE WRONG RESULT WITH SOME SPECIAL USAGE
</li><li>[Revision #2661.844.10](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.10)
     <span class="cstm-style datetime">Wed 2013-01-16 15:03:42 +0530</span>
<ul start="1"><li>Bug#11751794 MYSQL GIVES THE WRONG RESULT WITH SOME SPECIAL USAGE
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.17](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.17)
    <span class="cstm-style datetime">Wed 2013-01-16 11:30:34 +0530</span>
</li><li>[Revision #3077.184.16](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.16)
    <span class="cstm-style datetime">Tue 2013-01-15 15:30:26 +0530</span>
<ul start="1"><li>Bug#11757464:SERVER CRASH IN RECURSIVE CALL WHEN OOM
</li></ul>
</li><li>[Revision #3077.184.15](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.15) [merge]
    <span class="cstm-style datetime">Tue 2013-01-15 14:33:22 +0530</span>
<ul start="1"><li>Bug#11758009 - UNION EXECUTION ORDER WRONG ?
</li><li>[Revision #2661.844.9](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.9)
     <span class="cstm-style datetime">Tue 2013-01-15 14:24:35 +0530</span>
<ul start="1"><li>Bug#11758009 - UNION EXECUTION ORDER WRONG ?
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.14](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.14)
    <span class="cstm-style datetime">Tue 2013-01-15 12:26:49 +0530</span>
</li><li>[Revision #3077.184.13](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.13) [merge]
    <span class="cstm-style datetime">Mon 2013-01-14 16:51:52 +0530</span>
<ul start="1"><li>BUG#14303860 - EXECUTING A SELECT QUERY WITH TOO  MANY WILDCARDS CAUSES A SEGFAULT       Back port from 5.6 and trunk
</li><li>[Revision #2661.844.8](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.8)
     <span class="cstm-style datetime">Mon 2013-01-14 14:59:48 +0530</span>
<ul start="1"><li>BUG#14303860 - EXECUTING A SELECT QUERY WITH TOO  MANY WILDCARDS CAUSES A SEGFAULT
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.12](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.12)
    <span class="cstm-style datetime">Mon 2013-01-14 10:58:17 +0100</span>
<ul start="1"><li>Fix for Bug#14636211 WRONG RESULT (EXTRA ROW) ON A FROM SUBQUERY                       WITH A VARIABLE AND ORDER BY         Bug#16035412 MYSQL SERVER 5.5.29 WRONG SORTING USING COMPLEX INDEX
</li></ul>
</li><li>[Revision #3077.184.11](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.11) [merge]
    <span class="cstm-style datetime">Mon 2013-01-14 10:57:04 +0530</span>
<ul start="1"><li>Merge from mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.844.7](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.7)
     <span class="cstm-style datetime">Mon 2013-01-14 10:49:51 +0530</span>
<ul start="1"><li>- BUG#1608883:  KILLING A QUERY INSIDE INNODB CAUSES IT TO EVENTUALLY CRASH   WITH AN ASSERTION
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.10](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.10) [merge]
    <span class="cstm-style datetime">Sat 2013-01-12 11:17:03 +0530</span>
<ul start="1"><li>BUG#11757250: REPLACE(...) INSIDE A STORED PROCEDURE.
</li><li>[Revision #2661.844.6](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.6)
     <span class="cstm-style datetime">Sat 2013-01-12 11:13:37 +0530</span>
<ul start="1"><li>BUG#11757250: REPLACE(...) INSIDE A STORED PROCEDURE.
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.9](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.9) [merge]
    <span class="cstm-style datetime">Fri 2013-01-11 16:36:44 +0530</span>
<ul start="1"><li>Bug#15843818  PARTITIONING BY RANGE WITH TO_DAYS ALWAYS                INCLUDES FIRST PARTITION WHEN PRUNING
</li><li>[Revision #2661.844.5](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.5)
     <span class="cstm-style datetime">Fri 2013-01-11 16:27:37 +0530</span>
<ul start="1"><li>Bug#15843818  PARTITIONING BY RANGE WITH TO_DAYS ALWAYS                INCLUDES FIRST PARTITION WHEN PRUNING
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.8](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.8) [merge]
    <span class="cstm-style datetime">Fri 2013-01-11 06:36:53 +0530</span>
<ul start="1"><li>Merge from 5.1 to 5.5
</li><li>[Revision #2661.844.4](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.4)
     <span class="cstm-style datetime">Thu 2013-01-10 16:17:13 +0530</span>
<ul start="1"><li>Bug#11760726: LEFT JOIN OPTIMIZED INTO JOIN LEADS TO               INCORRECT RESULTS
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.7](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.7)
    <span class="cstm-style datetime">Thu 2013-01-10 16:37:20 +0530</span>
<ul start="1"><li>Bug #14553380 MYSQL C API LIBRARY EXITS AT NET_CLEAR AT NET_SERV.CC:223
</li></ul>
</li><li>[Revision #3077.184.6](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.6) [merge]
    <span class="cstm-style datetime">Thu 2013-01-10 14:37:02 +0530</span>
<ul start="1"><li>Merge from 5.1 to 5.5
</li><li>[Revision #2661.844.3](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.3)
     <span class="cstm-style datetime">Thu 2013-01-10 14:34:27 +0530</span>
<ul start="1"><li>Bug#11749556: DEBUG ASSERTION WHEN ACCESSING A VIEW AND               AVAILABLE MEMORY IS TOO LOW 
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.5](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.5)
    <span class="cstm-style datetime">Thu 2013-01-10 10:28:04 +0530</span>
<ul start="1"><li>Bug #16004999 ASSERT STATE == TRX_STATE_NOT_STARTED, UNLOCK_ROW()
</li></ul>
</li><li>[Revision #3077.184.4](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.4)
    <span class="cstm-style datetime">Thu 2013-01-10 09:00:23 +0530</span>
<ul start="1"><li>Bug#16064876    MAIN.KILL FAILS OCCASIONALLY ON SOL10 SPARC64
</li></ul>
</li><li>[Revision #3077.184.3](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.3) [merge]
    <span class="cstm-style datetime">Thu 2013-01-10 10:11:53 +1100</span>
<ul start="1"><li>Merge from mysql-5.1 to mysql-5.5.
</li><li>[Revision #2661.844.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.2)
     <span class="cstm-style datetime">Thu 2013-01-10 10:01:50 +1100</span>
<ul start="1"><li>Bug#13997024 SEGV IN SYNC_ARRAY_CELL_PRINT PRINTING OUT LONG SEMAPHORE WAIT DATA
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.2) [merge]
    <span class="cstm-style datetime">Tue 2013-01-08 13:07:39 +0100</span>
<ul start="1"><li>Empty version change upmerge
</li><li>[Revision #2661.844.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.844.1)
     <span class="cstm-style datetime">Tue 2013-01-08 12:42:36 +0100</span>
<ul start="1"><li>Raise version number after cloning 5.1.68
</li></ul>
</li></ul>
</li><li>[Revision #3077.184.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.184.1)
    <span class="cstm-style datetime">Tue 2013-01-08 07:28:41 +0100</span>
<ul start="1"><li>Raise version number after cloning 5.5.30
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.414](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.414)
   <span class="cstm-style datetime">Mon 2013-05-06 16:51:41 +0300</span>
<ul start="1"><li>If one declared several continue handler for the same condition on different level of stored procedures, all of them where executed. Now we only execute the innermost of them (the most relevant).
</li></ul>
</li><li>[Revision #3334.1.413](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.413) [merge]
   <span class="cstm-style datetime">Sun 2013-05-05 05:38:09 +0400</span>
<ul start="1"><li>[MDEV-4482](https://jira.mariadb.org/browse/MDEV-4482) fix null-merged to 5.5
</li><li>[Revision #3334.35.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.35.1) [merge]
    <span class="cstm-style datetime">Sun 2013-05-05 05:29:33 +0400</span>
<ul start="1"><li>Merge
</li><li>[Revision #2502.577.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.577.1)
     <span class="cstm-style datetime">Sun 2013-05-05 05:27:02 +0400</span>
<ul start="1"><li>[MDEV-4482](https://jira.mariadb.org/browse/MDEV-4482): main.windows test fails in buildbot with result mismatch - Rollback an earlier patch (was pushed into 5.3 instead of 5.5)
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.412](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.412) [merge]
   <span class="cstm-style datetime">Sat 2013-05-04 21:56:45 -0700</span>
<ul start="1"><li>Merge 5.3-&gt;5.5
</li><li>[Revision #2502.567.99](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.99)
    <span class="cstm-style datetime">Fri 2013-05-03 22:46:45 -0700</span>
<ul start="1"><li>Fixed bug [MDEV-4336](https://jira.mariadb.org/browse/MDEV-4336). When iterating over a list of conditions using List_iterator the function remove_eq_conds should skip all predicates that replace a condition from the list. Otherwise it can come to an infinite recursion.
</li></ul>
</li><li>[Revision #2502.567.98](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.98)
    <span class="cstm-style datetime">Fri 2013-05-03 18:45:20 -0700</span>
<ul start="1"><li>Made consistent handling of the predicates of the form &lt;non-nullable datatime field&gt; IS NULL in outer joins with that in inner joins. Previously such condition was transformed into the condition &lt;non-nullable datatime field&gt; = 0 unless the field belonged to an inner table of an outer join. In this case the predicate was interpreted as for any other field. Now if the field in the predicate &lt;non-nullable datatime field&gt; IS NULL belongs to an inner table of an outer join the predicate is  transformed into the disjunction &lt;non-nullable datatime field&gt; = 0 OR &lt;non-nullable datatime field&gt; IS NULL. This is fully compatible with the semantics of such predicates in 5.5.
</li></ul>
</li><li>[Revision #2502.567.97](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.97)
    <span class="cstm-style datetime">Mon 2013-04-29 20:31:40 -0700</span>
<ul start="1"><li>Fixed bug [MDEV-4274](https://jira.mariadb.org/browse/MDEV-4274). This bug was the result of incompleteness of the patch for bug [MDEV-4177](https://jira.mariadb.org/browse/MDEV-4177). When an OR condition is simplified to a single conjunct it is merged into the embedding AND condition. Multiple equalities are also merged, and any field item involved in those equality should acquire a pointer to a the multiple equality formed by this merge.
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.411](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.411)
   <span class="cstm-style datetime">Sat 2013-05-04 20:42:43 +0400</span>
<ul start="1"><li>[MDEV-4071](https://jira.mariadb.org/browse/MDEV-4071): Valgrind warnings 'Invalid read' in subselect_engine::calc_const_tables with ... - Call tmp_having-&gt;update_used_tables() *before* we have call JOIN::cleanup().   Making the call after join::cleanup() is not allowed, because subquery    predicate items walk parent join's JOIN_TAB structures. Which can be    invalidated by JOIN::cleanup().
</li></ul>
</li><li>[Revision #3334.1.410](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.410)
   <span class="cstm-style datetime">Sat 2013-05-04 21:02:07 +0400</span>
<ul start="1"><li>[MDEV-389](https://jira.mariadb.org/browse/MDEV-389): Wrong result (missing row) with semijoin, join_cache_level&gt;4 ... - Added testcase
</li></ul>
</li><li>[Revision #3334.1.409](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.409)
   <span class="cstm-style datetime">Sat 2013-05-04 13:05:24 +0400</span>
<ul start="1"><li>Update testcase result
</li></ul>
</li><li>[Revision #3334.1.408](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.408)
   <span class="cstm-style datetime">Sat 2013-05-04 01:08:20 +0400</span>
<ul start="1"><li>[MDEV-4270](https://jira.mariadb.org/browse/MDEV-4270): crash in fix_semijoin_strategies_for_picked_join_order - Added testcase
</li></ul>
</li><li>[Revision #3334.1.407](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.407)
   <span class="cstm-style datetime">Sat 2013-05-04 00:56:50 +0400</span>
<ul start="1"><li>[MDEV-621](https://jira.mariadb.org/browse/MDEV-621): [Bug #693329](https://bugs.launchpad.net/bugs/693329) - Assertion `!is_interleave_error' failed on low optimizer_search_depth - When restore_prev_nj_state() is called for the table that is    the last remaining child of a nested join, do not leave that   nested join's bit in join-&gt;cur_embedding_map.
</li></ul>
</li><li>[Revision #3334.1.406](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.406)
   <span class="cstm-style datetime">Fri 2013-05-03 00:10:43 +0400</span>
<ul start="1"><li>[MDEV-4465](https://jira.mariadb.org/browse/MDEV-4465): Reproducible crash (mysqld got signal 11) in multi_delete::initialize_tables... - make multi_delete::initialize_tables() take into account that the JOIN structure may have   semi-join nests (which are not fully initialized when this function is called, they have    tab-&gt;table=NULL which caused the crash) - Also checked multi_update::initialize_tables(): it has a different logic and needed no fixing.
</li></ul>
</li><li>[Revision #3334.1.405](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.405)
   <span class="cstm-style datetime">Tue 2013-04-30 00:29:47 +0200</span>
<ul start="1"><li>[MDEV-4458](https://jira.mariadb.org/browse/MDEV-4458) - Windows installer does not launch upgrade wizard anymore, even if there are upgradable instances (i.e windows service of lower MariaDB/MySQL version)
</li></ul>
</li><li>[Revision #3334.1.404](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.404)
   <span class="cstm-style datetime">Sun 2013-04-28 14:28:46 +0200</span>
<ul start="1"><li>fix test on Windows
</li></ul>
</li><li>[Revision #3334.1.403](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.403)
   <span class="cstm-style datetime">Sat 2013-04-27 23:28:48 -0700</span>
<ul start="1"><li>Fixed bug [MDEV-4340](https://jira.mariadb.org/browse/MDEV-4340). The function make_join_statistics checks whether eq_ref access uses only constant expressions, and, if this is the case the function performs constant row substitution. The code of this check must take into account hidden components of extended secondary keys.
</li></ul>
</li><li>[Revision #3334.1.402](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.402)
   <span class="cstm-style datetime">Thu 2013-04-25 15:11:59 +0200</span>
<ul start="1"><li>Fix build on Windows
</li></ul>
</li><li>[Revision #3334.1.401](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.401)
   <span class="cstm-style datetime">Thu 2013-04-25 13:16:35 +0200</span>
<ul start="1"><li>Fix unsigned/signed conversion bug in event type during mysql_binlog_send().
</li></ul>
</li><li>[Revision #3334.1.400](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.400)
   <span class="cstm-style datetime">Mon 2013-04-22 16:22:39 +0200</span>
<ul start="1"><li>[MDEV-4396](https://jira.mariadb.org/browse/MDEV-4396): Fix sporadic failure of test innodb.innodb_bug14676111
</li></ul>
</li><li>[Revision #3334.1.399](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.399)
   <span class="cstm-style datetime">Fri 2013-04-19 12:50:16 +0200</span>
<ul start="1"><li>[MDEV-260](https://jira.mariadb.org/browse/MDEV-260) auditing table accesses
</li></ul>
</li><li>[Revision #3334.1.398](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.398)
   <span class="cstm-style datetime">Fri 2013-04-19 12:08:55 +0200</span>
<ul start="1"><li>[MDEV-4398](https://jira.mariadb.org/browse/MDEV-4398) -  Change default for innodb_use_fallocate to FALSE, due to bugs in older Linux kernels (posix_fallocate() does not always guarantee that file size is like one specified)
</li></ul>
</li><li>[Revision #3334.1.397](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.397)
   <span class="cstm-style datetime">Thu 2013-04-18 22:17:29 +0200</span>
<ul start="1"><li>[MDEV-4332](https://jira.mariadb.org/browse/MDEV-4332) Increase username length from 16 characters
</li></ul>
</li><li>[Revision #3334.1.396](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.396)
   <span class="cstm-style datetime">Wed 2013-04-17 19:42:34 +0200</span>
<ul start="1"><li>strmake_buf(X,Y) helper, equivalent to strmake(X,Y,sizeof(X)-1) with a bit of lame protection against abuse.
</li></ul>
</li><li>[Revision #3334.1.395](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.395)
   <span class="cstm-style datetime">Tue 2013-04-16 18:52:23 +0200</span>
<ul start="1"><li>debug_sync is only available in debug build.
</li></ul>
</li><li>[Revision #3334.1.394](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.394)
   <span class="cstm-style datetime">Tue 2013-04-16 17:33:47 +0200</span>
<ul start="1"><li>Fix race in test case.
</li></ul>
</li><li>[Revision #3334.1.393](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.393)
   <span class="cstm-style datetime">Tue 2013-04-16 09:42:09 +0200</span>
<ul start="1"><li>[MDEV-3882](https://jira.mariadb.org/browse/MDEV-3882): .deb versions lower than upstream repo, causing install failure
</li></ul>
</li><li>[Revision #3334.1.392](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.392)
   <span class="cstm-style datetime">Sun 2013-04-14 16:48:16 +0200</span>
<ul start="1"><li>compiler warnings
</li></ul>
</li><li>[Revision #3334.1.391](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.391)
   <span class="cstm-style datetime">Sun 2013-04-14 10:00:42 +0200</span>
<ul start="1"><li>add missing tests
</li></ul>
</li><li>[Revision #3334.1.390](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.390)
   <span class="cstm-style datetime">Fri 2013-04-12 13:19:00 +0300</span>
<ul start="1"><li>Increase default value of max_binlog_cache_size and max_binlog_stmt_cache_size to ulonglong_max. This fixes that by default LOAD DATA INFILE will not generate the error: "Multi-statement transaction required more than 'max_binlog_cache_size' bytes of storage..."
</li></ul>
</li><li>[Revision #3334.1.389](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.389)
   <span class="cstm-style datetime">Fri 2013-04-12 01:05:29 +0200</span>
<ul start="1"><li>complier warnings. hide the redundant condition under #ifdef (because only there it  makes any sense)
</li></ul>
</li><li>[Revision #3334.1.388](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.388) [merge]
   <span class="cstm-style datetime">Fri 2013-04-12 01:01:18 +0200</span>
<ul start="1"><li>5.3 merge
</li><li>[Revision #2502.567.96](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.96) [merge]
    <span class="cstm-style datetime">Thu 2013-04-11 19:35:39 +0200</span>
<ul start="1"><li>5.2 merge
</li><li>[Revision #2502.566.45](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.45) [merge]
     <span class="cstm-style datetime">Thu 2013-04-11 19:30:59 +0200</span>
<ul start="1"><li>5.1 merge
</li><li>[Revision #2502.565.45](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.45)
      <span class="cstm-style datetime">Sat 2013-04-06 21:29:12 +0200</span>
<ul start="1"><li>[MDEV-4244](https://jira.mariadb.org/browse/MDEV-4244) [PATCH] Buffer overruns and use-after-free errors
</li></ul>
</li><li>[Revision #2502.565.44](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.44)
      <span class="cstm-style datetime">Thu 2013-04-04 11:35:10 +0200</span>
<ul start="1"><li>[MDEV-4088](https://jira.mariadb.org/browse/MDEV-4088) Replication 10.0 -&gt; 5.5 fails
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #2502.567.95](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.95)
    <span class="cstm-style datetime">Sat 2013-04-06 15:51:08 +0200</span>
<ul start="1"><li>[MDEV-4244](https://jira.mariadb.org/browse/MDEV-4244) [PATCH] Buffer overruns and use-after-free errors
</li></ul>
</li><li>[Revision #2502.567.94](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.94)
    <span class="cstm-style datetime">Sat 2013-04-06 15:14:46 +0200</span>
<ul start="1"><li>[MDEV-4316](https://jira.mariadb.org/browse/MDEV-4316) MariaDB server crash with signal 11
</li></ul>
</li><li>[Revision #2502.567.93](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.93)
    <span class="cstm-style datetime">Mon 2013-04-08 12:04:28 +0300</span>
<ul start="1"><li>If a range tree has a branch that is an expensive constant, currently get_mm_tree skipped the evaluation of this constant and icorrectly proceeded. The correct behavior is to return a NULL subtree, according to the IF branch being fixed - when it evaluates the constant it returns a value, and doesn't continue further.
</li></ul>
</li><li>[Revision #2502.567.92](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.92)
    <span class="cstm-style datetime">Thu 2013-04-04 12:34:31 +0400</span>
<ul start="1"><li>Update tests results, mysql-test/r/windows.result
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.387](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.387)
   <span class="cstm-style datetime">Sun 2013-04-07 20:32:39 +0200</span>
<ul start="1"><li>[MDEV-4356](https://jira.mariadb.org/browse/MDEV-4356) : MariaDB does not start if bind-address gets resolved to more than single IP address.
</li></ul>
</li><li>[Revision #3334.1.386](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.386)
   <span class="cstm-style datetime">Sat 2013-04-06 00:36:10 +0200</span>
<ul start="1"><li>[MDEV-4338](https://jira.mariadb.org/browse/MDEV-4338) - Support FusionIO/directFS atomic writes
</li></ul>
</li><li>[Revision #3334.1.385](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.385)
   <span class="cstm-style datetime">Sat 2013-04-06 00:35:45 +0200</span>
<ul start="1"><li>[MDEV-4338](https://jira.mariadb.org/browse/MDEV-4338) - Support FusionIO/directFS atomic writes
</li></ul>
</li><li>[Revision #3334.1.384](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.384)
   <span class="cstm-style datetime">Thu 2013-04-04 11:37:23 +0200</span>
<ul start="1"><li>compilation warnings
</li></ul>
</li><li>[Revision #3334.1.383](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.383)
   <span class="cstm-style datetime">Thu 2013-04-04 11:37:13 +0200</span>
<ul start="1"><li>fix have_debug_sync.inc to be more robust (debug_sync value can have single quotes)
</li></ul>
</li><li>[Revision #3334.1.382](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.382)
   <span class="cstm-style datetime">Thu 2013-04-04 11:05:04 +0200</span>
<ul start="1"><li>[MDEV-4161](https://jira.mariadb.org/browse/MDEV-4161) Assertion `status_var.memory_used == 0' fails in virtual THD::THD()
</li></ul>
</li><li>[Revision #3334.1.381](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.381)
   <span class="cstm-style datetime">Thu 2013-03-28 20:04:14 +0100</span>
<ul start="1"><li>[MDEV-4243](https://jira.mariadb.org/browse/MDEV-4243) Warnings/errors while compiling with clang
</li></ul>
</li><li>[Revision #3334.1.380](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.380) [merge]
   <span class="cstm-style datetime">Wed 2013-04-03 18:51:29 +0400</span>
<ul start="1"><li>Merge 5.3 -&gt; 5.5
</li><li>[Revision #2502.567.91](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.91)
    <span class="cstm-style datetime">Mon 2013-04-01 18:03:14 +0400</span>
<ul start="1"><li>[MDEV-4240](https://jira.mariadb.org/browse/MDEV-4240): [mariadb 5.3.12](/kb/en/mariadb-5312-release-notes/) using more memory than MySQL 5.1 for an inefficient query - Let index_merge allocate table handlers on quick select's MEM_ROOT,   not on statement's MEM_ROOT.    This is crucial for big "range checked for each record" queries, where    index_merge can be created and deleted many times during query exection.    We should not make O(#rows) allocations on statement's MEM_ROOT.
</li></ul>
</li><li>[Revision #2502.567.90](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.90)
    <span class="cstm-style datetime">Fri 2013-03-29 19:27:06 +0400</span>
<ul start="1"><li>[MDEV-4335](https://jira.mariadb.org/browse/MDEV-4335): Unexpected results when selecting on information_schema - When converting a subquery to a semi-join, propagate OPTION_SCHEMA_TABLE.
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.379](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.379)
   <span class="cstm-style datetime">Fri 2013-03-29 17:53:21 +0200</span>
<ul start="1"><li>Fix for [MDEV-4144](https://jira.mariadb.org/browse/MDEV-4144)
</li></ul>
</li><li>[Revision #3334.1.378](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.378)
   <span class="cstm-style datetime">Fri 2013-03-29 14:56:09 +0100</span>
<ul start="1"><li>[MDEV-4243](https://jira.mariadb.org/browse/MDEV-4243) : remove several clang warnings.
</li></ul>
</li><li>[Revision #3334.1.377](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.377) [merge]
   <span class="cstm-style datetime">Thu 2013-03-28 19:18:36 -0700</span>
<ul start="1"><li>Merge 5.3-&gt;5.5.
</li><li>[Revision #2502.567.89](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.89) [merge]
    <span class="cstm-style datetime">Wed 2013-03-27 08:58:16 -0700</span>
<ul start="1"><li>Merge.
</li><li>[Revision #2502.576.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.576.1)
     <span class="cstm-style datetime">Fri 2013-03-22 21:33:06 -0700</span>
<ul start="1"><li>Fixed bug [MDEV-4318](https://jira.mariadb.org/browse/MDEV-4318). In some cases, when using views the optimizer incorrectly determined possible join orders for queries with nested outer and inner joins. This could lead to invalid execution plans for such queries.
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.376](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.376) [merge]
   <span class="cstm-style datetime">Wed 2013-03-27 22:22:52 -0700</span>
<ul start="1"><li>Merge
</li><li>[Revision #3334.34.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.34.1)
    <span class="cstm-style datetime">Wed 2013-03-27 19:17:32 -0700</span>
<ul start="1"><li>Fixed bug [MDEV-4311](https://jira.mariadb.org/browse/MDEV-4311) (bug #68749). This bug was introduced by the patch for [WL#3220](http://askmonty.org/worklog/?tid=3220). If the memory allocated for the tree to store unique elements to be counted is not big enough to include all of them then an external file is used to store the elements. The unique elements are guaranteed not to be nulls. So, when  reading them from the file we don't have to care about the null flags of the read values. However, we should remove the flag  at the very beginning of the process. If we don't do it and if the last value written into the record buffer for the field whose distinct values needs to be counted happens to be null, then all values read from the file  are considered to be nulls and are not counted in. The fix does not remove a possible null flag for the read values. Rather it just counts the values in the same way it was done before WL #3220.
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.375](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.375) [merge]
   <span class="cstm-style datetime">Wed 2013-03-27 10:03:28 +0100</span>
<ul start="1"><li>5.3 merge
</li><li>[Revision #2502.567.88](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.88) [merge]
    <span class="cstm-style datetime">Tue 2013-03-26 19:09:47 +0100</span>
<ul start="1"><li>5.2 merge
</li><li>[Revision #2502.566.44](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.44) [merge]
     <span class="cstm-style datetime">Tue 2013-03-26 17:39:45 +0100</span>
<ul start="1"><li>5.1 merge
</li><li>[Revision #2502.565.43](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.43)
      <span class="cstm-style datetime">Wed 2013-03-20 21:20:51 +0100</span>
<ul start="1"><li>add 'plugins' suite - empty, but the line <code class="fixed" style="white-space:pre-wrap">./mtr --suite=main,plugins</code> will work on all branches.
</li></ul>
</li><li>[Revision #2502.565.42](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.42)
      <span class="cstm-style datetime">Tue 2013-03-19 17:25:58 +0400</span>
<ul start="1"><li>[MDEV-4295](https://jira.mariadb.org/browse/MDEV-4295) Server crashes in get_point on a query with Area, AsBinary, MultiPoint.         Need to check if the number of points is 0 for the polygon.
</li></ul>
</li><li>[Revision #2502.565.41](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.41)
      <span class="cstm-style datetime">Tue 2013-03-19 17:16:10 +0400</span>
<ul start="1"><li>[MDEV-4296](https://jira.mariadb.org/browse/MDEV-4296) Assertion `n_linear_rings &gt; 0' fails in Gis_polygon::centroid_xy.         Forgotten DBUG_ASSERT should be replaced with the 'return error'.
</li></ul>
</li><li>[Revision #2502.565.40](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.40)
      <span class="cstm-style datetime">Mon 2013-03-18 15:07:52 +0200</span>
<ul start="1"><li>[MDEV-4269](https://jira.mariadb.org/browse/MDEV-4269) fix. Item_default_value inherited form Item_field so should create temporary table field similary.
</li></ul>
</li><li>[Revision #2502.565.39](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.39)
      <span class="cstm-style datetime">Mon 2013-03-18 17:58:00 +0400</span>
<ul start="1"><li>[MDEV-4252](https://jira.mariadb.org/browse/MDEV-4252) geometry query crashes server.     Additional fixes for possible overflows in length-related     calculations in 'spatial' implementations.     Checks added to the ::get_data_size() methods.     max_n_points decreased to occupy less 2G size. An     object of that size is practically inoperable anyway.
</li></ul>
</li><li>[Revision #2502.565.38](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.38)
      <span class="cstm-style datetime">Mon 2013-03-18 10:35:03 +0100</span>
<ul start="1"><li>[MDEV-4289](https://jira.mariadb.org/browse/MDEV-4289) Assertion `0' fails in make_sortkey with GROUP_CONCAT, MAKE_SET, GROUP BY
</li></ul>
</li><li>[Revision #2502.565.37](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.37)
      <span class="cstm-style datetime">Sun 2013-03-10 23:08:05 +0400</span>
<ul start="1"><li>[MDEV-4252](https://jira.mariadb.org/browse/MDEV-4252) geometry query crashes server.       The bug was found by Alyssa Milburn.       If the number of points of a geometry feature read from       binary representation is greater than 0x10000000, then       the (uint32) (num_points * 16) will cut the higher byte,       which leads to various errors.       Fixed by additional check if (num_points &gt; max_n_points).
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #2502.567.87](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.87)
    <span class="cstm-style datetime">Tue 2013-03-26 21:47:06 +0400</span>
<ul start="1"><li>GEOMETRYCOLLECTION EMPTY handling fixed.           The get_mbr() method shouldn't return the error, rather an invalid MBR           in this case.
</li></ul>
</li><li>[Revision #2502.567.86](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.86)
    <span class="cstm-style datetime">Tue 2013-03-26 13:07:46 +0200</span>
<ul start="1"><li>[MDEV-4292](https://jira.mariadb.org/browse/MDEV-4292) fix.
</li></ul>
</li><li>[Revision #2502.567.85](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.85)
    <span class="cstm-style datetime">Fri 2013-03-22 17:32:27 +0400</span>
<ul start="1"><li>[MDEV-4310](https://jira.mariadb.org/browse/MDEV-4310) geometry function equals hangs forever.         The Geometry::get_mbr() function can return an error on         a bad data. We have to check for that and act respectively.
</li></ul>
</li><li>[Revision #2502.567.84](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.84) [merge]
    <span class="cstm-style datetime">Thu 2013-03-21 11:07:38 +0400</span>
<ul start="1"><li>Merge
</li><li>[Revision #2502.575.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.575.1)
     <span class="cstm-style datetime">Thu 2013-03-21 11:06:27 +0400</span>
<ul start="1"><li>[MDEV-4277](https://jira.mariadb.org/browse/MDEV-4277): Crash inside mi_killed_in_mariadb() with myisammrg - Set MI_INFO::external_ref for MyISAM tables that are parts of myisamMRG table.
</li></ul>
</li></ul>
</li><li>[Revision #2502.567.83](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.83)
    <span class="cstm-style datetime">Wed 2013-03-20 16:13:00 +0100</span>
<ul start="1"><li>[MDEV-4293](https://jira.mariadb.org/browse/MDEV-4293) Valgrind warnings (Conditional jump or move depends on uninitialised value) in remove_eq_conds on time functions with NULL argument
</li></ul>
</li><li>[Revision #2502.567.82](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.82)
    <span class="cstm-style datetime">Mon 2013-03-18 08:44:24 +0100</span>
<ul start="1"><li>[MDEV-4283](https://jira.mariadb.org/browse/MDEV-4283) Assertion `scale &lt;= precision' fails in strings/decimal.c
</li></ul>
</li><li>[Revision #2502.567.81](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.81)
    <span class="cstm-style datetime">Sun 2013-03-17 17:44:15 +0100</span>
<ul start="1"><li>[MDEV-4286](https://jira.mariadb.org/browse/MDEV-4286) Server crashes in Protocol_text::store, stack smashing detected
</li></ul>
</li><li>[Revision #2502.567.80](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.80)
    <span class="cstm-style datetime">Sun 2013-03-17 07:41:22 +0100</span>
<ul start="1"><li>[MDEV-4281](https://jira.mariadb.org/browse/MDEV-4281) Assertion `maybe_null &amp;&amp; item-&gt;null_value' fails in make_sortkey on CASE with different return types, GROUP_CONCAT, GROUP BY
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.374](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.374)
   <span class="cstm-style datetime">Tue 2013-03-26 19:17:26 +0100</span>
<ul start="1"><li>[MDEV-4307](https://jira.mariadb.org/browse/MDEV-4307) Support at least 48 utf8 characters in username in server and PAM
</li></ul>
</li><li>[Revision #3334.1.373](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.373)
   <span class="cstm-style datetime">Tue 2013-03-26 17:57:36 +0100</span>
<ul start="1"><li>fix @@external_user variable
</li></ul>
</li><li>[Revision #3334.1.372](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.372)
   <span class="cstm-style datetime">Mon 2013-03-25 16:38:00 +0100</span>
<ul start="1"><li>fixes for windows
</li></ul>
</li><li>[Revision #3334.1.371](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.371)
   <span class="cstm-style datetime">Wed 2013-03-20 20:56:14 +0100</span>
<ul start="1"><li>[MDEV-249](https://jira.mariadb.org/browse/MDEV-249) QUERY CACHE INFORMATION
</li></ul>
</li><li>[Revision #3334.1.370](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.370)
   <span class="cstm-style datetime">Tue 2013-03-19 15:25:58 +0100</span>
<ul start="1"><li>extend check_global_access() to avoid my_error when it's not needed (in INFORMATION_SCHEMA).
</li></ul>
</li><li>[Revision #3334.1.369](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.369)
   <span class="cstm-style datetime">Tue 2013-03-26 10:34:21 +0100</span>
<ul start="1"><li>Fixes for Windows XP
</li></ul>
</li><li>[Revision #3334.1.368](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.368)
   <span class="cstm-style datetime">Tue 2013-03-26 08:17:22 +0100</span>
<ul start="1"><li>[MDEV-4330](https://jira.mariadb.org/browse/MDEV-4330) - get_tty_password() does not work if input redirection is used.
</li></ul>
</li><li>[Revision #3334.1.367](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.367)
   <span class="cstm-style datetime">Mon 2013-03-25 16:45:24 +0200</span>
<ul start="1"><li>Patch by Ian Good for [MDEV-4319](https://jira.mariadb.org/browse/MDEV-4319): mysqlbinlog output ambiguous escaping
</li></ul>
</li><li>[Revision #3334.1.366](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.366)
   <span class="cstm-style datetime">Sun 2013-03-17 11:41:25 +0100</span>
<ul start="1"><li>[MDEV-4284](https://jira.mariadb.org/browse/MDEV-4284) Assertion `cmp_items[(uint)cmp_type]' fails in sql/item_cmpfunc.cc
</li></ul>
</li><li>[Revision #3334.1.365](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.365)
   <span class="cstm-style datetime">Thu 2013-03-14 19:07:20 +0200</span>
<ul start="1"><li>[MDEV-4272](https://jira.mariadb.org/browse/MDEV-4272) fix.
</li></ul>
</li><li>[Revision #3334.1.364](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.364)
   <span class="cstm-style datetime">Thu 2013-03-14 18:39:22 +0200</span>
<ul start="1"><li>OPTION is now a valid identifier (not a reserved word)
</li></ul>
</li><li>[Revision #3334.1.363](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.363)
   <span class="cstm-style datetime">Thu 2013-03-14 16:52:20 +0400</span>
<ul start="1"><li>[MDEV-4214](https://jira.mariadb.org/browse/MDEV-4214) : main.partition_rename_longfilename fails on eCryptFS   Adding an include file which checks whether long names are supported
</li></ul>
</li><li>[Revision #3334.1.362](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.362)
   <span class="cstm-style datetime">Wed 2013-03-13 22:33:52 +0100</span>
<ul start="1"><li>[MDEV-4265](https://jira.mariadb.org/browse/MDEV-4265) 5.5 is slower than 5.3 because of many str_to_datetime calls
</li></ul>
</li><li>[Revision #3334.1.361](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.361)
   <span class="cstm-style datetime">Mon 2013-03-11 21:00:08 +0100</span>
<ul start="1"><li>fix innodb failures on solaris
</li></ul>
</li><li>[Revision #3334.1.360](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.360)
   <span class="cstm-style datetime">Tue 2013-03-12 21:06:46 +0100</span>
<ul start="1"><li>Fix clang warning (suggest parentheses)
</li></ul>
</li><li>[Revision #3334.1.359](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.359)
   <span class="cstm-style datetime">Tue 2013-03-12 20:11:05 +0100</span>
<ul start="1"><li>[MDEV-4267](https://jira.mariadb.org/browse/MDEV-4267) : do not copy sql_yacc.cc and sql_yacc.h from unpacked source tarball into build directory, if usable bison is installed on the build machine.
</li></ul>
</li><li>[Revision #3334.1.358](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.358)
   <span class="cstm-style datetime">Tue 2013-03-12 20:09:49 +0100</span>
<ul start="1"><li>[MDEV-4224](https://jira.mariadb.org/browse/MDEV-4224) : func_math test fails, when clang 3.0  compiler is used.
</li></ul>
</li><li>[Revision #3334.1.357](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.357)
   <span class="cstm-style datetime">Wed 2013-03-06 13:30:40 +0100</span>
<ul start="1"><li>[MDEV-4249](https://jira.mariadb.org/browse/MDEV-4249) : when autodetecting default client charset on Windows, fallback to GetACP() whenever GetConsoleCP() returns 0 (i.e appkication does not have a console , which is the case for GUI apps, Windows services etc)
</li></ul>
</li></ul>
- [Revision #3394](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3394)
  <span class="cstm-style datetime">Sat 2013-05-25 12:22:57 +0300</span>
<ul start="1"><li>References: [MDEV-4572](https://jira.mariadb.org/browse/MDEV-4572) - merge with lp:codership-mysql/5.5-23 revisions 3858..3867
</li></ul>
- [Revision #3393](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3393) [merge]
  <span class="cstm-style datetime">Fri 2013-05-24 15:29:01 +0300</span>
<ul start="1"><li>References: [MDEV-4572](https://jira.mariadb.org/browse/MDEV-4572) - merge with [mariaDB 5.5.30](/kb/en/mariadb-5530-release-notes/)
</li><li>[Revision #3334.1.356](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.356)
   <span class="cstm-style datetime">Mon 2013-03-11 13:50:17 +0400</span>
<ul start="1"><li>The i386 specific code improving character set conversion on the ASCII range was not enabled on x86_64 machines. Enabling it. Gives up to 18 times conversion performance improvement.
</li></ul>
</li><li>[Revision #3334.1.355](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.355) [merge]
   <span class="cstm-style datetime">Sun 2013-03-10 12:46:56 +0100</span>
<ul start="1"><li>5.3-&gt;5.5 merge
</li><li>[Revision #2502.567.79](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.79)
    <span class="cstm-style datetime">Fri 2013-03-08 00:25:26 -0800</span>
<ul start="1"><li>Fixed bug [MDEV-4250](https://jira.mariadb.org/browse/MDEV-4250). This is a bug in the legacy code. It did not manifest itself because it was masked by other bugs that were fixed by the patches for [MDEV-4172](https://jira.mariadb.org/browse/MDEV-4172) and [MDEV-4177](https://jira.mariadb.org/browse/MDEV-4177).
</li></ul>
</li><li>[Revision #2502.567.78](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.78)
    <span class="cstm-style datetime">Wed 2013-03-06 22:22:24 +0100</span>
<ul start="1"><li>Fix typo (clang issued warning that =+ was used where += was intended)
</li></ul>
</li><li>[Revision #2502.567.77](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.77)
    <span class="cstm-style datetime">Wed 2013-03-06 21:10:58 +0200</span>
<ul start="1"><li>[MDEV-4241](https://jira.mariadb.org/browse/MDEV-4241) fix.
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.354](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.354)
   <span class="cstm-style datetime">Fri 2013-03-08 19:09:45 +0100</span>
<ul start="1"><li>[MDEV-4186](https://jira.mariadb.org/browse/MDEV-4186) Test case main.myisampack fails on ppc32 (only)
</li></ul>
</li><li>[Revision #3334.1.353](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.353)
   <span class="cstm-style datetime">Fri 2013-03-08 19:09:15 +0100</span>
<ul start="1"><li>[MDEV-4175](https://jira.mariadb.org/browse/MDEV-4175) auth_socket to build on OpenBSD / Bitrig
</li></ul>
</li><li>[Revision #3334.1.352](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.352) [merge]
   <span class="cstm-style datetime">Fri 2013-03-08 19:08:45 +0100</span>
<ul start="1"><li>merge with XtraDB as of Percona-Server-5.5.30-rel30.1
</li><li>[Revision #0.12.61](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/0.12.61)
    <span class="cstm-style datetime">Fri 2013-03-08 13:13:46 +0100</span>
<ul start="1"><li>Percona-Server-5.5.30-rel30.1.tar.gz
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.351](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.351)
   <span class="cstm-style datetime">Wed 2013-03-06 09:38:08 +0100</span>
<ul start="1"><li>hack in dependencies to imitate mysql-*.rpm even better
</li></ul>
</li><li>[Revision #3334.1.350](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.350)
   <span class="cstm-style datetime">Wed 2013-03-06 09:32:13 +0100</span>
<ul start="1"><li>[MDEV-4068](https://jira.mariadb.org/browse/MDEV-4068) rpm scriptlet chown command dangerous
</li></ul>
</li><li>[Revision #3334.1.349](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.349)
   <span class="cstm-style datetime">Tue 2013-03-05 17:49:37 +0100</span>
<ul start="1"><li>[MDEV-4066](https://jira.mariadb.org/browse/MDEV-4066) semisync_master + temporary tables causes memory leaks
</li></ul>
</li><li>[Revision #3334.1.348](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.348)
   <span class="cstm-style datetime">Wed 2013-03-06 01:45:25 +0400</span>
<ul start="1"><li>TODO-424 geometry query crashes server.         The bug was found by Alyssa Milburn.         If the number of points of a geometry feature read from         binary representation is greater than 0x10000000, then         the (uint32) (num_points * 16) will cut the higher byte,         which leads to various errors.         Fixed by additional check if (num_points &gt; max_n_points).
</li></ul>
</li><li>[Revision #3334.1.347](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.347)
   <span class="cstm-style datetime">Tue 2013-03-05 20:15:36 +0200</span>
<ul start="1"><li>Fix for assert found by mysql-test-run
</li></ul>
</li><li>[Revision #3334.1.346](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.346)
   <span class="cstm-style datetime">Tue 2013-03-05 00:53:18 +0200</span>
<ul start="1"><li>Fixed issue with LOCK TABLE + ALTER TABLE ENABLE KEYS + SHOW commands.
</li></ul>
</li><li>[Revision #3334.1.345](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.345)
   <span class="cstm-style datetime">Mon 2013-03-04 12:49:35 +0100</span>
<ul start="1"><li>Fix wrong install location for DEB supportfiles.
</li></ul>
</li><li>[Revision #3334.1.344](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.344) [merge]
   <span class="cstm-style datetime">Sat 2013-03-02 14:04:11 -0800</span>
<ul start="1"><li>Merge
</li><li>[Revision #3334.33.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.33.1)
    <span class="cstm-style datetime">Sat 2013-03-02 12:36:32 -0800</span>
<ul start="1"><li>Fixed bug [MDEV-4220](https://jira.mariadb.org/browse/MDEV-4220). This bug is a regression bug. The regression was introduced by the patch for [MDEV-3851](https://jira.mariadb.org/browse/MDEV-3851), that tried to weaken the condition when a ref access with an extended key can be converted to an eq_ref access. The patch incorrectly formed this condition. As a result, while improving performance for some queries, the patch caused  worse performance for another queries.
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.343](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.343)
   <span class="cstm-style datetime">Fri 2013-03-01 20:58:19 +0100</span>
<ul start="1"><li>[MDEV-4216](https://jira.mariadb.org/browse/MDEV-4216) : export additional functions  mysql_get_timeout_value(),mysql_get_timeout_value_ms(),  mysql_get_socket() from shared client library. They are documented as part of async API.
</li></ul>
</li><li>[Revision #3334.1.342](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.342)
   <span class="cstm-style datetime">Fri 2013-03-01 11:36:15 -0500</span>
<ul start="1"><li>Removed the obsolete instructions from the MySQL 5.1 manual. Instead provide a link to [https://kb.askmonty.org/en/compiling-mariadb-from-source/](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/)
</li></ul>
</li><li>[Revision #3334.1.341](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.341) [merge]
   <span class="cstm-style datetime">Fri 2013-03-01 18:09:06 +0200</span>
<ul start="1"><li>Automatic merge
</li><li>[Revision #3334.32.3](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.32.3)
    <span class="cstm-style datetime">Fri 2013-03-01 18:01:44 +0200</span>
<ul start="1"><li>Fixed bug MPDEV-628 / [Bug #989055](https://bugs.launchpad.net/bugs/989055) - Querying myisam table metadata may corrupt the table.
</li></ul>
</li><li>[Revision #3334.32.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.32.2)
    <span class="cstm-style datetime">Thu 2013-02-28 16:47:03 +0200</span>
<ul start="1"><li>Added test case for bug in replace with replication that existed in MySQL 5.1: Replace with an auto_increment primary key and another unique key didn't replicate correctly with REPLACE
</li></ul>
</li><li>[Revision #3334.32.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.32.1)
    <span class="cstm-style datetime">Thu 2013-02-28 08:42:05 +0200</span>
<ul start="1"><li>Added support for <code class="fixed" style="white-space:pre-wrap">--crash-script</code> in mysqld_safe. Trivial cleanup
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.340](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.340)
   <span class="cstm-style datetime">Fri 2013-03-01 14:58:19 +0100</span>
<ul start="1"><li>Fix compile error when building with DBUG, but without DEBUG_SYNC.
</li></ul>
</li><li>[Revision #3334.1.339](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.339) [merge]
   <span class="cstm-style datetime">Fri 2013-03-01 11:44:10 +0400</span>
<ul start="1"><li>Merge 5.3-&gt;5.5
</li><li>[Revision #2502.567.76](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.76)
    <span class="cstm-style datetime">Fri 2013-03-01 08:23:35 +0400</span>
<ul start="1"><li>Fix compile error on windows in fix for [MDEV-4177](https://jira.mariadb.org/browse/MDEV-4177).
</li></ul>
</li><li>[Revision #2502.567.75](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.75) [merge]
    <span class="cstm-style datetime">Thu 2013-02-28 17:09:56 -0800</span>
<ul start="1"><li>Merge
</li><li>[Revision #2502.574.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.574.1)
     <span class="cstm-style datetime">Thu 2013-02-28 14:35:46 -0800</span>
<ul start="1"><li>Fixed bug [MDEV-4209](https://jira.mariadb.org/browse/MDEV-4209) Do not include BLOB fields into the key to access the temporary table created for a materialized view/derived table. BLOB components are not allowed in keys.
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.338](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.338) [merge]
   <span class="cstm-style datetime">Thu 2013-02-28 23:56:17 +0100</span>
<ul start="1"><li>merge with XtraDB as of Percona-Server-5.5.29-rel30.0
</li><li>[Revision #0.12.60](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/0.12.60)
    <span class="cstm-style datetime">Thu 2013-02-28 22:23:45 +0100</span>
<ul start="1"><li>Percona-Server-5.5.29-rel30.0.tar.gz
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.337](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.337) [merge]
   <span class="cstm-style datetime">Thu 2013-02-28 22:47:29 +0100</span>
<ul start="1"><li>5.3-&gt;5.5 merge
</li><li>[Revision #2502.567.74](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.74) [merge]
    <span class="cstm-style datetime">Thu 2013-02-28 21:48:47 +0100</span>
<ul start="1"><li>5.2 -&gt; 5.3
</li><li>[Revision #2502.566.43](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.566.43) [merge]
     <span class="cstm-style datetime">Thu 2013-02-28 19:00:58 +0100</span>
<ul start="1"><li>5.1 -&gt; 5.2 merge
</li><li>[Revision #2502.565.36](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.36)
      <span class="cstm-style datetime">Thu 2013-02-28 11:46:35 +0100</span>
<ul start="1"><li>a simpler fix for MySQL Bug #12408412: GROUP_CONCAT + ORDER BY + INPUT/OUTPUT SAME USER VARIABLE = CRASH and MySQL Bug#14664077 SEVERE PERFORMANCE DEGRADATION IN SOME CASES WHEN USER VARIABLES ARE USED
</li></ul>
</li><li>[Revision #2502.565.35](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.35)
      <span class="cstm-style datetime">Thu 2013-02-28 10:00:07 +0100</span>
<ul start="1"><li>Fixed BUG#51763 Can't delete rows from MEMORY table with HASH key
</li></ul>
</li><li>[Revision #2502.565.34](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.34) [merge]
      <span class="cstm-style datetime">Thu 2013-02-28 09:58:39 +0100</span>
<ul start="1"><li>mysql-5.1 merge
</li><li>[Revision #2661.838.56](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.56)
       <span class="cstm-style datetime">Tue 2013-01-08 13:29:11 +0100</span>
<ul start="1"><li>Applying patch for Bug#67177 Bug#15967374 from Kent
</li></ul>
</li></ul>
</li><li>[Revision #2502.565.33](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.33)
      <span class="cstm-style datetime">Tue 2013-02-26 21:20:15 +0100</span>
<ul start="1"><li>[MDEV-4203](https://jira.mariadb.org/browse/MDEV-4203) : fix maria SE repair functions (wrong operator precedence)
</li></ul>
</li><li>[Revision #2502.565.32](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.32)
      <span class="cstm-style datetime">Thu 2013-02-21 23:20:26 +0100</span>
<ul start="1"><li>[MDEV-4194](https://jira.mariadb.org/browse/MDEV-4194): Fix typo (missing comma) in mysys error messages
</li></ul>
</li><li>[Revision #2502.565.31](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.31)
      <span class="cstm-style datetime">Thu 2013-02-14 16:27:55 +0400</span>
<ul start="1"><li>[MDEV-4169](https://jira.mariadb.org/browse/MDEV-4169): mysql-test-run doesn't strip expected warnings (setrlimit)
</li></ul>
</li><li>[Revision #2502.565.30](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.565.30)
      <span class="cstm-style datetime">Fri 2013-02-01 00:09:36 +0200</span>
<ul start="1"><li>Fix bug [MDEV-641](https://jira.mariadb.org/browse/MDEV-641)
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #2502.567.73](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.73)
    <span class="cstm-style datetime">Thu 2013-02-28 09:55:35 -0800</span>
<ul start="1"><li>Fixed a compile error for some platform.
</li></ul>
</li><li>[Revision #2502.567.72](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.72)
    <span class="cstm-style datetime">Sun 2013-02-24 19:16:11 -0800</span>
<ul start="1"><li>Fixed bug [MDEV-4177](https://jira.mariadb.org/browse/MDEV-4177) The function remove_eq_cond removes the parts of a disjunction for which it has been proved that they are always true. In the result of this removal the disjunction may be converted into a  formula without OR that must be merged into the the AND formula that contains the disjunction. The merging of two AND conditions must take into account the multiple equalities that may be part of each of them. These multiple equality must be merged and become part of the and object built as the result of the merge of the AND conditions. Erroneously the function remove_eq_cond lacked the code that  would merge multiple equalities of the merged AND conditions. This could lead to confusing situations when at the same AND  level there were two multiple equalities with common members and the list of equal items contained only some of these  multiple equalities. This, in its turn, could lead to an incorrect work of the function substitute_for_best_equal_field when it tried to optimize ref accesses. This resulted in forming invalid TABLE_REF objects that were used to build look-up keys when materialized subqueries were exploited.
</li></ul>
</li><li>[Revision #2502.567.71](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.71)
    <span class="cstm-style datetime">Thu 2013-02-21 17:13:12 -0800</span>
<ul start="1"><li>Fixed bug [MDEV-4172](https://jira.mariadb.org/browse/MDEV-4172). This bug in the legacy code could manifest itself in queries with semi-join materialized subqueries. When a subquery is materialized all conditions that are imposed only on the columns belonging to the tables from the subquery  are taken into account.The code responsible for subquery optimizations that employes subquery materialization  makes sure to remove these conditions from the WHERE conditions of the query obtained after it has transformed the original query into a query with a semi-join. If the condition to be removed is an equality condition it could be added to ON expressions and/or conditions from disjunctive branches (parts of OR conditions) in an attempt to generate better access keys to the tables of the query. Such equalities are supposed to be removed later from all the formulas where they have been added to. However, erroneously, this was not done in some cases when an ON expression and/or a disjunctive part of the OR condition could be converted into one multiple equality. As a result some equality predicates over columns belonging to the tables of the materialized subquery remained in the ON condition and/or the a disjunctive  part of the OR condition, and the excuter later, when trying to evaluate them, returned wrong answers as the values of the fields from these equalities were not valid.   This happened because any standalone multiple equality (a multiple equality that are not ANDed with any other predicates) lacked the information about equality predicates inherited from upper levels (in particular, inherited from the WHERE condition). The fix adds a reference to such information to any standalone multiple equality.
</li></ul>
</li><li>[Revision #2502.567.70](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.70) [merge]
    <span class="cstm-style datetime">Wed 2013-02-20 19:22:02 -0800</span>
<ul start="1"><li>Merge.
</li><li>[Revision #2502.573.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.573.1)
     <span class="cstm-style datetime">Wed 2013-02-20 18:01:36 -0800</span>
<ul start="1"><li>Fixed bug [MDEV-3913](https://jira.mariadb.org/browse/MDEV-3913). The wrong result set returned by the left join query  from the bug test case happened due to several inconsistencies  and bugs of the legacy mysql code.
</li></ul>
</li></ul>
</li><li>[Revision #2502.567.69](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.69)
    <span class="cstm-style datetime">Wed 2013-02-13 11:58:16 +0200</span>
<ul start="1"><li>Fix for [MDEV-4140](https://jira.mariadb.org/browse/MDEV-4140)
</li></ul>
</li><li>[Revision #2502.567.68](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.68) [merge]
    <span class="cstm-style datetime">Tue 2013-02-12 11:49:46 -0800</span>
<ul start="1"><li>Merge.
</li><li>[Revision #2502.572.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.572.1)
     <span class="cstm-style datetime">Thu 2013-02-07 21:46:02 -0800</span>
<ul start="1"><li>Fixed bug [MDEV-3995](https://jira.mariadb.org/browse/MDEV-3995). This bug happened because the executor tried to use a wrong TABLE REF object when building access keys. It constructed keys from fields of a materialized table from a ref object created to construct keys from the fields of the underlying base table. This could happen only when materialized table was created for a non-correlated IN subquery and only when the materialized table used for lookups. In this case we are guaranteed to be able to construct the keys from the fields of tables that would be outer tables for the tables of the IN subquery. The patch makes sure that no ref objects constructed from fields of materialized lookup tables are to be used.
</li></ul>
</li></ul>
</li><li>[Revision #2502.567.67](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.67)
    <span class="cstm-style datetime">Mon 2013-02-11 10:55:58 +0200</span>
<ul start="1"><li>[MDEV-4123](https://jira.mariadb.org/browse/MDEV-4123) fix.
</li></ul>
</li><li>[Revision #2502.567.66](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2502.567.66)
    <span class="cstm-style datetime">Mon 2013-02-04 17:35:48 +0200</span>
<ul start="1"><li>Fix for bug [MDEV-765](https://jira.mariadb.org/browse/MDEV-765) ([Bug #825075](https://bugs.launchpad.net/bugs/825075))
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.336](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.336)
   <span class="cstm-style datetime">Thu 2013-02-28 20:19:53 +0100</span>
<ul start="1"><li>revert
</li></ul>
</li><li>[Revision #3334.1.335](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.335) [merge]
   <span class="cstm-style datetime">Thu 2013-02-28 18:42:49 +0100</span>
<ul start="1"><li>merge with mysql-5.5.30 minus few incorrect or not applicable changesets
</li><li>[Revision #3077.175.115](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.115)
    <span class="cstm-style datetime">Wed 2013-01-16 08:09:26 +0100</span>
<ul start="1"><li>Removed Conflicts: mysql-libs mysql-libs-advanced from spec file
</li></ul>
</li><li>[Revision #3077.175.114](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.114)
    <span class="cstm-style datetime">Tue 2013-01-15 09:56:36 +0100</span>
<ul start="1"><li>A bit more intelligent processing of .in files in mysql-test/collections
</li></ul>
</li><li>[Revision #3077.175.113](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.113)
    <span class="cstm-style datetime">Tue 2013-01-15 08:52:38 +0100</span>
<ul start="1"><li>Fix for Bug#14636211 WRONG RESULT (EXTRA ROW) ON A FROM SUBQUERY                       WITH A VARIABLE AND ORDER BY         Bug#16035412 MYSQL SERVER 5.5.29 WRONG SORTING USING COMPLEX INDEX
</li></ul>
</li><li>[Revision #3077.175.112](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.112) [merge]
    <span class="cstm-style datetime">Mon 2013-01-07 16:58:02 +0530</span>
<ul start="1"><li>Null Merge mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.838.55](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.55)
     <span class="cstm-style datetime">Mon 2013-01-07 16:56:16 +0530</span>
<ul start="1"><li>Post Fix to Bug#14628410 - ASSERTION `! IS_SET()' FAILED IN 			    DIAGNOSTICS_AREA::SET_OK_STATUS
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.111](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.111) [merge]
    <span class="cstm-style datetime">Mon 2013-01-07 16:19:06 +0530</span>
<ul start="1"><li>Merge of patch for Bug#16066243 from mysql-5.1.
</li><li>[Revision #2661.838.54](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.54)
     <span class="cstm-style datetime">Mon 2013-01-07 16:16:08 +0530</span>
<ul start="1"><li>Bug#16066243 PB2 FAILURES I_MAIN.BUG15912213 AND       I_MAIN.CTYPE_UTF8 FOR MACOSX10.6 FOR 5.1
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.110](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.110) [merge]
    <span class="cstm-style datetime">Fri 2013-01-04 17:34:02 +0530</span>
<ul start="1"><li>Merge Post Fix for BUG#14628410 from mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.838.53](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.53)
     <span class="cstm-style datetime">Fri 2013-01-04 17:30:39 +0530</span>
<ul start="1"><li>Post Fix to Bug#14628410 - ASSERTION `! IS_SET()' FAILED IN 			    DIAGNOSTICS_AREA::SET_OK_STATUS
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.109](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.109) [merge]
    <span class="cstm-style datetime">Fri 2013-01-04 16:42:49 +0530</span>
<ul start="1"><li>Merge of patch for bug#16066243 from mysql-5.1.
</li><li>[Revision #2661.838.52](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.52)
     <span class="cstm-style datetime">Fri 2013-01-04 16:38:12 +0530</span>
<ul start="1"><li>Bug#16066243 PB2 FAILURES I_MAIN.BUG15912213 AND     I_MAIN.CTYPE_UTF8 FOR MACOSX10.6 FOR 5.1
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.108](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.108)
    <span class="cstm-style datetime">Fri 2013-01-04 11:48:11 +0530</span>
</li><li>[Revision #3077.175.107](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.107)
    <span class="cstm-style datetime">Thu 2013-01-03 19:19:28 +0530</span>
</li><li>[Revision #3077.175.106](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.106) [merge]
    <span class="cstm-style datetime">Wed 2013-01-02 18:32:38 +0530</span>
<ul start="1"><li>BUG#11753923-SQL THREAD CRASHES ON DISK FULL Merging fix from mysql-5.1
</li><li>[Revision #2661.838.51](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.51)
     <span class="cstm-style datetime">Wed 2013-01-02 16:31:58 +0530</span>
<ul start="1"><li>BUG#11753923-SQL THREAD CRASHES ON DISK FULL Problem:If Disk becomes full while writing into the binlog, then the server instance hangs till someone frees the space. After user frees up the disk space, mysql server crashes with an assert (m_status != DA_EMPTY)
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.105](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.105)
    <span class="cstm-style datetime">Wed 2013-01-02 11:00:55 +0100</span>
<ul start="1"><li>Bug#16060864 SEGMENTATION FAULT IN PERFORMANCE_SCHEMA WITH HISTORY SIZE 0
</li></ul>
</li><li>[Revision #3077.175.104](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.104)
    <span class="cstm-style datetime">Wed 2013-01-02 06:18:27 +0100</span>
<ul start="1"><li>Updated Windows MSI package copyright year to 2013
</li></ul>
</li><li>[Revision #3077.175.103](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.103) [merge]
    <span class="cstm-style datetime">Tue 2013-01-01 03:36:10 +0100</span>
<ul start="1"><li>Updated README and client executables copyright year to 2013
</li><li>[Revision #2661.838.50](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.50)
     <span class="cstm-style datetime">Tue 2013-01-01 03:33:40 +0100</span>
<ul start="1"><li>Updated README and client executables copyright year to 2013
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.102](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.102) [merge]
    <span class="cstm-style datetime">Sat 2012-12-29 23:49:11 +0530</span>
<ul start="1"><li>[Revision #2661.838.49](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.49)
     <span class="cstm-style datetime">Sat 2012-12-29 23:46:31 +0530</span>
</li></ul>
</li><li>[Revision #3077.175.101](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.101) [merge]
    <span class="cstm-style datetime">Fri 2012-12-28 16:21:07 +0530</span>
<ul start="1"><li>BUG#14726272- BACKPORT FIX FOR BUG 11746142 TO 5.5 AND 5.1 Merging fix from mysql-5.1
</li><li>[Revision #2661.838.48](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.48)
     <span class="cstm-style datetime">Fri 2012-12-28 16:13:48 +0530</span>
<ul start="1"><li>BUG#14726272- BACKPORT FIX FOR BUG 11746142 TO 5.5 AND 5.1 Details of BUG#11746142: CALLING MYSQLD WHILE ANOTHER  INSTANCE IS RUNNING, REMOVES PID FILE Fix: Before removing the pid file, ensure it was created by the same process, leave it intact otherwise.
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.100](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.100) [merge]
    <span class="cstm-style datetime">Thu 2012-12-27 17:36:11 +0530</span>
<ul start="1"><li>Merge of patch for Bug#16046140 from mysql-5.1.
</li><li>[Revision #2661.838.47](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.47)
     <span class="cstm-style datetime">Thu 2012-12-27 17:33:34 +0530</span>
<ul start="1"><li>Bug#16046140 BIN/MYSQLD_SAFE: TEST: ARGUMENT EXPECTED
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.99](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.99) [merge]
    <span class="cstm-style datetime">Thu 2012-12-27 02:43:20 +0100</span>
<ul start="1"><li>merge
</li><li>[Revision #2661.838.46](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.46)
     <span class="cstm-style datetime">Thu 2012-12-27 02:27:00 +0100</span>
<ul start="1"><li>Bug#14589559 Post push fix for valgrind warnings.
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.98](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.98) [merge]
    <span class="cstm-style datetime">Wed 2012-12-26 20:28:10 +0530</span>
<ul start="1"><li>Merge from 5.1 to 5.5
</li><li>[Revision #2661.838.45](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.45)
     <span class="cstm-style datetime">Wed 2012-12-26 20:21:19 +0530</span>
<ul start="1"><li>Bug#12347040: MEMORY LEAK IN CONVERT_TZ COULD POSSIBLY CAUSE                     DOS ATTACKS
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.97](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.97) [merge]
    <span class="cstm-style datetime">Wed 2012-12-26 12:45:46 +0100</span>
<ul start="1"><li>Upmerge of the 5.1.67 build
</li><li>[Revision #2661.838.44](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.44) [merge]
     <span class="cstm-style datetime">Wed 2012-12-26 12:42:47 +0100</span>
<ul start="1"><li>Merge from mysql-5.1.67-release
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.96](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.96) [merge]
    <span class="cstm-style datetime">Mon 2012-12-24 16:51:23 +0530</span>
<ul start="1"><li>Null merge from mysql-5.1 to mysql-5.5.
</li><li>[Revision #2661.838.43](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.43)
     <span class="cstm-style datetime">Mon 2012-12-24 16:49:42 +0530</span>
<ul start="1"><li>Fixing a pb2 issue.  There is some difference in the output in my local machine and pb2 machines in the explain output.
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.95](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.95) [merge]
    <span class="cstm-style datetime">Mon 2012-12-24 06:42:02 +0530</span>
<ul start="1"><li>Merge from 5.1
</li><li>[Revision #2661.838.42](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.42)
     <span class="cstm-style datetime">Mon 2012-12-24 06:39:54 +0530</span>
<ul start="1"><li>Bug#11757005: UNION CONVERTS UNSIGNED MEDIUMINT AND BIGINT               TO SIGNED Problem: When we are joining types (of fields) in case of a union, we usually upgrade the datatypes to the largest present in the query. In case of mediumint, it is not happening. Analysis: When joined with types LONG and LONGLONG, mediumint should get upgraded to LONG and LONGLONG respectively. W.r.t the given query, constant '1' will be created as a LONGLONG internally and SIGNED flag is enabled. As a result, while combining types for the field, LONGLONG along with MEDIUMINT gets converted to LONG first. LONG with MEDIUMINT(of the third select) gets converted to MEDIUMINT. SIGNED FLAG would be that of the first field's. As a result, the final result would be SIGNED MEDIUMINT. Fix: While joining types, MEDIUMINT with LONGLONG and MEDIUMINT with LONG is converted to LONGLONG and LONG respectively. Also, made some  changes for FLOAT and DOUBLE.
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.94](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.94) [merge]
    <span class="cstm-style datetime">Fri 2012-12-21 10:26:26 +0100</span>
<ul start="1"><li>merge 5.1 =&gt; 5.5
</li><li>[Revision #2661.838.41](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.41)
     <span class="cstm-style datetime">Thu 2012-12-20 10:56:09 +0100</span>
<ul start="1"><li>Bug#16027468 ADDRESSSANITIZER BUG IN MYSQLTEST
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.93](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.93)
    <span class="cstm-style datetime">Fri 2012-12-21 09:53:42 +0100</span>
<ul start="1"><li>Bug#15972635: Incorrect results returned in 32 table join with HAVING
</li></ul>
</li><li>[Revision #3077.175.92](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.92) [merge]
    <span class="cstm-style datetime">Fri 2012-12-21 11:07:05 +0530</span>
<ul start="1"><li>Bug#14627287 THREAD CACHE - BYPASSES PRIVILEGES merge from 5.1
</li><li>[Revision #2661.838.40](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.40)
     <span class="cstm-style datetime">Fri 2012-12-21 11:04:49 +0530</span>
<ul start="1"><li>Bug#14627287 THREAD CACHE - BYPASSES PRIVILEGES
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.91](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.91)
    <span class="cstm-style datetime">Thu 2012-12-20 19:26:20 +0530</span>
<ul start="1"><li>Bug #13819630 ARCHIVE TABLE WITH 1000+ PARTITIONS CRASHES SERVER  ON "DROP TABLE"
</li></ul>
</li><li>[Revision #3077.175.90](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.90)
    <span class="cstm-style datetime">Thu 2012-12-20 11:59:36 +0530</span>
<ul start="1"><li>Bug #14556349 RENAME OF COMPRESSED TABLE AND INSERT BUFFER MERGE CAUSE HANG
</li></ul>
</li><li>[Revision #3077.175.89](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.89) [merge]
    <span class="cstm-style datetime">Wed 2012-12-19 13:25:07 +0100</span>
<ul start="1"><li>Merge from mysql-5.5.29-release
</li></ul>
</li><li>[Revision #3077.175.88](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.88)
    <span class="cstm-style datetime">Wed 2012-12-19 13:46:00 +0200</span>
<ul start="1"><li>Fix Bug#16021177 DICT_LOAD_FOREIGNS() PASSES UNALIGNED MEMORY TO DTUPLE_CREATE_FROM_MEM()
</li></ul>
</li><li>[Revision #3077.175.87](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.87) [merge]
    <span class="cstm-style datetime">Tue 2012-12-18 21:02:56 +0200</span>
<ul start="1"><li>Merge mysql-5.1 -&gt; mysql-5.5
</li><li>[Revision #2661.838.39](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.39)
     <span class="cstm-style datetime">Tue 2012-12-18 20:55:30 +0200</span>
<ul start="1"><li>Fix Bug#16000909 MEMORY LEAK, MYSQL_INPLACE_ALTER_TABLE
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.86](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.86) [merge]
    <span class="cstm-style datetime">Tue 2012-12-18 22:16:12 +0530</span>
<ul start="1"><li>BUG#14727815 - CRASH IN PTHREAD_RWLOCK_WRLOCK/SRW_UNLOCK                              IN QUERY CACHE CODE
</li><li>[Revision #2661.838.38](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.38)
     <span class="cstm-style datetime">Tue 2012-12-18 22:12:56 +0530</span>
<ul start="1"><li>BUG#14727815 - CRASH IN PTHREAD_RWLOCK_WRLOCK/SRW_UNLOCK                              IN QUERY CACHE CODE
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.85](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.85) [merge]
    <span class="cstm-style datetime">Tue 2012-12-18 16:52:58 +0200</span>
<ul start="1"><li>Merge mysql-5.1 -&gt; mysql-5.5
</li><li>[Revision #2661.838.37](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.37)
     <span class="cstm-style datetime">Tue 2012-12-18 16:51:41 +0200</span>
<ul start="1"><li>Fix Bug#13463493 INNODB PLUGIN WERE CHANGED, BUT STILL USE THE SAME VERSION NUMBER 1.0.17
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.84](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.84)
    <span class="cstm-style datetime">Mon 2012-12-10 09:55:08 +0100</span>
<ul start="1"><li>Bug#15960005 VALGRIND WARNINGS IN PROCESS_ARGS
</li></ul>
</li><li>[Revision #3077.175.83](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.83)
    <span class="cstm-style datetime">Mon 2012-12-17 23:13:46 +0800</span>
<ul start="1"><li>Approved by Jimmy and Inaam. rb#1576
</li></ul>
</li><li>[Revision #3077.175.82](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.82) [merge]
    <span class="cstm-style datetime">Fri 2012-12-14 14:01:43 +0400</span>
<ul start="1"><li>Auto-merge from mysql-5.1.
</li><li>[Revision #2661.838.36](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.36)
     <span class="cstm-style datetime">Fri 2012-12-14 13:55:30 +0400</span>
<ul start="1"><li>Fix for BUG#15948580 UPDATE_XML() CRASHES THE SERVER.
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.81](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.81) [merge]
    <span class="cstm-style datetime">Fri 2012-12-14 11:29:07 +0500</span>
<ul start="1"><li>merge from 5.1
</li><li>[Revision #2661.838.35](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.35)
     <span class="cstm-style datetime">Fri 2012-12-14 11:24:57 +0500</span>
<ul start="1"><li>Bug#14329288 IS THE CALL TO IBUF_MERGE_OR_DELETE_FOR_PAGE FROM BUF_PAGE_GET_GEN REDUNDANT?
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.80](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.80) [merge]
    <span class="cstm-style datetime">Thu 2012-12-13 20:58:09 +0530</span>
<ul start="1"><li>Merging from 5.1 to 5.5 for bug#11761752
</li><li>[Revision #2661.838.34](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.34)
     <span class="cstm-style datetime">Thu 2012-12-13 20:33:44 +0530</span>
<ul start="1"><li>bug#11761752: DO NOT ALLOW USE OF ALTERNATE DATA STREAMS ON NTFS FILESYSTEM.
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.79](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.79)
    <span class="cstm-style datetime">Thu 2012-12-13 17:12:21 +0200</span>
<ul start="1"><li>Follow-up fix to Bug#14628410: Remove the Windows InnoDB Plugin specific implementation of innobase_mysql_tmpfile() from MySQL 5.5 onwards.
</li></ul>
</li><li>[Revision #3077.175.78](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.78) [merge]
    <span class="cstm-style datetime">Thu 2012-12-13 18:56:47 +0530</span>
<ul start="1"><li>Merge fix for Bug#14628410 from mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.838.33](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.33)
     <span class="cstm-style datetime">Thu 2012-12-13 18:53:16 +0530</span>
<ul start="1"><li>Bug#14628410 - ASSERTION `! IS_SET()' FAILED IN DIAGNOSTICS_AREA::SET_OK_STATUS
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.77](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.77) [merge]
    <span class="cstm-style datetime">Thu 2012-12-13 10:19:14 +0530</span>
<ul start="1"><li>Bug#15965288: BUFFER OVERFLOW IN YASSL FUNCTION               DOPROCESSREPLY()
</li><li>[Revision #2661.838.32](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.32)
     <span class="cstm-style datetime">Thu 2012-12-13 10:17:26 +0530</span>
<ul start="1"><li>Bug#15965288: BUFFER OVERFLOW IN YASSL FUNCTION               DOPROCESSREPLY()
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.76](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.76)
    <span class="cstm-style datetime">Wed 2012-12-12 22:31:03 +0530</span>
<ul start="1"><li>Bug#13639125 DELIMITER STRIPS THE NEXT NEW LINE              IN A SQL STATEMENT
</li></ul>
</li><li>[Revision #3077.175.75](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.75) [merge]
    <span class="cstm-style datetime">Wed 2012-12-12 15:10:47 +0530</span>
<ul start="1"><li>upmerge 14737171 5.1=&gt;5.5
</li><li>[Revision #2661.838.31](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.31)
     <span class="cstm-style datetime">Wed 2012-12-12 15:09:31 +0530</span>
<ul start="1"><li>Bug #14737171:MTR DOES NOT PRESERVE TEST CASE LOGS ON RETRY-FAIL
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.74](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.74) [merge]
    <span class="cstm-style datetime">Tue 2012-12-11 22:04:30 +0400</span>
<ul start="1"><li>Bug #15954872 "MAKE MDL SUBSYSTEM AND TABLE DEFINITION CACHE ROBUST AGAINST BUGS IN CALLERS".
</li><li>[Revision #2661.838.30](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.30)
     <span class="cstm-style datetime">Tue 2012-12-11 22:00:51 +0400</span>
<ul start="1"><li>Bug #15954872 "MAKE MDL SUBSYSTEM AND TABLE DEFINITION CACHE  ROBUST AGAINST BUGS IN CALLERS".
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.73](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.73) [merge]
    <span class="cstm-style datetime">Tue 2012-12-11 18:35:52 +0530</span>
<ul start="1"><li>upmerge 14737171 5.1 =&gt; 5.5
</li><li>[Revision #2661.838.29](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.29)
     <span class="cstm-style datetime">Tue 2012-12-11 18:34:04 +0530</span>
<ul start="1"><li>Bug #14737171: MTR DOES NOT PRESERVE TEST CASE LOGS ON RETRY-FAIL
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.72](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.72) [merge]
    <span class="cstm-style datetime">Tue 2012-12-11 11:30:58 +0100</span>
<ul start="1"><li>Merge ULN RPM stuff to main branch.
</li><li>[Revision #3077.183.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.183.1)
     <span class="cstm-style datetime">Mon 2012-12-10 09:42:18 +0100</span>
<ul start="1"><li>RPMs for ULN do not build in MySQL 5.6: Patches + libmysqld.so Bug #15972480
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.71](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.71) [merge]
    <span class="cstm-style datetime">Tue 2012-12-11 10:51:24 +0530</span>
<ul start="1"><li>Merging from mysql-5.1 to mysql-5.5.
</li><li>[Revision #2661.838.28](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.28)
     <span class="cstm-style datetime">Tue 2012-12-11 10:11:24 +0530</span>
<ul start="1"><li>Bug #14200010 NEWLY CREATED TABLE DOESN'T ALLOW FOR LOOSE INDEX SCANS
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.70](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.70) [merge]
    <span class="cstm-style datetime">Sun 2012-12-09 17:26:44 +0530</span>
<ul start="1"><li>BUG#12359942 - REPLICATION TEST FROM ENGINE SUITE RPL_ROW_UNTIL TIMES OUT
</li><li>[Revision #2661.838.27](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.27) [merge]
     <span class="cstm-style datetime">Sun 2012-12-09 17:21:51 +0530</span>
<ul start="1"><li>BUG#12359942 - REPLICATION TEST FROM ENGINE SUITE PL_ROW_UNTIL TIMES OUT
</li><li>[Revision #2661.843.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.843.1)
      <span class="cstm-style datetime">Sun 2012-12-09 15:50:32 +0530</span>
<ul start="1"><li>BUG#12359942 - REPLICATION TEST FROM ENGINE SUITE                 RPL_ROW_UNTIL TIMES OUT
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.69](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.69)
    <span class="cstm-style datetime">Fri 2012-12-07 18:26:02 +0530</span>
<ul start="1"><li>Bug #15930494 	MYSQLDUMP TEST SOMETIMES FAILS DUE TO MIXING STDOUT AND STDERR
</li></ul>
</li><li>[Revision #3077.175.68](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.68)
    <span class="cstm-style datetime">Fri 2012-12-07 15:41:49 +0530</span>
</li><li>[Revision #3077.175.67](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.67)
    <span class="cstm-style datetime">Thu 2012-12-06 16:53:02 +0530</span>
<ul start="1"><li>Bug#15912213: BUFFER OVERFLOW IN ACL_GET()
</li></ul>
</li><li>[Revision #3077.175.66](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.66)
    <span class="cstm-style datetime">Thu 2012-12-06 15:59:15 +0600</span>
<ul start="1"><li>This patch fixes bug#14729757 - MY_HASH_SEARCH(&amp;XID_CACHE,                                 XID_STATE-&gt;XID.KEY(),                                 XID_STATE-&gt;XID.KEY_LENGTH())==0
</li></ul>
</li><li>[Revision #3077.175.65](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.65)
    <span class="cstm-style datetime">Wed 2012-12-05 20:41:29 +0400</span>
<ul start="1"><li>Bug #15948123: SERVER WORKS INCORRECT WITH LONG TABLE ALIASES
</li></ul>
</li><li>[Revision #3077.175.64](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.64) [merge]
    <span class="cstm-style datetime">Wed 2012-12-05 19:50:02 +0400</span>
<ul start="1"><li>Merged fix for bug #15954896 "SP, MULTI-TABLE DELETE AND LONG ALIAS" into 5.5 tree.
</li><li>[Revision #2661.838.26](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.26)
     <span class="cstm-style datetime">Wed 2012-12-05 19:26:56 +0400</span>
<ul start="1"><li>Bug #15954896 "SP, MULTI-TABLE DELETE AND LONG ALIAS".
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.63](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.63)
    <span class="cstm-style datetime">Wed 2012-12-05 16:16:32 +0100</span>
</li><li>[Revision #3077.175.62](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.62)
    <span class="cstm-style datetime">Wed 2012-12-05 15:14:08 +0100</span>
<ul start="1"><li>Remove moot <code class="fixed" style="white-space:pre-wrap">--unit-test</code> option for mtr in collections
</li></ul>
</li><li>[Revision #3077.175.61](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.61)
    <span class="cstm-style datetime">Wed 2012-12-05 16:53:33 +0400</span>
<ul start="1"><li>Bug #15948123: SERVER WORKS INCORRECT WITH LONG TABLE ALIASES
</li></ul>
</li><li>[Revision #3077.175.60](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.60) [merge]
    <span class="cstm-style datetime">Wed 2012-12-05 10:24:45 +0530</span>
<ul start="1"><li>BUG#12359942 - REPLICATION TEST FROM ENGINE SUITE RPL_ROW_UNTIL TIMES OUT
</li><li>[Revision #2661.838.25](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.25) [merge]
     <span class="cstm-style datetime">Wed 2012-12-05 10:17:53 +0530</span>
<ul start="1"><li>BUG#12359942 - REPLICATION TEST FROM ENGINE SUITE RPL_ROW_UNTIL TIMES OUT
</li><li>[Revision #2661.842.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.842.1)
      <span class="cstm-style datetime">Fri 2012-11-30 12:12:33 +0530</span>
<ul start="1"><li>BUG#12359942 - REPLICATION TEST FROM ENGINE SUITE RPL_ROW_UNTIL TIMES OUT
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.59](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.59)
    <span class="cstm-style datetime">Tue 2012-12-04 16:09:48 +0000</span>
<ul start="1"><li>Bug#13545447 RPL_ROTATE_LOGS FAILS DUE TO CONCURRENCY ISSUES IN REP. CODE
</li></ul>
</li><li>[Revision #3077.175.58](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.58)
    <span class="cstm-style datetime">Tue 2012-12-04 18:14:01 +0530</span>
<ul start="1"><li>BUG#13812374 - RPL.RPL_REPORT_PORT FAILS OCCASIONALLY ON PB2
</li></ul>
</li><li>[Revision #3077.175.57](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.57)
    <span class="cstm-style datetime">Tue 2012-12-04 12:32:58 +0900</span>
<ul start="1"><li>UNIV_DEBUG build of some environments needs #include "read0read.h" for srv0srv.c and trx0rec.c. This is only for mysql-5.5
</li></ul>
</li><li>[Revision #3077.175.56](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.56) [merge]
    <span class="cstm-style datetime">Sat 2012-12-01 09:17:56 +0100</span>
<ul start="1"><li>merge mysql-5.1 -&gt; mysql-5.5
</li><li>[Revision #2661.838.24](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.24) [merge]
     <span class="cstm-style datetime">Sat 2012-12-01 09:07:03 +0100</span>
<ul start="1"><li>merge of bug#14589559 into mysql-5.1
</li><li>[Revision #2661.841.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.841.2)
      <span class="cstm-style datetime">Fri 2012-11-30 16:17:38 +0100</span>
<ul start="1"><li>bug#14589559: ASSERTION `FILE_ENTRY_BUF[2] == 0' FAILED                            IN DEACTIVATE_DDL_LOG_ENTRY
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.55](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.55) [merge]
    <span class="cstm-style datetime">Sat 2012-12-01 09:12:05 +0100</span>
<ul start="1"><li>merge of bug#14589559 to mysql-5.5
</li><li>[Revision #3077.182.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.182.1) [merge]
     <span class="cstm-style datetime">Thu 2012-10-18 12:27:02 +0200</span>
<ul start="1"><li>Manual merge of bug#14589559 from mysql-5.1 to 5.5
</li><li>[Revision #2661.841.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.841.1)
      <span class="cstm-style datetime">Thu 2012-10-18 11:59:47 +0200</span>
<ul start="1"><li>Bug#14589559: ASSERTION `FILE_ENTRY_BUF[2] == 0' FAILED IN DEACTIVATE_DDL_LOG_ENTRY
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.54](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.54) [merge]
    <span class="cstm-style datetime">Sat 2012-12-01 08:06:45 +0800</span>
<ul start="1"><li>Auto Merge
</li><li>[Revision #2661.838.23](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.23)
     <span class="cstm-style datetime">Sat 2012-12-01 08:04:33 +0800</span>
<ul start="1"><li>Bug#11764602 ASSERTION IN FORMAT_DESCRIPTION_LOG_EVENT::CALC_SERVER_VERSION_SPLIT
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.53](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.53) [merge]
    <span class="cstm-style datetime">Fri 2012-11-30 16:28:58 +0500</span>
<ul start="1"><li>merge from 5.1
</li><li>[Revision #2661.838.22](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.22)
     <span class="cstm-style datetime">Fri 2012-11-30 16:19:30 +0500</span>
<ul start="1"><li>Reverting fix for bug#14329288
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.52](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.52)
    <span class="cstm-style datetime">Thu 2012-11-29 17:21:36 +0100</span>
<ul start="1"><li>Bug#11754279 SIGNIFICANT INACCURACY IN DECIMAL MULTIPLICATION CALCULATIONS
</li></ul>
</li><li>[Revision #3077.175.51](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.51)
    <span class="cstm-style datetime">Thu 2012-11-29 17:33:06 +0530</span>
<ul start="1"><li>BUG#15888454: SLAVE CRASHES WHEN DML REQUIRES CONVERSION &amp; TABLE HAS  LESS COLUMNS THAN MASTER
</li></ul>
</li><li>[Revision #3077.175.50](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.50) [merge]
    <span class="cstm-style datetime">Thu 2012-11-29 17:24:15 +0530</span>
<ul start="1"><li>Bug#15912213: BUFFER OVERFLOW IN ACL_GET()
</li><li>[Revision #2661.838.21](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.21)
     <span class="cstm-style datetime">Thu 2012-11-29 17:23:23 +0530</span>
<ul start="1"><li>Bug#15912213: BUFFER OVERFLOW IN ACL_GET()
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.49](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.49)
    <span class="cstm-style datetime">Wed 2012-11-28 19:01:59 +0530</span>
</li><li>[Revision #3077.175.48](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.48) [merge]
    <span class="cstm-style datetime">Wed 2012-11-28 17:07:02 +0900</span>
<ul start="1"><li>Bug#59354 : Bug #12659252 : ASSERT !OTHER_LOCK AT LOCK_REC_ADD_TO_QUEUE DURING A DELETE OPERATION The converted implicit lock should wait for the prior conflicting lock if found.
</li><li>[Revision #2661.838.20](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.20)
     <span class="cstm-style datetime">Wed 2012-11-28 17:05:23 +0900</span>
<ul start="1"><li>Bug#59354 : Bug #12659252 : ASSERT !OTHER_LOCK AT LOCK_REC_ADD_TO_QUEUE DURING A DELETE OPERATION The converted implicit lock should wait for the prior conflicting lock if found.
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.47](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.47) [merge]
    <span class="cstm-style datetime">Wed 2012-11-28 09:03:37 +0200</span>
<ul start="1"><li>Merge mysql-5.1 to mysql-5.5.
</li><li>[Revision #2661.838.19](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.19)
     <span class="cstm-style datetime">Wed 2012-11-28 09:00:24 +0200</span>
<ul start="1"><li>Bug#14329288 IS THE CALL TO IBUF_MERGE_OR_DELETE_FOR_PAGE FROM BUF_PAGE_GET_GEN REDUNDANT?
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.46](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.46)
    <span class="cstm-style datetime">Mon 2012-11-26 15:14:26 +0100</span>
<ul start="1"><li>Reinstate install of mysql-test/lib/My/SafeProcess/Base.pm, removed by mistake
</li></ul>
</li><li>[Revision #3077.175.45](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.45) [merge]
    <span class="cstm-style datetime">Mon 2012-11-26 16:16:49 +0530</span>
<ul start="1"><li>upmerge 5.1 =&gt; 5.5
</li><li>[Revision #2661.838.18](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.18)
     <span class="cstm-style datetime">Mon 2012-11-26 16:09:18 +0530</span>
<ul start="1"><li>Bug #14757120 - SAFE_PROCESS.CC/SAFE_PROCESS.PL SHOULD NOT KILL MYSQLD ON SIGSTOP/SIGCONT
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.44](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.44)
    <span class="cstm-style datetime">Mon 2012-11-26 15:16:23 +0530</span>
<ul start="1"><li>Added new line at the end to resolve PB2 per push errors.
</li></ul>
</li><li>[Revision #3077.175.43](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.43) [merge]
    <span class="cstm-style datetime">Mon 2012-11-26 15:59:35 +0900</span>
<ul start="1"><li>Bug #14676249 : ROW_VERS_IMPL_X_LOCKED_LOW() MIGHT HIT !BPAGE-&gt;FILE_PAGE_WAS_FREED ASSERTION  trx_undo_prev_version_build() should confirm existence of inherited (not-own) external pages.
</li><li>[Revision #2661.838.17](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.17)
     <span class="cstm-style datetime">Mon 2012-11-26 15:57:26 +0900</span>
<ul start="1"><li>Bug #14676249 : ROW_VERS_IMPL_X_LOCKED_LOW() MIGHT HIT !BPAGE-&gt;FILE_PAGE_WAS_FREED ASSERTION  trx_undo_prev_version_build() should confirm existence of inherited (not-own) external pages.
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.42](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.42) [merge]
    <span class="cstm-style datetime">Wed 2012-11-21 19:14:15 +0530</span>
<ul start="1"><li>Bug#15883127: PORT FIX FOR BUG #13904906 TO MYSQL 5.1
</li><li>[Revision #2661.838.16](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.16)
     <span class="cstm-style datetime">Wed 2012-11-21 19:12:20 +0530</span>
<ul start="1"><li>Bug#15883127: PORT FIX FOR BUG #13904906 TO MYSQL 5.1
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.41](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.41)
    <span class="cstm-style datetime">Tue 2012-11-20 12:37:23 +0000</span>
<ul start="1"><li>BUG#15891524: RLI_FAKE MODE IS NOT UNSET AFTER BINLOG REPLAY
</li></ul>
</li><li>[Revision #3077.175.40](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.40)
    <span class="cstm-style datetime">Tue 2012-11-20 15:33:45 +0530</span>
<ul start="1"><li>BUG #15895810 - REQUIRE ADDITIONAL INFORMATION WITH THE <code class="fixed" style="white-space:pre-wrap">--RESULT-FILE</code> OPTION OF MTR
</li></ul>
</li><li>[Revision #3077.175.39](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.39)
    <span class="cstm-style datetime">Tue 2012-11-20 11:53:54 +0530</span>
<ul start="1"><li>Bug#14463669 FAILURE TO CORRECTLY PARSE ROUTINES IN                    MYSQLDUMP OUTPUT
</li></ul>
</li><li>[Revision #3077.175.38](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.38)
    <span class="cstm-style datetime">Tue 2012-11-20 11:30:39 +0530</span>
<ul start="1"><li>Bug#14463669 FAILURE TO CORRECTLY PARSE ROUTINES IN              MYSQLDUMP OUTPUT
</li></ul>
</li><li>[Revision #3077.175.37](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.37)
    <span class="cstm-style datetime">Mon 2012-11-19 21:41:35 +0530</span>
<ul start="1"><li>Bug#14463669 FAILURE TO CORRECTLY PARSE ROUTINES IN              MYSQLDUMP OUTPUT
</li></ul>
</li><li>[Revision #3077.175.36](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.36)
    <span class="cstm-style datetime">Mon 2012-11-19 14:58:51 +0530</span>
<ul start="1"><li>Bug#14147491 - INFINITE LOOP WHEN OPENING A CORRUPTED TABLE
</li></ul>
</li><li>[Revision #3077.175.35](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.35) [merge]
    <span class="cstm-style datetime">Fri 2012-11-16 15:26:56 +0100</span>
<ul start="1"><li>Merge
</li><li>[Revision #3077.181.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.181.1) [merge]
     <span class="cstm-style datetime">Fri 2012-11-16 15:20:53 +0100</span>
<ul start="1"><li>Merge
</li><li>[Revision #3077.180.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.180.1) [merge]
      <span class="cstm-style datetime">Fri 2012-11-16 14:32:37 +0100</span>
<ul start="1"><li>Merge
</li><li>[Revision #3077.179.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.179.1)
       <span class="cstm-style datetime">Thu 2012-11-15 15:52:51 +0100</span>
<ul start="1"><li>remove usage of <code class="fixed" style="white-space:pre-wrap">--skip-ndb</code> from collections  - no need to use <code class="fixed" style="white-space:pre-wrap">--skip-ndb</code> in collections files anymore, since long but   more clear logic after recent mtr.pl fixes. ndb tests are never run in MySQL Server   unless explicitly requested  - remove sys_vars.ndb_log_update_as_write_basic.test and sys_vars.ndb_log_updated_only_basic.result since MySQL Server does not have those   options.  - Only sys_vars.have_ndbcluster_basic left since MySQL Server has that variable hardcoded.
</li></ul>
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.34](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.34) [merge]
    <span class="cstm-style datetime">Fri 2012-11-16 09:06:27 -0500</span>
<ul start="1"><li>merge from 5.1
</li><li>[Revision #2661.838.15](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.15)
     <span class="cstm-style datetime">Fri 2012-11-16 09:02:35 -0500</span>
<ul start="1"><li>Bug#15859402 INNODB_BUFFER_POOL_READ_AHEAD_EVICTED IS INACCURATE
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.33](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.33) [merge]
    <span class="cstm-style datetime">Fri 2012-11-16 13:09:44 +0100</span>
<ul start="1"><li>merge
</li><li>[Revision #2661.838.14](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.14)
     <span class="cstm-style datetime">Tue 2012-11-13 09:21:59 +0100</span>
<ul start="1"><li>Bug#14845133: 
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.32](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.32) [merge]
    <span class="cstm-style datetime">Fri 2012-11-16 13:08:07 +0100</span>
<ul start="1"><li>merge
</li><li>[Revision #3077.178.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.178.1) [merge]
     <span class="cstm-style datetime">Tue 2012-11-13 14:47:49 +0100</span>
<ul start="1"><li>manual merge of bug#14845133 mysql-5.1 -&gt; mysql-5.5
</li><li>[Revision #2661.840.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.840.1)
      <span class="cstm-style datetime">Tue 2012-11-13 09:21:59 +0100</span>
<ul start="1"><li>Bug#14845133: 
</li></ul>
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.31](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.31) [merge]
    <span class="cstm-style datetime">Fri 2012-11-16 14:47:32 +0530</span>
<ul start="1"><li>Bug#11753779: MAX_CONNECT_ERRORS WORKS ONLY WHEN 1ST               INC_HOST_ERRORS() IS CALLED.
</li><li>[Revision #2661.838.13](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.13)
     <span class="cstm-style datetime">Fri 2012-11-16 14:43:10 +0530</span>
</li></ul>
</li><li>[Revision #3077.175.30](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.30) [merge]
    <span class="cstm-style datetime">Thu 2012-11-15 22:11:03 +0200</span>
<ul start="1"><li>Merge mysql-5.1 to mysql-5.5.
</li><li>[Revision #2661.838.12](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.12)
     <span class="cstm-style datetime">Thu 2012-11-15 22:05:08 +0200</span>
<ul start="1"><li>Bug#15872736 FAILING ASSERTION
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.29](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.29) [merge]
    <span class="cstm-style datetime">Thu 2012-11-15 20:38:04 +0200</span>
<ul start="1"><li>Merge mysql-5.1 to mysql-5.5.
</li><li>[Revision #2661.838.11](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.11)
     <span class="cstm-style datetime">Thu 2012-11-15 20:30:36 +0200</span>
<ul start="1"><li>Bug#15874001 CREATE INDEX ON A UTF8 CHAR COLUMN FAILS WITH ROW_FORMAT=REDUNDANT
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.28](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.28)
    <span class="cstm-style datetime">Thu 2012-11-15 12:27:05 +0530</span>
</li><li>[Revision #3077.175.27](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.27)
    <span class="cstm-style datetime">Wed 2012-11-14 17:17:14 +0000</span>
<ul start="1"><li>BUG#12669186: AUTOINC VALUE PERSISTENCY BREAKS CERTAIN REPLICATION SCENARIOS
</li></ul>
</li><li>[Revision #3077.175.26](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.26)
    <span class="cstm-style datetime">Wed 2012-11-14 17:02:36 +0530</span>
<ul start="1"><li>BUG#13556107: CHECK AND REPAIR TABLE SHOULD BE MORE ROBUST [3]
</li></ul>
</li><li>[Revision #3077.175.25](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.25)
    <span class="cstm-style datetime">Wed 2012-11-14 16:35:08 +0530</span>
</li><li>[Revision #3077.175.24](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.24) [merge]
    <span class="cstm-style datetime">Wed 2012-11-14 15:30:23 +0530</span>
<ul start="1"><li>[Revision #2661.838.10](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.10)
     <span class="cstm-style datetime">Wed 2012-11-14 14:57:28 +0530</span>
</li></ul>
</li><li>[Revision #3077.175.23](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.23)
    <span class="cstm-style datetime">Wed 2012-11-14 17:00:41 +0800</span>
<ul start="1"><li>Fix Bug #14753402 - FAILING ASSERTION: LEN == IFIELD-&gt;FIXED_LEN
</li></ul>
</li><li>[Revision #3077.175.22](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.22) [merge]
    <span class="cstm-style datetime">Wed 2012-11-14 11:02:13 +0530</span>
<ul start="1"><li>[Revision #2661.838.9](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.9)
     <span class="cstm-style datetime">Wed 2012-11-14 10:40:20 +0530</span>
</li></ul>
</li><li>[Revision #3077.175.21](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.21) [merge]
    <span class="cstm-style datetime">Tue 2012-11-13 19:13:59 +0100</span>
<ul start="1"><li>Merge
</li><li>[Revision #3077.177.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.177.2)
     <span class="cstm-style datetime">Sun 2012-11-04 22:17:17 +0100</span>
<ul start="1"><li>mtr.pl  - remove unused hack to turn on extra suites based on current directory name  - remove 4 your old debug printout of "vardir: &lt;dir&gt;"
</li></ul>
</li><li>[Revision #3077.177.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.177.1)
     <span class="cstm-style datetime">Sun 2012-11-04 22:11:34 +0100</span>
<ul start="1"><li>mtr.pl  - improve the logic that decides when ndbcluster should be enabled and the extra   test suites for MySQL Cluster should be added. Should be consistent and logical now ;)
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.20](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.20)
    <span class="cstm-style datetime">Mon 2012-11-12 14:24:43 +0200</span>
<ul start="1"><li>This is a backport of "[WL#5674](http://askmonty.org/worklog/?tid=5674) InnoDB: report all deadlocks (Bug#1784)" from MySQL 5.6 into MySQL 5.5
</li></ul>
</li><li>[Revision #3077.175.19](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.19) [merge]
    <span class="cstm-style datetime">Mon 2012-11-12 22:33:40 +0900</span>
<ul start="1"><li>Bug #14676111 WRONG PAGE_LEVEL WRITTEN FOR UPPER THAN FATHER PAGE AT BTR_LIFT_PAGE_UP()
</li><li>[Revision #2661.838.8](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.8)
     <span class="cstm-style datetime">Mon 2012-11-12 22:31:30 +0900</span>
<ul start="1"><li>Bug #14676111 WRONG PAGE_LEVEL WRITTEN FOR UPPER THAN FATHER PAGE AT BTR_LIFT_PAGE_UP()
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.18](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.18)
    <span class="cstm-style datetime">Fri 2012-11-09 15:51:28 +0100</span>
</li><li>[Revision #3077.175.17](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.17)
    <span class="cstm-style datetime">Fri 2012-11-09 19:19:11 +0530</span>
<ul start="1"><li>Bug#13556000: CHECK AND REPAIR TABLE SHOULD BE MORE ROBUST[2]
</li></ul>
</li><li>[Revision #3077.175.16](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.16) [merge]
    <span class="cstm-style datetime">Fri 2012-11-09 19:04:59 +0530</span>
<ul start="1"><li>Null merge from mysql-5.1 to mysql-5.5
</li><li>[Revision #2661.838.7](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.7)
     <span class="cstm-style datetime">Fri 2012-11-09 19:04:01 +0530</span>
<ul start="1"><li>Bug #14669848 CRASH DURING ALTER MAKES ORIGINAL TABLE INACCESSIBLE
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.15](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.15) [merge]
    <span class="cstm-style datetime">Fri 2012-11-09 18:56:20 +0530</span>
<ul start="1"><li>Merging from mysql-5.1 to mysql-5.5.
</li><li>[Revision #2661.839.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.839.1)
     <span class="cstm-style datetime">Thu 2012-11-08 18:09:13 +0530</span>
<ul start="1"><li>Bug #14669848 CRASH DURING ALTER MAKES ORIGINAL TABLE INACCESSIBLE
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.14](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.14) [merge]
    <span class="cstm-style datetime">Fri 2012-11-09 15:16:49 +0530</span>
<ul start="1"><li>BUG#11762933: MYSQLDUMP WILL SILENTLY SKIP THE `EVENT`               TABLE DATA IF DUMPS MYSQL DATABA Problem: If mysqldump is run without <code class="fixed" style="white-space:pre-wrap">--events</code> (or with <code class="fixed" style="white-space:pre-wrap">--skip-events</code>) it will not dump the mysql.event table's data. This behaviour is inconsistent with that of <code class="fixed" style="white-space:pre-wrap">--routines</code> option, which does not affect the dumping of mysql.proc table. According to the Manual, <code class="fixed" style="white-space:pre-wrap">--events</code> (<code class="fixed" style="white-space:pre-wrap">--skip-events</code>) defines, if the Event Scheduler events for the dumped databases should be included in the mysqldump output and this has nothing to do with the mysql.event table itself. Solution: A warning has been added when mysqldump is used without <code class="fixed" style="white-space:pre-wrap">--events</code>  (or with <code class="fixed" style="white-space:pre-wrap">--skip-events</code>) and a separate patch with the behavioral change  will be prepared for 5.6/trunk.
</li><li>[Revision #2661.838.6](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.6)
     <span class="cstm-style datetime">Fri 2012-11-09 15:15:16 +0530</span>
<ul start="1"><li>BUG#11762933: MYSQLDUMP WILL SILENTLY SKIP THE `EVENT`               TABLE DATA IF DUMPS MYSQL DATABA Problem: If mysqldump is run without <code class="fixed" style="white-space:pre-wrap">--events</code> (or with <code class="fixed" style="white-space:pre-wrap">--skip-events</code>) it will not dump the mysql.event table's data. This behaviour is inconsistent with that of <code class="fixed" style="white-space:pre-wrap">--routines</code> option, which does not affect the dumping of mysql.proc table. According to the Manual, <code class="fixed" style="white-space:pre-wrap">--events</code> (<code class="fixed" style="white-space:pre-wrap">--skip-events</code>) defines, if the Event Scheduler events for the dumped databases should be included in the mysqldump output and this has nothing to do with the mysql.event table itself. Solution: A warning has been added when mysqldump is used without <code class="fixed" style="white-space:pre-wrap">--events</code>  (or with <code class="fixed" style="white-space:pre-wrap">--skip-events</code>) and a separate patch with the behavioral change  will be prepared for 5.6/trunk.
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.13](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.13)
    <span class="cstm-style datetime">Fri 2012-11-09 14:54:35 +0530</span>
<ul start="1"><li>BUG#14458232 - CRASH IN THD_IS_TRANSACTION_ACTIVE DURING                THREAD POOLING STRESS TEST PROBLEM: Connection stress tests which consists of concurrent kill connections interleaved with mysql ping queries cause the mysqld server which uses thread pool scheduler to crash. FIX: Killing a connection involves shutdown and close of client socket and this can cause EPOLLHUP(or EPOLLERR) events to be to be queued and handled after disarming and cleanup of  of the connection object (THD) is being done.We disarm the  the connection by modifying the epoll mask to zero which ensure no events come and release the ownership of waiting  thread that collect events and then do the cleanup of THD. object.As per the linux kernel epoll source code (                [http://lxr.linux.no/linux+*/fs/eventpoll.c#L1771](http://lxr.linux.no/linux+*/fs/eventpoll.c#L1771)), EPOLLHUP (or EPOLLERR) can't be masked even if we set EPOLL mask to zero. So we disarm the connection and thus prevent  execution of any query processing handler/queueing to  client ctx. queue by removing the client fd from the epoll         set via EPOLL_CTL_DEL. Also there is a race condition which involve the following threads: 1) Thread X executing KILL CONNECTION Y and is in THD::awake and using mysys_var (holding LOCK_thd_data). 2) Thread Y in tp_process_event executing and is being killed. 3) Thread Z receives KILL flag internally and possible call the tp_thd_cleanup function which set thread session variable and changing mysys_var. The fix for the above race is to set thread session variable under LOCK_thd_data. We also do not call THD::awake if we found the thread in the thread list that is to be killed but it's KILL_CONNECTION flag set thus avoiding any possible concurrent cleanup. This patch is approved by Mikael Ronstrom via email review.
</li></ul>
</li><li>[Revision #3077.175.12](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.12) [merge]
    <span class="cstm-style datetime">Thu 2012-11-08 19:23:54 +0100</span>
<ul start="1"><li>Merge the ULN RPM fix into main.
</li><li>[Revision #3077.176.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.176.1)
     <span class="cstm-style datetime">Thu 2012-11-08 15:49:28 +0100</span>
<ul start="1"><li>Building RPMs for ULN:
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.11](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.11) [merge]
    <span class="cstm-style datetime">Thu 2012-11-08 15:21:02 +0530</span>
<ul start="1"><li>Bug#14234028 - CRASH DURING SHUTDOWN WITH BACKGROUND PURGE THREAD
</li><li>[Revision #2661.838.5](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.5)
     <span class="cstm-style datetime">Thu 2012-11-08 15:14:29 +0530</span>
<ul start="1"><li>Bug#14234028 - CRASH DURING SHUTDOWN WITH BACKGROUND PURGE THREAD
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.10](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.10) [merge]
    <span class="cstm-style datetime">Thu 2012-11-08 14:23:02 +0530</span>
<ul start="1"><li>Bug#11751825 - OPTIMIZE PARTITION RECREATES FULL TABLE INSTEAD JUST PARTITION
</li><li>[Revision #2661.838.4](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.4)
     <span class="cstm-style datetime">Thu 2012-11-08 14:19:27 +0530</span>
<ul start="1"><li>Bug#11751825 - OPTIMIZE PARTITION RECREATES FULL TABLE INSTEAD JUST PARTITION
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.9](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.9)
    <span class="cstm-style datetime">Wed 2012-11-07 20:32:54 +0100</span>
<ul start="1"><li>Make RPMs for ULN build again.
</li></ul>
</li><li>[Revision #3077.175.8](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.8)
    <span class="cstm-style datetime">Wed 2012-11-07 17:45:02 +0100</span>
<ul start="1"><li>Placement change:
</li></ul>
</li><li>[Revision #3077.175.7](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.7)
    <span class="cstm-style datetime">Wed 2012-11-07 19:08:33 +0530</span>
<ul start="1"><li>Bug#14466617 - INVALID WRITES AND/OR CRASH WITH USER                VARIABLES 
</li></ul>
</li><li>[Revision #3077.175.6](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.6)
    <span class="cstm-style datetime">Wed 2012-11-07 18:41:42 +0530</span>
</li><li>[Revision #3077.175.5](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.5) [merge]
    <span class="cstm-style datetime">Wed 2012-11-07 09:03:33 +0530</span>
<ul start="1"><li>Bug #11759445: CAN'T DELETE ROWS FROM MEMORY TABLE WITH HASH KEY.
</li><li>[Revision #2661.838.3](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.3)
     <span class="cstm-style datetime">Wed 2012-11-07 09:00:17 +0530</span>
<ul start="1"><li>Bug #11759445: CAN'T DELETE ROWS FROM MEMORY TABLE WITH HASH KEY.
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.4](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.4) [merge]
    <span class="cstm-style datetime">Tue 2012-11-06 18:44:22 +0530</span>
<ul start="1"><li>Bug#11751825 - OPTIMIZE PARTITION RECREATES FULL TABLE INSTEAD JUST PARTITION
</li><li>[Revision #2661.838.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.2)
     <span class="cstm-style datetime">Tue 2012-11-06 18:35:03 +0530</span>
<ul start="1"><li>Bug#11751825 - OPTIMIZE PARTITION RECREATES FULL TABLE INSTEAD JUST PARTITION
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.3](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.3)
    <span class="cstm-style datetime">Mon 2012-11-05 17:45:13 +0530</span>
</li><li>[Revision #3077.175.2](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.2) [merge]
    <span class="cstm-style datetime">Mon 2012-11-05 12:08:05 +0100</span>
<ul start="1"><li>Version change upmerge - empty
</li><li>[Revision #2661.838.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/2661.838.1)
     <span class="cstm-style datetime">Mon 2012-11-05 11:05:46 +0100</span>
<ul start="1"><li>Raise version number after cloning 5.1.67
</li></ul>
</li></ul>
</li><li>[Revision #3077.175.1](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3077.175.1)
    <span class="cstm-style datetime">Sat 2012-11-03 05:06:09 +0100</span>
<ul start="1"><li>Raise version number after cloning 5.5.29
</li></ul>
</li></ul>
</li><li>[Revision #3334.1.334](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.334)
   <span class="cstm-style datetime">Wed 2013-02-27 10:43:07 +0400</span>
<ul start="1"><li>[MDEV-4208](https://jira.mariadb.org/browse/MDEV-4208): Test rpl.rpl_rotate_purge_deadlock has incorrect preamble
</li></ul>
</li><li>[Revision #3334.1.333](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.333)
   <span class="cstm-style datetime">Sun 2013-02-24 20:05:26 +0100</span>
<ul start="1"><li>Compilation : fix oqgraph's system check, in case where boost header aren't in standard include directory.
</li></ul>
</li><li>[Revision #3334.1.332](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.332)
   <span class="cstm-style datetime">Thu 2013-02-21 22:59:54 +0100</span>
<ul start="1"><li>[MDEV-4190](https://jira.mariadb.org/browse/MDEV-4190) : Fix system checks for OpenBSD
</li></ul>
</li><li>[Revision #3334.1.331](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.331)
   <span class="cstm-style datetime">Thu 2013-02-21 21:46:24 +0100</span>
<ul start="1"><li>[MDEV-4021](https://jira.mariadb.org/browse/MDEV-4021) : Enable Ctrl-C handler when reading password, on Windows.
</li></ul>
</li><li>[Revision #3334.1.330](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.330)
   <span class="cstm-style datetime">Wed 2013-02-20 14:52:43 +0100</span>
<ul start="1"><li>[MDEV-4181](https://jira.mariadb.org/browse/MDEV-4181) : ensure mysql client's beep works on all Windows systems. Use MessageBeep, which employs sound card, rather than system speaker. The secondary benefit is that one can use volume control for this sound (see MySQL's Bug #17088)
</li></ul>
</li><li>[Revision #3334.1.329](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.329)
   <span class="cstm-style datetime">Thu 2013-02-21 01:03:45 +0400</span>
<ul start="1"><li>[MDEV-3819](https://jira.mariadb.org/browse/MDEV-3819) missing constraints for spatial column types.     Checks added to return and error when inappropriate     geometry type is stored in a field.
</li></ul>
</li><li>[Revision #3334.1.328](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.328)
   <span class="cstm-style datetime">Tue 2013-02-19 23:46:52 +0100</span>
<ul start="1"><li>[MDEV-4174](https://jira.mariadb.org/browse/MDEV-4174) - Use kqueue for threadpool implementation on more BSD variants than just FreeBSD  or OSX - i.e NetBSD, OpenBSD, DragonFly, etc.
</li></ul>
</li><li>[Revision #3334.1.327](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.327)
   <span class="cstm-style datetime">Mon 2013-02-18 20:51:36 +0100</span>
<ul start="1"><li>fix typo
</li></ul>
</li><li>[Revision #3334.1.326](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.326)
   <span class="cstm-style datetime">Mon 2013-02-18 20:35:11 +0100</span>
<ul start="1"><li>[MDEV-4183](https://jira.mariadb.org/browse/MDEV-4183): Export additional symbols from RPMs , compatibly to distribution RPMs. -Ensure that symbols listed in CLIENT_API_EXTRA are not thrown away by the linker. -Add THR_KEY_mysys to this list, because Fedora18 exports it.
</li></ul>
</li><li>[Revision #3334.1.325](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.325)
   <span class="cstm-style datetime">Fri 2013-02-08 22:24:06 +0100</span>
<ul start="1"><li>[MDEV-4156](https://jira.mariadb.org/browse/MDEV-4156) Test cases query_cache and query_cache_size_basic fail on 32 bit ppc and s390
</li></ul>
</li><li>[Revision #3334.1.324](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3334.1.324)
   <span class="cstm-style datetime">Fri 2013-02-08 12:59:54 +0100</span>
<ul start="1"><li>make rpm packages to respect CMAKE_INSTALL_PREFIX
</li></ul>
</li></ul>
- [Revision #3392](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3392)
  <span class="cstm-style datetime">Tue 2013-05-21 00:10:35 +0300</span>
<ul start="1"><li>merged in revisions 3853..3857 from lp:codership-mysql/5.5-23
</li></ul>
- [Revision #3391](http://bazaar.launchpad.net/%7Emaria-captains/maria/maria-5.5-galera/revision/3391)
  <span class="cstm-style datetime">Tue 2013-03-26 16:40:02 +0200</span>
<ul start="1"><li>References: [MDEV-4328](https://jira.mariadb.org/browse/MDEV-4328) - avoiding race condition for wsrep_format_desc access
</li></ul>