# mysqlreport

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-report` is a symlink to `mysqlreport`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mysqlreport` is the symlink, and `mariadb-report` the binary name.

`mysqlreport` makes a friendly report of important MariaDB status values. Actually, it makes a friendly report of nearly every status
 value from SHOW STATUS. Unlike SHOW STATUS which simply dumps over 100 values to the screen in one long list, mysqlreport interprets and
 formats the values and presents the basic values and many more inferred values in a human-readable format. Numerous example reports
 are available at the mysqlreport web page at [http://hackmysql.com/mysqlreport](http://hackmysql.com/mysqlreport).

The benefit of mysqlreport is that it allows you to very quickly see a wide array of performance indicators for your MariaDB server
 which would otherwise need to be calculated by hand from all the various SHOW STATUS values. For example, the Index Read Ratio is an
 important value but it's not present in SHOW STATUS; it's an inferred value (the ratio of Key_reads to Key_read_requests).

This documentation outlines all the command line options in mysqlreport, most of which control which reports are printed. This document does not address how to interpret these reports; that topic is covered in the document Guide To Understanding mysqlreport at
 [http://hackmysql.com/mysqlreportguide](http://hackmysql.com/mysqlreportguide).

## Usage

```sql
mysqlreport [options]
```

## mysqlreport options

Technically, command line options are in the form `--option`, but `-option` works too. All options can be abbreviated if the abbreviation is unique. For example, option `--host` can be abbreviated to `--ho` but not `--h` because `--h` is ambiguous: it could mean `--host` or `--help`.

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--all</code></td><td>Equivalent to <code>--dtq --dms --com 3 --sas --qcache</code>. (Notice <code>--tab</code> is not invoked by <code>--all</code>.)</td></tr>
<tr><td><code>--com N</code></td><td>Print top N number of non-DMS Com_ <a href="/kb/en/server-status-variables/">status values</a> in descending order (after DMS in Questions report). If N is not given, default is 3. Such non-DMS Com_ values include <a href="/kb/en/server-status-variables/#com_change_db">Com_change_db</a>, <a href="/kb/en/server-status-variables/#com_show_tables">Com_show_tables</a>, <a href="/kb/en/server-status-variables/#com_rollback">Com_rollback</a>, etc.</td></tr>
<tr><td><code>--dms</code></td><td>Print Data Manipulation Statements (DMS) report (under DMS in Questions report). DMS are those from the <a href="/kb/en/data-manipulation/">Data Manipulation</a> section. Currently, mysqlreport considers only <a href="/kb/en/select/">SELECT</a>, <a href="/kb/en/insert/">INSERT</a>, <a href="/kb/en/replace/">REPLACE</a>, <a href="/kb/en/update/">UPDATE</a>, and <a href="/kb/en/delete/">DELETE</a>. Each DMS is listed in descending order by count.</td></tr>
<tr><td><code>--dtq</code></td><td>Print Distribution of Total Queries (DTQ) report (under Total in Questions report). Queries (or Questions) can be divided into four main areas: DMS (see <code>--dms</code>), Com_ (see <code>--com</code> ), COM_QUIT (see COM_QUIT and Questions at <a href="http://hack‐mysql.com/com_quit">http://hack‐mysql.com/com_quit</a>), and Unknown. <code>--dtq</code> lists the number of queries in each of these areas in descending order.</td></tr>
<tr><td><code>--email ADDRESS</code></td><td>After printing the report to screen, email the report to ADDRESS. This option requires sendmail in /usr/sbin/, therefore it does not work on Windows. /usr/sbin/sendmail can be a sym link to qmail, for example, or any MTA that emulates  sendmail's  -t command line option and operation. The FROM: field is "mysqlreport", SUBJECT: is "MySQL status report".</td></tr>
<tr><td><code>--flush-status</code></td><td>Execute a <a href="/kb/en/flush/">FLUSH  STATUS</a>  after  generating the reports. If you do not have permissions in MariaDB to do this an error from DBD::mysql::st will be printed after the reports.</td></tr>
<tr><td><code>--help</code></td><td>Output help information and exit.</td></tr>
<tr><td><code>--host ADDRESS</code></td><td>Host address.</td></tr>
<tr><td><code>--infile FILE</code></td><td>Instead of getting <a href="/kb/en/show-status/">SHOW STATUS</a> values from MariaDB, read values from FILE. FILE is often a copy of the output of SHOW STATUS including formatting characters (+, -). <em>mysqlreport</em> expects FILE to have the format " value number " where value is only alpha and underscore characters (A-Z and _) and number is a positive integer. Anything before, between, or after value and number is ignored. <em>mysqlreport</em> also needs the following MariaDB server variables: <a href="/kb/en/server-system-variables/#version">version</a>, <a href="/kb/en/server-system-variables/#table_open_cache">table_cache</a>, <a href="/kb/en/server-system-variables/#max_connections">max_connections</a>, <a href="/kb/en/myisam-system-variables/#key_buffer_size">key_buffer_size</a>, <a href="/kb/en/server-system-variables/#query_cache_size">query_cache_size</a>. These values can be specified in INFILE in the format "name = value" where name is one of the aforementioned server variables and value is a positive integer with or without a trailing M and possible periods (for version). For example, to specify an 18M key_buffer_size: key_buffer_size = 18M. Or, a 256 table_cache: table_cache = 256. The M implies Megabytes not million, so 18M means 18,874,368 not 18,000,000. If these server variables are not specified the following defaults are used (respectively) which may cause strange values to be reported: 0.0.0, 64, 100, 8M, 0.</td></tr>
<tr><td><code>--no-mycnf</code></td><td>Makes mysqlreport not read /.my.cnf which it does by default otherwise. <code>--user</code> and <code>--password</code> always override values from /.my.cnf.</td></tr>
<tr><td><code>--outfile FILE</code></td><td>After printing the report to screen, print the report to FILE too. Internally, mysqlreport always writes the report to a temp file first: <code>/tmp/mysqlreport.PID</code> on *nix, <code>c:sqlreport.PID</code> on Windows (PID is the script's process ID). Then it prints the temp file to screen. Then if <code>--outfile</code> is specified, the temp file is copied to OUTFILE. After <code>--email</code> (above), the temp file is deleted.</td></tr>
<tr><td><code>--password</code></td><td>As of version 2.3 <code>--password</code> can take the password on the command line like <code>--password FOO</code>. Using <code>--password</code> alone without giving a password on the command line causes mysqlreport to prompt for a password.</td></tr>
<tr><td><code>--port PORT</code></td><td>Port number.</td></tr>
<tr><td><code>--qcache</code></td><td>Print <a href="/kb/en/query-cache/">Query Cache</a> report.</td></tr>
<tr><td><code>--sas</code></td><td>Print report for Select_ and Sort_ <a href="/kb/en/server-status-variables/">status values</a> (after Questions report). See MySQL Select and Sort Status Variables at <a href="http://hackmysql.com/selectandsort">http://hackmysql.com/selectandsort</a>.</td></tr>
<tr><td><code>--socket SOCKET</code></td><td>For connections to localhost, the Unix socket file to use, or, on Windows, the name of the named pipe to use.</td></tr>
<tr><td><code>--tab</code></td><td>Print Threads, Aborted, and Bytes status reports (after Created temp report). As of mysqlreport v2.3 the Threads report reports on all Threads_ status values.</td></tr>
<tr><td><code>--user USERNAME</code></td><td>Username.</td></tr>
</tbody></table>

## Examples