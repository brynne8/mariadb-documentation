# mysqldump

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-dump` is a symlink to `mysqldump`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadb-dump` is the name of the command-line client, with `mysqldump` a symlink .

The `mysqldump` client is a backup program originally written by Igor
Romanenko. It can be used to dump a database or a collection of databases for
backup or transfer to another database server (not necessarily MariaDB or MySQL). The
dump typically contains SQL statements to create the table, populate it, or
both. However, `mysqldump` can also be used to generate files in CSV, other
delimited text, or XML format.

If you are doing a backup on the server and your tables all are [MyISAM](/kb/en/myisam/) tables,
consider using [mysqlhotcopy](/clients-utilities/backup-restore-and-import-clients/mysqlhotcopy/) instead because it can accomplish faster
backups and faster restores.

mysqldump dumps triggers along with tables, as these are part of the table definition. However, [stored procedures](/programming-customizing-mariadb/stored-routines/stored-procedures/), [views](/programming-customizing-mariadb/views/), and [events](/programming-customizing-mariadb/triggers-events/event-scheduler/events/) are not, and need extra parameters to be recreated explicitly (for example, `--routines` and `--events`). [Procedures](/programming-customizing-mariadb/stored-routines/stored-procedures/) and [functions](functions) are however also part of the system tables (for example [mysql.proc](/kb/en/mysqlproc-table/)).

`mysqldump` supports the [enhancements for START TRANSACTION WITH CONSISTENT SNAPSHOT](/kb/en/enhancements-for-start-transaction-with-consistent-snapshot/#mysqldump).

## Performance

mysqldump doesn't usually consume much CPU resources on modern hardware as by default it uses a single thread. This method is good for a heavily loaded server.

Disk input/outputs per second (IOPS), can however increase for multiple reasons. When you back-up on the same device as the database, this produces unnecessary random IOPS. The dump is done sequentially, on a per table basis, causing a full-table scan and many buffer page misses on tables that are not fully cached in memory.

It's recommended that you back-up from a network location to remove disk IOPS on the database server, but it is vital to use a separate network card to keep network bandwidth available for regular traffic.

Although mysqldump will by default preserve your resources for regular spindle disks and low-core hardware, this doesn't mean that concurrent dumps cannot benefit from hardware architecture like SAN, flash storage, low write workload. The back-up time would benefit from a tool such as MyDumper.

## Usage

There are four general ways to invoke `mysqldump`:

```sql
shell> mysqldump [options] db_name [tbl_name ...]
shell> mysqldump [options] --databases db_name ...
shell> mysqldump [options] --all-databases
shell> mysqldump [options] --system=[option_list]
```

If you do not name any tables following db_name or if you use the
<code class="fixed" style="white-space:pre-wrap">--databases</code> or <code class="fixed" style="white-space:pre-wrap">--all-databases</code> option, entire
databases are dumped.

`mysqldump` does not dump the INFORMATION_SCHEMA (or PERFORMANCE_SCHEMA, if enabled) database by default. MariaDB dumps the `INFORMATION_SCHEMA` if you name it explicitly on the command line, although currently you must also use the <code class="fixed" style="white-space:pre-wrap">--skip-lock-tables</code> option.

To see a list of the options your version of `mysqldump`
supports, execute <code class="fixed" style="white-space:pre-wrap">mysqldump --help</code>.

### Row by Row vs. Buffering

`mysqldump` can retrieve and dump table contents row by row,
or it can retrieve the entire content from a table and buffer it in memory
before dumping it. Buffering in memory can be a problem if you are dumping
large tables. To dump tables row by row, use the <code class="fixed" style="white-space:pre-wrap">--quick</code>
option (or <code class="fixed" style="white-space:pre-wrap">--opt</code>, which enables <code class="fixed" style="white-space:pre-wrap">--quick</code>).
The <code class="fixed" style="white-space:pre-wrap">--opt</code> option (and hence <code class="fixed" style="white-space:pre-wrap">--quick</code>) is
enabled by default, so to enable memory buffering, use
 <code class="fixed" style="white-space:pre-wrap">--skip-quick</code>.

### mysqldump in [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and Higher

`mysqldump` in [MariaDB 10.3](/kb/en/what-is-mariadb-103/) includes logic to cater for the [mysql.transaction_registry table](/kb/en/mysqltransaction_registry-table/). `mysqldump` from an earlier MariaDB release cannot be used on [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and beyond.

### mysqldump and Old Versions of MySQL

If you are using a recent version of `mysqldump` to generate a
dump to be reloaded into a very old MySQL server, you should not use the
 <code class="fixed" style="white-space:pre-wrap">--opt</code> or <code class="fixed" style="white-space:pre-wrap">--extended-insert</code> option. Use
 <code class="fixed" style="white-space:pre-wrap">--skip-opt</code> instead.

### Options

`mysqldump` supports the following options:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--all</code></td><td>Deprecated. Use <code>--create-options</code> instead.</td></tr>
<tr><td><code>-A</code>, <code>--all-databases</code></td><td>Dump all the databases. This will be same as <code>--databases</code> with all databases selected.</td></tr>
<tr><td><code>-Y</code>, <code>--all-tablespaces</code></td><td>Dump all the tablespaces.</td></tr>
<tr><td><code>-y</code>, <code>--no-tablespaces</code></td><td>Do not dump any tablespace information.</td></tr>
<tr><td><code>--add-drop-database</code></td><td>Add a <a href="/kb/en/drop-database/">DROP DATABASE</a> before each create. Typically used in conjunction with the <code>--all-databases</code> or <code>--databases</code> option because no <a href="/kb/en/create-database/">CREATE DATABASE</a> statements are written unless one of those options is specified.</td></tr>
<tr><td><code>--add-drop-table</code></td><td>Add a <a href="/kb/en/drop-table/">DROP TABLE</a> before each create.</td></tr>
<tr><td><code>--add-drop-trigger</code></td><td>Add a <a href="/kb/en/drop-trigger/">DROP TRIGGER</a> statement before each <a href="/kb/en/create-trigger/">CREATE TRIGGER</a>. From <a href="/kb/en/mariadb-1026-release-notes/">MariaDB 10.2.6</a>.</td></tr>
<tr><td><code>--add-locks</code></td><td>Add locks around <a href="/kb/en/insert/">INSERT</a> statements, which results in faster inserts when the dump file is reloaded. Use <code>--skip-add-locks</code> to disable.</td></tr>
<tr><td><code>--allow-keywords</code></td><td>Allow creation of column names that are keywords. This works by prefixing each column name with the table name.</td></tr>
<tr><td><code>----apply-slave-statements</code></td><td>Adds <a href="/kb/en/stop-slave/">STOP SLAVE</a> prior to <a href="/kb/en/change-master-to/">CHANGE MASTER</a> and <a href="/kb/en/start-slave/">START SLAVE</a> to bottom of dump.</td></tr>
<tr><td><code>--character-sets-dir=name</code></td><td>Directory for <a href="/kb/en/data-types-character-sets-and-collations/">character set</a> files.</td></tr>
<tr><td><code>-i</code>, <code>--comments</code></td><td>Write additional information in the dump file such as program version, server version, and host. Disable with <code>--skip-comments</code>.</td></tr>
<tr><td><code>--compact</code></td><td>Give less verbose output (useful for debugging). Disables structure comments and header/footer constructs. Enables the <code>--skip-add-drop-table</code>, <code>--skip-add-locks</code>, <code>--skip-comments</code>, <code>--skip-disable-keys</code>, and <code>--skip-set-charset</code> options.</td></tr>
<tr><td><code>--compatible=name</code></td><td>Change the dump to be compatible with a given mode. By default tables are dumped in a format optimized for MariaDB and MySQL. Legal modes are: <code>ansi</code>, <code>mysql323</code>, <code>mysql40</code>, <code>postgresql</code>, <code>oracle</code>, <code>mssql</code>, <code>db2</code>, <code>maxdb</code>, <code>no_key_options</code>, <code>no_table_options</code>, and <code>no_field_options</code>. One can use several modes separated by commas.<br><br>This option does not guarantee compatibility with other servers. It only enables those SQL mode values that are currently available for making dump output more compatible. For example, <code>--compatible=oracle</code> does not map data types to Oracle types or use Oracle comment syntax.<br><br></td></tr>
<tr><td><code>-c</code>, <code>--complete-insert</code></td><td>Use complete <a href="/kb/en/insert/">INSERT</a> statements that include column names.</td></tr>
<tr><td><code>-C</code>, <code>--compress</code></td><td>Use compression in server/client protocol. Both client and server must support compression for this to work.</td></tr>
<tr><td><code>--copy-s3-tables</code></td><td>By default <a href="/kb/en/s3-storage-engine/">S3</a> tables are ignored. With this option set, the result file will contain a CREATE statement for a similar <a href="/kb/en/aria-storage-engine/">Aria</a> table, followed by the table data and ending with an <code>ALTER TABLE xxx ENGINE=S3</code>. From <a href="/kb/en/mariadb-1050-release-notes/">MariaDB 10.5.0</a>.</td></tr>
<tr><td><code>-a</code>, <code>--create-options</code></td><td>Include all MariaDB and/or MySQL specific create options in <code>CREATE TABLE</code> statements. Use <code>--skip-create-options</code> to disable.</td></tr>
<tr><td><code>-B</code>, <code>--databases</code></td><td>Dump several databases. Normally, <code>mysqldump</code> treats the first name argument on the command line as a database name and following names as table names. With this option, it treats all name arguments as database names. <a href="/kb/en/create-database/">CREATE DATABASE</a> and <a href="/kb/en/use/">USE</a> statements are included in the output before each new database.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">-#, --debug[=#]</code></td><td>If using a debug version of MariaDB, write a debugging log. A typical debug_options string is ´d:t:o,file_name´. The default value is ´d:t:o,/tmp/mysqldump.trace´. If using a non-debug version, <code>mysqldump</code> will catch this and exit.</td></tr>
<tr><td><code>--debug-check</code></td><td>Check memory and open file usage at exit.</td></tr>
<tr><td><code>--debug-info</code></td><td>Print some debug info at exit.</td></tr>
<tr><td><code>--default-auth=name</code></td><td>Default authentication client-side plugin to use.</td></tr>
<tr><td><code>--default-character-set=name</code></td><td>Set the default <a href="/kb/en/data-types-character-sets-and-collations/">character set</a> to <em>name</em>. If no character set is specified, until <a href="/kb/en/mariadb-10311-release-notes/">MariaDB 10.3.11</a>, <em>mysqldump</em> uses utf8, and from <a href="/kb/en/mariadb-10311-release-notes/">MariaDB 10.3.11</a>, uses utf8mb4.</td></tr>
<tr><td><code>--defaults-extra-file=name</code></td><td>Read the file <em>name</em> after the global files are read. Must be given as the first argument.</td></tr>
<tr><td><code>--defaults-file=name</code></td><td>Only read default options from the given file <em>name</em>. Must be given as the first argument.</td></tr>
<tr><td><code>--defaults-group-suffix=str</code></td><td>Also read groups with a suffix of <em>str</em>. For example, since <em>mysqldump</em> normally reads the [client] and [mysqldump] groups, --defaults-group-suffix=x would cause it to also read the groups [mysqldump_x] and [client_x].</td></tr>
<tr><td><code>--delayed-insert</code></td><td>Insert rows with <a href="/kb/en/insert-delayed/">INSERT DELAYED</a> instead of <a href="/kb/en/insert/">INSERT</a>.</td></tr>
<tr><td><code>--delete-master-logs</code></td><td>On a master replication server, delete the binary logs by sending a <a href="/kb/en/sql-commands-purge-logs/">PURGE BINARY LOGS</a> statement to the server after performing the dump operation. This option automatically enables <code>--master-data</code>.</td></tr>
<tr><td><code>-K</code>, <code>--disable-keys</code></td><td><code class="fixed" style="white-space:pre-wrap">'/*!40000 ALTER TABLE tb_name DISABLE KEYS */;</code> and <code class="fixed" style="white-space:pre-wrap">'/*!40000 ALTER TABLE tb_name ENABLE KEYS */;</code> will be put in the output. This makes loading the dump file faster because the indexes are created after all rows are inserted. This option is effective only for non-unique indexes of MyISAM tables. Disable with <code>--skip-disable-keys</code>.</td></tr>
<tr><td><code>--dump-date</code></td><td>If the <code class="fixed" style="white-space:pre-wrap">--comments</code> option and this option are given, <code>mysqldump</code> produces a comment at the end of the dump of the following form:<pre class="fixed"><span class="c1">-- Dump completed on DATE</span>
</pre> However, the date causes dump files taken at different times to appear to be different, even if the data are otherwise identical. <code class="fixed" style="white-space:pre-wrap">--dump-date</code> and <code class="fixed" style="white-space:pre-wrap">--skip-dump-date</code> control whether the date is added to the comment. The default is <code class="fixed" style="white-space:pre-wrap">--dump-date</code> (include the date in the comment). <code class="fixed" style="white-space:pre-wrap">--skip-dump-date</code> suppresses date printing.</td></tr>
<tr><td><code>--dump-slave[=value]</code></td><td>Used for producing a dump file from a replication slave server that can be used to set up another slave server with the same master. Causes the <a href="/kb/en/binary-log/">binary log</a> position and filename of the master to be appended to the dumped data output. Setting the value to <code>1</code> (the default) will print it as a <a href="/kb/en/change-master-to/">CHANGE MASTER</a> command in the dumped data output; if set to <code>2</code>, that command will be prefixed with a comment symbol. This option will turn <code>--lock-all-tables</code> on, unless <code>--single-transaction</code> is specified too (in which case a global read lock is only taken a short time at the beginning of the dump - don't forget to read about <code>--single-transaction</code> below). In all cases any action on logs will happen at the exact moment of the dump. Option automatically turns <code>--lock-tables</code> off. Using this option causes mysqldump to stop the slave SQL thread before beginning the dump, and restart it again after completion.</td></tr>
<tr><td><code>-E, --events</code></td><td>Include <a href="/kb/en/stored-programs-and-views-events/">Event Scheduler events</a> for the dumped databases in the output.</td></tr>
<tr><td><code>-e</code>, <code>--extended-insert</code></td><td>Use multiple-row <a href="/kb/en/insert/">INSERT</a> syntax that include several <code>VALUES</code> lists. This results in a smaller dump file and speeds up inserts when the file is reloaded. Defaults to on; use <code>--skip-extended-insert</code> to disable.</td></tr>
<tr><td><code>--fields-terminated-by=name</code></td><td>Fields in the output file are terminated by the given string. Used with the <code>--tab</code> option and has the same meaning as the corresponding <code>FIELDS</code> clause for <a href="/kb/en/load-data-infile/">LOAD DATA INFILE</a>.</td></tr>
<tr><td><code>--fields-enclosed-by=name</code></td><td>Fields in the output file are enclosed by the given character. Used with the <code class="fixed" style="white-space:pre-wrap">--tab</code> option and has the same meaning as the corresponding <code>FIELDS</code> clause for <a href="/kb/en/load-data-infile/">LOAD DATA INFILE</a>.</td></tr>
<tr><td><code>--fields-optionally-enclosed-by=name</code></td><td>Fields in the output file are optionally enclosed by the given character. Used with the <code class="fixed" style="white-space:pre-wrap">--tab</code> option and has the same meaning as the corresponding <code>FIELDS</code> clause for <a href="/kb/en/load-data-infile/">LOAD DATA INFILE</a>.</td></tr>
<tr><td><code>--fields-escaped-by=name</code></td><td>Fields in the output file are escaped by the given character. Used with the <code class="fixed" style="white-space:pre-wrap">--tab</code> option and has the same meaning as the corresponding <code>FIELDS</code> clause for <a href="/kb/en/load-data-infile/">LOAD DATA INFILE</a>.</td></tr>
<tr><td><code>--first-slave</code></td><td>Removed in <a href="/kb/en/what-is-mariadb-55/">MariaDB 5.5</a>. Use <code>--lock-all-tables</code> instead.</td></tr>
<tr><td><code>-F</code>, <code>--flush-logs</code></td><td>Flush the MariaDB server log files before starting the dump. This option requires the <code>RELOAD</code> privilege. If you use this option in combination with the <code>--databases=</code> or <code>--all-databases</code> option, the logs are flushed for each database dumped. The exception is when using <code>--lock-all-tables</code>or <code>--master-data</code>: In this case, the logs are flushed only once, corresponding to the moment all tables are locked. If you want your dump and the log flush to happen at the same exact moment, you should use <code>--flush-logs</code> together with either <code>--lock-all-tables</code> or <code>--master-data</code>.</td></tr>
<tr><td><code>--flush-privileges</code></td><td>Send a <a href="/kb/en/flush/">FLUSH PRIVILEGES</a> statement to the server after dumping the <a href="/kb/en/the-mysql-database-tables/">mysql database</a>. This option should be used any time the dump contains the mysql database and any other database that depends on the data in the mysql database for proper restoration.</td></tr>
<tr><td><code>-f</code>, <code><code>--force</code></code></td><td>Continue even if an SQL error occurs during a table dump.<br><br>One use for this option is to cause <em>mysqldump</em> to continue executing even when it encounters a view that has become invalid because the definition refers to a table that has been dropped. Without <code>--force</code> in this example, <code>mysqldump</code> exits with an error message. With <code>--force</code>, <em>mysqldump</em> prints the error message, but it also writes an SQL comment containing the view definition to the dump output and continues executing.</td></tr>
<tr><td><code>--gtid</code></td><td>This option is available from version 10.0.13, and is used together with <code>--master-data</code> and <code>--dump-slave</code> to more conveniently set up a new GTID slave. It causes those options to output SQL statements that configure the slave to use the <a href="/kb/en/global-transaction-id/">global transaction ID</a> to connect to the master instead of old-style filename/offset positions. The old-style positions are still included in comments when <code>--gtid</code> is used; likewise the GTID position is included in comments even if <code>--gtid</code> is not used.</td></tr>
<tr><td><code>-?</code>, <code>--help</code></td><td>Display a help message and exit.</td></tr>
<tr><td><code>--hex-blob</code></td><td>Dump binary strings in hexadecimal format (for example, <code>´abc´</code> becomes <code>0x616263</code>). The affected data types are <a href="/kb/en/binary/">BINARY</a>, <a href="/kb/en/varbinary/">VARBINARY</a>, the <a href="/kb/en/blob/">BLOB</a> types, and <a href="/kb/en/bit/">BIT</a>.</td></tr>
<tr><td><code>-h name</code>, <code>--host=name</code></td><td>Connect to and dump data from the MariaDB or MySQL server on the given host. The default host is <code>localhost</code>.</td></tr>
<tr><td><code>--ignore-database=name</code></td><td>Do not dump the specified database. To specify more than one database to ignore, use the directive multiple times, once for each database. Only takes effect when used together with <code>--all-databases</code> or <code>-A</code>. Added in <a href="/kb/en/mariadb-1037-release-notes/">MariaDB 10.3.7</a>.</td></tr>
<tr><td><code>--ignore-table=name</code></td><td>Do not dump the specified table. To specify more than one table to ignore, use the directive multiple times, once for each table. Each table must be specified with both database and table names, e.g., <code>--ignore-table=database.table</code>. This option also can be used to ignore views.</td></tr>
<tr><td><code>--ignore-table-data=name</code></td><td>Do not dump the specified table data (only the structure). To specify more than one table to ignore, use the directive multiple times, once for each table. Each table must be specified with both database and table names. From <a href="/kb/en/mariadb-10146-release-notes/">MariaDB 10.1.46</a>, <a href="/kb/en/mariadb-10233-release-notes/">MariaDB 10.2.33</a>, <a href="/kb/en/mariadb-10324-release-notes/">MariaDB 10.3.24</a>, <a href="/kb/en/mariadb-10414-release-notes/">MariaDB 10.4.14</a> and <a href="/kb/en/mariadb-1053-release-notes/">MariaDB 10.5.3</a>. See also <code>--no-data</code>.</td></tr>
<tr><td><code>--include-master-host-port</code></td><td>Add the <code>MASTER_HOST</code> and <code>MASTER_PORT</code> options for the <a href="/kb/en/change-master-to/">CHANGE MASTER TO</a> statement when using the <code>--dump-slave</code> option for a slave dump.</td></tr>
<tr><td><code>--insert-ignore</code></td><td>Insert rows with <code>INSERT IGNORE</code> instead of <code>INSERT</code>.</td></tr>
<tr><td><code>--lines-terminated-by=name</code></td><td>Lines in the output file are terminated by the given string. This option is used with the <code>--tab</code> option and has the same meaning as the corresponding <code>LINES</code> clause for <a href="/kb/en/load-data-infile/">LOAD DATA INFILE</a>.</td></tr>
<tr><td><code>-x</code>, <code>--lock-all-tables</code></td><td>Lock all tables across all databases. This is achieved by acquiring a global read lock for the duration of the whole dump by executing <a href="/kb/en/flush/">FLUSH TABLES WITH READ LOCK</a>. This option automatically turns off <code>--single-transaction</code> and <code>--lock-tables</code>.</td></tr>
<tr><td><code>-l</code>, <code>--lock-tables</code></td><td>For each dumped database, lock all tables to be dumped before dumping them. The tables are locked with <code>READ LOCAL</code> to allow concurrent inserts in the case of <a href="/kb/en/myisam/">MyISAM</a> tables. For transactional tables such as <a href="/kb/en/innodb/">InnoDB</a>, <code>--single-transaction</code> is a much better option than <code>--lock-tables</code> because it does not need to lock the tables at all.<br><br>Because <code>--lock-tables</code> locks tables for each database separately, this option does not guarantee that the tables in the dump file are logically consistent between databases. Tables in different databases may be dumped in completely different states. Use <code>--skip-lock-tables</code> to disable.</td></tr>
<tr><td><code>--log-error=name</code></td><td>Log warnings and errors by appending them to the named file. The default is to do no logging.</td></tr>
<tr><td><code>--log-queries</code></td><td>When restoring the dump, the server will, if logging is turned on, log the queries to the general and <a href="/kb/en/slow-query-log/">slow query log</a>. Defaults to on; use --skip-log-queries to disable. Added in <a href="/kb/en/mariadb-1011-release-notes/">MariaDB 10.1.1</a>.</td></tr>
<tr><td><code>--master-data[=#]</code></td><td>Causes the <a href="/kb/en/binary-log/">binary log</a> position and filename to be appended to the output, useful for dumping a master replication server to produce a dump file that can be used to set up another server as a slave of the master. These are the master server coordinates from which the slave should start replicating after you load the dump file into the slave. If the option is set to 1 (the default), will print it as a <a href="/kb/en/change-master-to/">CHANGE MASTER</a> command; if set to 2, that command will be prefixed with a comment symbol. This <code>--master-data</code> option will turn <code>--lock-all-tables</code> on, unless <code>--single-transaction</code> is specified too. Before <a href="/kb/en/what-is-mariadb-53/">MariaDB 5.3</a> this would take a global read lock for a short time at the beginning of the dump - see <a href="/kb/en/enhancements-for-start-transaction-with-consistent-snapshot/">Enhancements for START TRANSACTION WITH CONSISTENT SNAPSHOT</a> and the <code>--single-transaction</code> option below). In all cases, any action on logs will happen at the exact moment of the dump. This option automatically turns <code>--lock-tables</code> off.<br><br> In all cases, any action on logs happens at the exact moment of the dump.<br><br> It is also possible to set up a slave by dumping an existing slave of the master. To do this, use the following procedure on the existing slave:<br><br>1. Stop the slave´s SQL thread and get its current status:<pre class="fixed"><span class="n">mysql</span><span class="o">&gt;</span> <span class="n">STOP</span> <span class="n">SLAVE</span> <span class="n">SQL_THREAD</span><span class="p">;</span>
</pre> <pre class="fixed"><span class="n">mysql</span><span class="o">&gt;</span> <span class="k">SHOW</span> <span class="n">SLAVE</span> <span class="n">STATUS</span><span class="p">;</span>
</pre><br><br>2. From the output of the SHOW SLAVE STATUS statement, the binary log coordinates of the master server from which the new slave should start replicating are the values of the Relay_Master_Log_File and Exec_Master_Log_Pos fields. Denote those values as file_name and file_pos.<br><br>2. Dump the slave server:<pre class="fixed">shell&gt; mysqldump --master-data<span class="o">=</span><span class="m">2</span> --all-databases &gt; dumpfile
</pre><br><br>3. Restart the slave:<pre class="fixed"><span class="n">mysql</span><span class="o">&gt;</span> <span class="n">START</span> <span class="n">SLAVE</span><span class="p">;</span>
</pre><br><br>4. On the new slave, load the dump file:<pre class="fixed">shell&gt; mysql &lt; dumpfile
</pre><br><br>5. On the new slave, set the replication coordinates to those of the master server obtained earlier:<pre class="fixed"><span class="n">mysql</span><span class="o">&gt;</span> <span class="k">CHANGE</span> <span class="n">MASTER</span> <span class="k">TO</span> <span class="n">MASTER_LOG_FILE</span> <span class="o">=</span> <span class="err">´</span><span class="n">file_name</span><span class="err">´</span><span class="p">,</span> <span class="n">MASTER_LOG_POS</span> <span class="o">=</span> <span class="n">file_pos</span><span class="p">;</span>
</pre> The <code>CHANGE MASTER</code> TO statement might also need other parameters, such as MASTER_HOST to point the slave to the correct master server host. Add any such parameters as necessary.</td></tr>
<tr><td><code>--max-allowed-packet=# </code></td><td>The maximum packet length to send to or receive from server.</td></tr>
<tr><td><code>--net-buffer-length=# </code></td><td>The buffer size for TCP/IP and socket communication.</td></tr>
<tr><td><code>--no-autocommit</code></td><td>Enclose the <a href="/kb/en/insert/">INSERT</a> statements for each dumped table within <a href="/kb/en/server-system-variables/#autocommit">SET autocommit = 0</a> and <a href="/kb/en/commit/">COMMIT</a> statements.</td></tr>
<tr><td><code>-n</code>, <code>--no-create-db</code></td><td>This option suppresses the <a href="/kb/en/create-database/">CREATE DATABASE ... IF EXISTS</a> statement that normally is output for each dumped database if <code>--all-databases</code> or <code>--databases</code> is given.</td></tr>
<tr><td><code>-t</code>, <code>--no-create-info</code></td><td>Do not write <a href="create-tablle">CREATE TABLE</a> statements which re-create each dumped table.</td></tr>
<tr><td><code>-d</code>, <code>--no-data</code></td><td>Do not write any table row information (that is, do not dump table contents). This is useful if you want to dump only the <a href="/kb/en/create-table/">CREATE TABLE</a> statement for the table (for example, to create an empty copy of the table by loading the dump file). See also --ignore-table-data .</td></tr>
<tr><td><code>--no-data-med</code></td><td>Do not dump rows for engines that manage external data (i.e. <a href="/kb/en/merge/">MRG_MyISAM</a>, MRG_ISAM, <a href="/kb/en/connect/">CONNECT</a>, <a href="/kb/en/oqgraph-storage-engine/">OQGRAPH</a>, <a href="/kb/en/spider/">Spider</a>, VP, <a href="/kb/en/federatedx-storage-engine/">Federated</a>). This option is enabled by default. If you want to dump data for these engines, then you would need to set <code>--no-data-med=0</code>.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file. Must be given as the first argument.</td></tr>
<tr><td><code>-N</code>, <code>--no-set-names</code></td><td>Suppress the <code>SET NAMES</code> statement. This has the same effect as <code>--skip-set-charset</code>.</td></tr>
<tr><td><code>--opt</code></td><td>This option is shorthand. It is the same as specifying <code>--add-drop-table</code>, <code>--add-locks</code>, <code>--create-options</code>, <code>--quick</code>, <code>--extended-insert</code>, <code>--lock-tables</code>, <code>--set-charset</code>, and <code>--disable-keys</code>. Enabled by default, disable with <code>--skip-opt</code>. It should give you a fast dump operation and produce a dump file that can be reloaded into a MariaDB server quickly.<br><br>The <code>--opt</code> option is enabled by default. Use <code>--skip-opt</code> to disable it. See the discussion at the beginning of this section for information about selectively enabling or disabling a subset of the options affected by <code>--opt</code>.</td></tr>
<tr><td><code>--order-by-primary</code></td><td>Sorts each table's rows by primary key, or first unique key, if such a key exists. This is useful when dumping a <a href="/kb/en/myisam/">MyISAM</a> table to be loaded into an <a href="/kb/en/innodb/">InnoDB</a> table, but will make the dump itself take considerably longer.</td></tr>
<tr><td><code>-p[passwd]</code>, <code>--password[=passwd</code></td><td>The password to use when connecting to the server. If you use the short option form (<code>-p</code>), you cannot have a space between the option and the password. If you omit the password value following the <code>--password</code> or <code>-p</code> option on the command line, <em>mysqldump</em> prompts for one.<br><br>Specifying a password on the command line should be considered insecure. You can use an option file to avoid giving the password on the command line.</td></tr>
<tr><td><code>-W</code>, <code>--pipe</code></td><td>On Windows, connect to the server via a named pipe. This option applies only if the server supports named-pipe connections.</td></tr>
<tr><td><code>--plugin-dir</code></td><td>Directory for client-side plugins.</td></tr>
<tr><td><code>-P num</code>, <code>--port=num</code></td><td>The TCP/IP port number to use for the connection.</td></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit. Must be given as the first argument.</td></tr>
<tr><td><code>--protocol=name</code></td><td>The connection protocol to use for connecting to the server (TCP, SOCKET, PIPE, MEMORY). It is useful when the other connection parameters normally would cause a protocol to be used other than the one you want.</td></tr>
<tr><td><code>-q</code>, <code>--quick</code></td><td>This option is useful for dumping large tables. It forces <em>mysqldump</em> to retrieve rows for a table from the server a row at a time and to then dump the results directly to stdout rather than retrieving the entire row set and buffering it in memory before writing it out. Defaults to on, use <code>--skip-quick</code> to disable.</td></tr>
<tr><td><code>-Q</code>, <code>--quote-names</code></td><td>Quote identifiers (such as database, table, and column names) within backtick (<code>`</code>) characters. If the <code>ANSI_QUOTES</code> SQL mode is enabled, identifiers are quoted within (<code>"</code>) characters. This option is enabled by default. It can be disabled with <code>--skip-quote-names</code>, but this option should be given after any option such as <code>--compatible</code> that may enable <code>--quote-names</code>.</td></tr>
<tr><td><code>--replace</code></td><td>Use <a href="/kb/en/replace/">REPLACE INTO</a> statements instead of <a href="/kb/en/insert/">INSERT INTO</a> statements.</td></tr>
<tr><td><code>-r</code>, <code>--result-file=name</code></td><td>Direct output to a given file. This option should be used on Windows to prevent newline <code>"\n"</code> characters from being converted to <code>"\r\n"</code> carriage return/newline sequences. The result file is created and its previous contents overwritten, even if an error occurs while generating the dump.</td></tr>
<tr><td><code>-R</code>, <code>--routines</code></td><td>Include stored routines (<a href="/kb/en/stored-procedures/">procedures</a> and <a href="/kb/en/stored-functions/">functions</a>) for the dumped databases in the output. Use of this option requires the <code>SELECT</code> privilege for the <a href="/kb/en/mysqlproc-table/">mysql.proc</a> table. The output generated using <code>--routines</code> contains <a href="/kb/en/create-procedure/">CREATE PROCEDURE</a> and <a href="/kb/en/create-function/">CREATE FUNCTION</a> statements to re-create the routines. However, these statements do not include attributes such as the routine creation and modification timestamps. This means that when the routines are reloaded, they will be created with the timestamps equal to the reload time.<br><br>If you require routines to be re-created with their original timestamp attributes, do not use <code>--routines</code>. Instead, dump and reload the contents of the <a href="/kb/en/mysqlproc-table/">mysql.proc</a> table directly, using a MariaDB account which has appropriate privileges for the mysql database.<br><br></td></tr>
<tr><td><code>set-charset</code></td><td>Add <code>'SET NAMES default_character_set'</code> to the output in order to set the <a href="/kb/en/data-types-character-sets-and-collations/">character set</a>. Enabled by default; suppress with <code>--skip-set-charset</code>.</td></tr>
<tr><td><code class="fixed" style="white-space:pre-wrap">-O, --set-variable=name</code></td><td>Change the value of a variable. Please note that this option is deprecated; you can set variables directly with <code class="fixed" style="white-space:pre-wrap">--variable-name=value</code>.</td></tr>
<tr><td><code>--shared-memory-base-name</code></td><td>Shared-memory name to use for Windows connections using shared memory to a local server (started with the <code>--shared-memory</code> option). Case-sensitive. Defaults to <code>MYSQL</code>.</td></tr>
<tr><td><code>--single-transaction</code></td><td>This option sends a <a href="/kb/en/start-transaction/">START TRANSACTION</a> SQL statement to the server before dumping data. It is useful only with transactional tables such as <a href="/kb/en/innodb/">InnoDB</a>, because then it dumps the consistent state of the database at the time when <code>BEGIN</code> was issued without blocking any applications.<br><br>When using this option, you should keep in mind that only InnoDB tables are dumped in a consistent state. The single-transaction feature depends not only on the engine being transactional and capable of REPEATABLE-READ, but also on START TRANSACTION WITH CONSISTENT SNAPSHOT. The dump is <strong>not</strong> guaranteed to be consistent for other storage engines. For example, any <a href="/kb/en/tokudb/">TokuDB</a>, <a href="/kb/en/myisam/">MyISAM</a> or <a href="/kb/en/memory-storage-engine/">MEMORY</a> tables dumped while using this option may still change state.<br><br>While a <code>--single-transaction</code> dump is in process, to ensure a valid dump file (correct table contents and binary log coordinates), no other connection should use the following statements: <a href="/kb/en/alter-table/">ALTER TABLE</a>, <a href="/kb/en/create-table/">CREATE TABLE</a>, <a href="/kb/en/drop-table/">DROP TABLE</a>, <a href="/kb/en/rename-table/">RENAME TABLE</a>, or <a href="/kb/en/truncate-table/">TRUNCATE TABLE</a>. A consistent read is not isolated from those statements, so use of them on a table to be dumped can cause the <code>SELECT</code> (performed by <em>mysqldump</em> to retrieve the table contents) to obtain incorrect contents or fail.<br><br>The <code>--single-transaction</code> option and the <code>--lock-tables</code> option are mutually exclusive because <a href="/kb/en/lock-tables-and-unlock-tables/">LOCK TABLES</a> causes any pending transactions to be committed implicitly. So this option automatically turns off <code>--lock-tables</code><br><br>To dump large tables, you should combine the <code>--single-transaction</code> option with <code>--quick</code>.</td></tr>
<tr><td><code>--skip-add-locks</code></td><td>Disable the <code>--add-locks</code> option.</td></tr>
<tr><td><code>--skip-comments</code></td><td>Disable the <code>--comments</code> option.</td></tr>
<tr><td><code>--skip-disable-keys</code></td><td>Disable the <code>--disable-keys</code> option.</td></tr>
<tr><td><code>--skip-extended-insert</code></td><td>Disable the <code>--extended-insert</code> option.</td></tr>
<tr><td><code>--skip-opt</code></td><td>Disable the <code>--opt</code> option (disables <code>--add-drop-table</code>, <code>--add-locks</code>, <code>--create-options</code>, <code>--quick</code>, <code>--extended-insert</code>, <code>--lock-tables</code>, <code>--set-charset</code>, and <code>--disable-keys</code>).</td></tr>
<tr><td><code>--skip-quick</code></td><td>Disable the <code>--quick</code> option.</td></tr>
<tr><td><code>--skip-quote-name</code></td><td>Disable the <code>--quote-names</code> option.</td></tr>
<tr><td><code>--skip-set-charset</code></td><td>Disable the <code>--set-charset</code> option.</td></tr>
<tr><td><code>--skip-triggers</code></td><td>Disable the <code>--triggers</code> option.</td></tr>
<tr><td><code>--skip-tz-utc</code></td><td>Disable the <code>--tz-utc</code> option.</td></tr>
<tr><td><code>-S name</code>, <code>--socket=name</code></td><td>For connections to localhost, the Unix socket file to use, or, on Windows, the name of the named pipe to use.</td></tr>
<tr><td><code>--ssl</code></td><td>Enables <a href="/kb/en/data-in-transit-encryption/">TLS</a>. TLS is also enabled even without setting this option when certain other TLS options are set. Starting with <a href="/kb/en/what-is-mariadb-102/">MariaDB 10.2</a>, the <code>--ssl</code> option will not enable <a href="/kb/en/secure-connections-overview/#server-certificate-verification">verifying the server certificate</a> by default. In order to verify the server certificate, the user must specify the <code>--ssl-verify-server-cert</code> option.</td></tr>
<tr><td><code>--ssl-ca=name</code></td><td>Defines a path to a PEM file that should contain one or more X509 certificates for trusted Certificate Authorities (CAs) to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. See <a href="/kb/en/secure-connections-overview/#certificate-authorities-cas">Secure Connections Overview: Certificate Authorities (CAs)</a> for more information. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-capath=name</code></td><td>Defines a path to a directory that contains one or more PEM files that should each contain one X509 certificate for a trusted Certificate Authority (CA) to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. The directory specified by this option needs to be run through the <code><a href="https://www.openssl.org/docs/man1.1.1/man1/rehash.html">openssl rehash</a></code> command. See <a href="/kb/en/secure-connections-overview/#certificate-authorities-cas">Secure Connections Overview: Certificate Authorities (CAs)</a> for more information. This option is only supported if the client was built with OpenSSL or yaSSL. If the client was built with GnuTLS or Schannel, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-cert=name</code></td><td>Defines a path to the X509 certificate file to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-cipher=name</code></td><td>List of permitted ciphers or cipher suites to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-crl=name</code></td><td>Defines a path to a PEM file that should contain one or more revoked X509 certificates to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. See <a href="/kb/en/secure-connections-overview/#certificate-revocation-lists-crls">Secure Connections Overview: Certificate Revocation Lists (CRLs)</a> for more information. This option is only supported if the client was built with OpenSSL or Schannel. If the client was built with yaSSL or GnuTLS, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms.</td></tr>
<tr><td><code>--ssl-crlpath=name</code></td><td>Defines a path to a directory that contains one or more PEM files that should each contain one revoked X509 certificate to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. The directory specified by this option needs to be run through the <code><a href="https://www.openssl.org/docs/man1.1.1/man1/rehash.html">openssl rehash</a></code> command. See <a href="/kb/en/secure-connections-overview/#certificate-revocation-lists-crls">Secure Connections Overview: Certificate Revocation Lists (CRLs)</a> for more information. This option is only supported if the client was built with OpenSSL. If the client was built with yaSSL, GnuTLS, or Schannel, then this option is not supported. See <a href="/kb/en/tls-and-cryptography-libraries-used-by-mariadb/">TLS and Cryptography Libraries Used by MariaDB</a> for more information about which libraries are used on which platforms.</td></tr>
<tr><td><code>--ssl-key=name</code></td><td>Defines a path to a private key file to use for <a href="/kb/en/data-in-transit-encryption/">TLS</a>. This option requires that you use the absolute path, not a relative path. This option implies the <code>--ssl</code> option.</td></tr>
<tr><td><code>--ssl-verify-server-cert</code></td><td>Enables <a href="/kb/en/secure-connections-overview/#server-certificate-verification">server certificate verification</a>. This option is disabled by default.</td></tr>
<tr><td><code>--system</code></td><td>Dump the database's system tables in a logical form. With this option, the <a href="/kb/en/the-mysql-database-tables/">mysql database</a> tables are dumped as <a href="/kb/en/create-user/">CREATE USER</a>, <a href="/kb/en/create-server/">CREATE SERVER</a> and other forms of logical portable SQL statements. The values here are from the set of <code>all</code>, <code>users</code>, <code>plugins</code>, <code>udfs</code>, <code>servers</code>, <code>stats</code>, <code>timezones</code>. From <a href="/kb/en/mariadb-10237-release-notes/">MariaDB 10.2.37</a>, <a href="/kb/en/mariadb-10328-release-notes/">MariaDB 10.3.28</a>, <a href="/kb/en/mariadb-10418-release-notes/">MariaDB 10.4.18</a> and <a href="/kb/en/mariadb-1059-release-notes/">MariaDB 10.5.9</a>.</td></tr>
<tr><td><code>-T</code>, <code>--tab=name</code></td><td>Produce tab-separated text-format data files. With this option, for each dumped table <code>mysqldump</code> will create a <code>tbl_name.sql</code> file containing the <code>CREATE TABLE</code> statement that creates the table, and and a <code>tbl_name.txt</code> file containing the table's data. The option value is the directory in which to write the files.<br><br><strong>Note:</strong> <em>This option can only be used when </em>mysqldump/ is run on the same machine as the mysqld server. You must have the <code>FILE</code> privilege, and the server must have permission to write files in the directory that you specify.<em><br><br>By default, the <code>.txt</code> data files are formatted using tab characters between column values and a newline at the end of each line. The format can be specified explicitly using the <code>--fields-xxx</code> and <code>--lines-terminated-by</code> options.<br><br> Column values are converted to the character set specified by the <code>--default-character-set</code> option.</em></td></tr>
<tr><td><code>--tables</code></td><td>This option overrides the <code>--databases</code> (<code>-B</code>) option. <em>mysqldump</em> regards all name arguments following the option as table names.</td></tr>
<tr><td><code>--tls-version=name</code></td><td>This option accepts a comma-separated list of TLS protocol versions. A TLS protocol version will only be enabled if it is present in this list. All other TLS protocol versions will not be permitted. See <a href="/kb/en/secure-connections-overview/#tls-protocol-versions">Secure Connections Overview: TLS Protocol Versions</a> for more information. This option was added in <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>--triggers</code></td><td>Include <a href="/kb/en/triggers/">triggers</a> for each dumped table in the output. This option is enabled by default; disable it with <code>--skip-triggers</code>.</td></tr>
<tr><td><code>--tz-utc</code></td><td>This option enables <a href="/kb/en/timestamp/">TIMESTAMP</a> columns to be dumped and reloaded between servers in different time zones. <em>mysqldump</em> sets its connection time zone to UTC and adds <code>SET TIME_ZONE=´+00:00´</code> to the dump file. Without this option, TIMESTAMP columns are dumped and reloaded in the time zones local to the source and destination servers, which can cause the values to change if the servers are in different time zones. <code>--tz-utc</code> also protects against changes due to daylight saving time. <code>--tz-utc</code> is enabled by default. To disable it, use <code>--skip-tz-utc</code>.</td></tr>
<tr><td><code>-u name</code>, <code>--user=name</code></td><td>The MariaDB user name to use when connecting to the server.</td></tr>
<tr><td><code>-v</code>, <code>--verbose</code></td><td>Verbose mode. Print more information about what the program is doing during various stages.</td></tr>
<tr><td><code>-V</code>, <code>--version</code></td><td>Output version information and exit.</td></tr>
<tr><td><code>-w cond</code>, <code>--where=cond</code></td><td>Dump only rows selected by the given <code>WHERE</code> condition <em>cond</em>. Quotes around the condition are mandatory if it contains spaces or other characters that are special to your command interpreter.<br><br>Examples:<pre class="fixed">--where<span class="o">=</span><span class="s2">"user=´jimf´"</span>
</pre><pre class="fixed">-w<span class="s2">"userid&gt;1"</span>
</pre><pre class="fixed">-w<span class="s2">"userid&lt;1"</span>
</pre></td></tr>
<tr><td><code>-X</code>, <code>--xml</code></td><td>Dump a database as well formed XML.</td></tr>
</tbody></table>

#### Group Options

Some `mysqldump` options are shorthand for groups of other options:

- Use of <code class="fixed" style="white-space:pre-wrap">--opt</code> is the same as specifying
 <code class="fixed" style="white-space:pre-wrap">--add-drop-table</code>, <code class="fixed" style="white-space:pre-wrap">--add-locks</code>,
 <code class="fixed" style="white-space:pre-wrap">--create-options</code>, <code class="fixed" style="white-space:pre-wrap">--disable-keys</code>,
 <code class="fixed" style="white-space:pre-wrap">--extended-insert</code>, <code class="fixed" style="white-space:pre-wrap">--lock-tables</code>,
 <code class="fixed" style="white-space:pre-wrap">--quick</code>, and <code class="fixed" style="white-space:pre-wrap">--set-charset</code>. All of the
 options that <code class="fixed" style="white-space:pre-wrap">--opt</code> stands for also are on by default
 because <code class="fixed" style="white-space:pre-wrap">--opt</code> is on by default.
- Use of <code class="fixed" style="white-space:pre-wrap">--compact</code> is the same as specifying
 <code class="fixed" style="white-space:pre-wrap">--skip-add-drop-table</code>,
 <code class="fixed" style="white-space:pre-wrap">--skip-add-locks</code>, <code class="fixed" style="white-space:pre-wrap">--skip-comments</code>,
 <code class="fixed" style="white-space:pre-wrap">--skip-disable-keys</code>, and
 <code class="fixed" style="white-space:pre-wrap">--skip-set-charset</code> options.

To reverse the effect of a group option, uses its <code class="fixed" style="white-space:pre-wrap">--skip-xxx</code>
form (<code class="fixed" style="white-space:pre-wrap">--skip-opt</code> or <code class="fixed" style="white-space:pre-wrap">--skip-compact</code>). It
is also possible to select only part of the effect of a group option by
following it with options that enable or disable specific features. Here are
some examples:

- To select the effect of <code class="fixed" style="white-space:pre-wrap">--opt</code> except for some features,
 use the <code class="fixed" style="white-space:pre-wrap">--skip</code> option for each feature. To disable
 extended inserts and memory buffering, use <code class="fixed" style="white-space:pre-wrap">--opt</code>
 <code class="fixed" style="white-space:pre-wrap">--skip-extended-insert</code> <code class="fixed" style="white-space:pre-wrap">--skip-quick</code>.
 (Actually, <code class="fixed" style="white-space:pre-wrap">--skip-extended-insert</code>
 <code class="fixed" style="white-space:pre-wrap">--skip-quick</code> is sufficient because
 <code class="fixed" style="white-space:pre-wrap">--opt</code> is on by default.)
- To reverse <code class="fixed" style="white-space:pre-wrap">--opt</code> for all features except index disabling
 and table locking, use <code class="fixed" style="white-space:pre-wrap">--skip-opt</code>
 <code class="fixed" style="white-space:pre-wrap">--disable-keys</code> <code class="fixed" style="white-space:pre-wrap">--lock-tables</code>.

When you selectively enable or disable the effect of a group option, order is
important because options are processed first to last. For example,
 <code class="fixed" style="white-space:pre-wrap">--disable-keys</code> <code class="fixed" style="white-space:pre-wrap">--lock-tables</code>
 <code class="fixed" style="white-space:pre-wrap">--skip-opt</code> would not have the intended effect; it is the
same as <code class="fixed" style="white-space:pre-wrap">--skip-opt</code> by itself.

#### Special Characters in Option Values

Some options, like `--lines-terminated-by`, accept a string. The string can be quoted, if necessary. For example, on Unix systems this is the option to enclose fields within double quotes:

```sql
--fields-enclosed-by='"'
```

An alternative to specify the hexadecimal value of a character. For example, the following syntax works on any platform:

```sql
--fields-enclosed-by=0x22
```

### Option Files

In addition to reading options from the command-line, `mysqldump` can also read options from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/). If an unknown option is provided to `mysqldump` in an option file, then it is ignored.

The following options relate to how MariaDB command-line tools handles option files. They must be given as the first argument on the command-line:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--print-defaults</code></td><td>Print the program argument list and exit.</td></tr>
<tr><td><code>--no-defaults</code></td><td>Don't read default options from any option file.</td></tr>
<tr><td><code>--defaults-file=# </code></td><td>Only read default options from the given file #.</td></tr>
<tr><td><code>--defaults-extra-file=# </code></td><td>Read this file after the global files are read.</td></tr>
<tr><td><code>--defaults-group-suffix=# </code></td><td>In addition to the default option groups, also read option groups with this suffix.</td></tr>
</tbody></table>

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, `mysqldump` is linked with [MariaDB Connector/C](/kb/en/about-mariadb-connector-c/). However, MariaDB Connector/C does not yet handle the parsing of option files for this client. That is still performed by the server option file parsing code. See  [MDEV-19035](https://jira.mariadb.org/browse/MDEV-19035) for more information.

#### Option Groups

`mysqldump` reads options from the following [option groups](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

<table><tbody><tr><th>Group</th><th>Description</th></tr>
<tr><td><code>[mysqldump]</code></td><td>&nbsp;Options read by <code>mysqldump</code>, which includes both MariaDB Server and MySQL Server.</td></tr>
<tr><td><code>[mariadb-dump]</code></td><td>Options read by <code><a href="/kb/en/mysqldump/">mysqldump</a></code>. Available starting with <a href="/kb/en/mariadb-1046-release-notes/">MariaDB 10.4.6</a>.</td></tr>
<tr><td><code>[client]</code></td><td>&nbsp;Options read by all MariaDB and MySQL <a href="/kb/en/clients-utilities/">client programs</a>, which includes both MariaDB and MySQL clients. For example, <code>mysqldump</code>.</td></tr>
<tr><td><code>[client-server]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a> and the MariaDB Server. This is useful for options like socket and port, which is common between the server and the clients.</td></tr>
<tr><td><code>[client-mariadb]</code></td><td>Options read by all MariaDB <a href="/kb/en/clients-utilities/">client programs</a>.</td></tr>
</tbody></table>

### NULL, ´NULL´, and Empty Values in XML

For a column named `column_name`, the `NULL` value, an empty string, and
the string value `´NULL´` are distinguished from one another in the output
generated by this option as follows.

<table><tbody><tr><th>Value</th><th>XML Representation</th></tr>
<tr><td>NULL (unknown value)</td><td><code class="fixed" style="white-space:pre-wrap"><span class="nt">&lt;field</span> <span class="na">name=</span><span class="s">"column_name"</span> <span class="na">xsi:nil=</span><span class="s">"true"</span> <span class="nt">/&gt;</span>
</code></td></tr>
<tr><td>´´ (empty string)</td><td><code class="fixed" style="white-space:pre-wrap"><span class="nt">&lt;field</span> <span class="na">name=</span><span class="s">"column_name"</span><span class="nt">&gt;&lt;/field&gt;</span>
</code></td></tr>
<tr><td>´NULL´ (string value)</td><td><code class="fixed" style="white-space:pre-wrap"><span class="nt">&lt;field</span> <span class="na">name=</span><span class="s">"column_name"</span><span class="nt">&gt;</span>NULL<span class="nt">&lt;/field&gt;</span>
</code></td></tr>
</tbody></table>

The output from the mysql client when run using
the `--xml` option also follows the preceding rules.

XML output from mysqldump includes the XML namespace, as shown here :

```sql
shell> mysqldump --xml -u root world City
<?xml version="1.0"?>
<mysqldump xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
<database name="world">
<table_structure name="City">
<field Field="ID" Type="int(11)" Null="NO" Key="PRI" Extra="auto_increment" />
<field Field="Name" Type="char(35)" Null="NO" Key="" Default="" Extra="" />
<field Field="CountryCode" Type="char(3)" Null="NO" Key="" Default="" Extra="" />
<field Field="District" Type="char(20)" Null="NO" Key="" Default="" Extra="" />
<field Field="Population" Type="int(11)" Null="NO" Key="" Default="0" Extra="" />
<key Table="City" Non_unique="0" Key_name="PRIMARY" Seq_in_index="1" Column_name="ID"
Collation="A" Cardinality="4079" Null="" Index_type="BTREE" Comment="" />
<options Name="City" Engine="MyISAM" Version="10" Row_format="Fixed" Rows="4079"
Avg_row_length="67" Data_length="273293" Max_data_length="18858823439613951"
Index_length="43008" Data_free="0" Auto_increment="4080"
Create_time="2007-03-31 01:47:01" Update_time="2007-03-31 01:47:02"
Collation="latin1_swedish_ci" Create_options="" Comment="" />
</table_structure>
<table_data name="City">
<row>
<field name="ID">1</field>
<field name="Name">Kabul</field>
<field name="CountryCode">AFG</field>
<field name="District">Kabol</field>
<field name="Population">1780000</field>
</row>
...
<row>
<field name="ID">4079</field>
<field name="Name">Rafah</field>
<field name="CountryCode">PSE</field>
<field name="District">Rafah</field>
<field name="Population">92020</field>
</row>
</table_data>
</database>
</mysqldump>
```

### Restoring

To restore a backup created with mysqldump, use the [mysql client](/clients-utilities/mysql-client/mysql-command-line-client/) to import the dump, for example:

```sql
mysql db_name < backup-file.sql
```

## Variables

You can also set the following variables
(`--variable-name=value`) and boolean options `{FALSE|TRUE}` by using:

<table><tbody><tr><th>Name</th><th>Default Values</th><th>Description</th></tr>
<tr><td><code>all</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>all-databases</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>all-tablespaces</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>no-tablespaces</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>add-drop-database</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>add-drop-table</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>add-locks</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>allow-keywords</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>character-sets-dir</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>comments</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>compatible</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>compact</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>complete-insert</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>compress</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>create-options</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>databases</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>debug-check</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>debug-info</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>default-character-set</code></td><td><code>utf8mb4</code></td><td><code>utf8</code> until <a href="/kb/en/mariadb-10311-release-notes/">MariaDB 10.3.11</a></td></tr>
<tr><td><code>delayed-insert</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>delete-master-logs</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>disable-keys</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>events</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>extended-insert</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>fields-terminated-by</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>fields-enclosed-by</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>fields-optionally-enclosed-by</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>fields-escaped-by</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>first-slave</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>flush-logs</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>flush-privileges</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>force</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>hex-blob</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>host</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>insert-ignore</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>include-master-host-port</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>lines-terminated-by</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>lock-all-tables</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>lock-tables</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>log-error</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>log-queries</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>master-data</code></td><td><code>0</code></td><td></td></tr>
<tr><td><code>max_allowed_packet</code></td><td><code>25165824</code></td><td>The maximum size of the buffer for client/server communication. The maximum is 1GB.</td></tr>
<tr><td><code>net_buffer_length</code></td><td><code>1046528</code></td><td>The initial size of the buffer for client/server communication. When creating multiple-row <code>INSERT</code> statements (as with the <code class="fixed" style="white-space:pre-wrap">--extended-insert</code> or <code class="fixed" style="white-space:pre-wrap">--opt</code> option), <code>mysqldump</code> creates rows up to <code>net_buffer_length</code> length. If you increase this variable, you should also ensure that the <code>net_buffer_length</code> variable in the MariaDB server is at least this large.</td></tr>
<tr><td><code>no-autocommit</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>no-create-db</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>no-create-info</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>no-data</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>order-by-primary</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>port</code></td><td><code>0</code></td><td></td></tr>
<tr><td><code>quick</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>quote-names</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>replace</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>routines</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>set-charset</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>single-transaction</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>dump-date</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>socket</code></td><td><em>No default value)</em></td><td></td></tr>
<tr><td><code>ssl</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>ssl-ca</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>ssl-capath</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>ssl-cert</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>ssl-cipher</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>ssl-key</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>ssl-verify-server-cert</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>system</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>tab</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>triggers</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>tz-utc</code></td><td><code>TRUE</code></td><td></td></tr>
<tr><td><code>user</code></td><td><em>(No default value)</em></td><td></td></tr>
<tr><td><code>verbose</code></td><td><code>FALSE</code></td><td></td></tr>
<tr><td><code>where</code></td><td><em>(No default value)</em></td><td></td></tr>
</tbody></table>

## Examples

A common use of `mysqldump` is for making a backup of an entire database:

```sql
shell> mysqldump db_name > backup-file.sql
```

You can load the dump file back into the server like this:

```sql
shell> mysql db_name < backup-file.sql
```

Or like this:

```sql
shell> mysql -e "source /path-to-backup/backup-file.sql" db_name
```

`mysqldump` is also very useful for populating databases by
copying data from one MariaDB server to another:

```sql
shell> mysqldump --opt db_name | mysql --host=remote_host -C db_name
```

It is possible to dump several databases with one command:

```sql
shell> mysqldump --databases db_name1 [db_name2 ...] > my_databases.sql
```

To dump all databases, use the <code class="fixed" style="white-space:pre-wrap">--all-databases</code> option:

```sql
shell> mysqldump --all-databases > all_databases.sql
```

For InnoDB tables, `mysqldump` provides a way of making an
online backup:

```sql
shell> mysqldump --all-databases --single-transaction all_databases.sql
```

This backup acquires a global read lock on all tables (using
<code class="fixed" style="white-space:pre-wrap">FLUSH TABLES WITH READ LOCK</code>) at the beginning of the dump. As
soon as this lock has been acquired, the binary log coordinates are read and
the lock is released. If long updating statements are running when the FLUSH
statement is issued, the MariaDB server may get stalled until those statements
finish. After that, the dump becomes lock free and does not disturb reads and
writes on the tables. If the update statements that the MariaDB server receives
are short (in terms of execution time), the initial lock period should not be
noticeable, even with many updates.

For point-in-time recovery (also known as “roll-forward,” when you need to
restore an old backup and replay the changes that happened since that backup),
it is often useful to rotate the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) or at least know the binary log coordinates to which the dump corresponds:

```sql
shell> mysqldump --all-databases --master-data=2 > all_databases.sql
```

Or:

```sql
shell> mysqldump --all-databases --flush-logs --master-data=2 > all_databases.sql
```

The <code class="fixed" style="white-space:pre-wrap">--master-data</code> and <code class="fixed" style="white-space:pre-wrap">--single-transaction</code>
options can be used simultaneously, which provides a convenient way to make an
online backup suitable for use prior to point-in-time recovery if tables are
stored using the InnoDB storage engine.

## See Also

- [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/)
- [MariaDB Enterprise Backup](https://mariadb.com/docs/usage/mariadb-enterprise-backup/)
- [Upgrading to a newer major version of MariaDB](https://www.youtube.com/watch?v=1kLIXN2DoEo) (video)