# mysql-test-run.pl Options

## Syntax

```sql
./mysql-test-run.pl [ OPTIONS ] [ TESTCASE ]
```

Where the test case can be specified as:
`testcase[.test]` Runs the test case named 'testcase' from all suits

```sql
path-to-testcase
[suite.]testcase[,combination]
```

### Examples

`alias`
`main.alias` 'main' is the name of the suite for the 't' directory.

```sql
rpl.rpl_invoked_features,mix,xtradb_plugin
suite/rpl/t/rpl.rpl_invoked_features
```

## Options

### Options to Control What Engine/Variation to Run

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--embedded-server</code></td><td>Use the embedded server, i.e. no mysqld daemons.</td></tr>
<tr><td><code>--ps-protocol</code></td><td>Use the binary protocol between client and server.</td></tr>
<tr><td><code>--cursor-protocol</code></td><td>Use the cursor protocol between client and server (implies <code>--ps-protocol</code>).</td></tr>
<tr><td><code>--view-protocol</code></td><td>Create a view to execute all non updating queries.</td></tr>
<tr><td><code>--sp-protocol</code></td><td>Create a stored procedure to execute all queries.</td></tr>
<tr><td><code>--compress</code></td><td>Use the compressed protocol between client and server if both support it.</td></tr>
<tr><td><code>--ssl</code></td><td>If mysql-test-run.pl is started with the <code>--ssl</code> option, it sets up a <a href="/kb/en/secure-connections-overview/">secure connection</a> for all test cases. In this case, if mysqld does not support TLS, mysql-test-run.pl exits with an error message: <code>Couldn´t find support for SSL</code>.</td></tr>
<tr><td><code>--skip-ssl</code></td><td>Dont start server with support for TLS connections.</td></tr>
<tr><td><code>--vs-config</code></td><td>Visual Studio configuration used to create executables (default: MTR_VS_CONFIG environment variable).</td></tr>
<tr><td><code>--parallel=num</code></td><td>How many parallel tests should be run. Default is <code>1</code>, use <code>--parallel=auto</code> for auto-setting of <em>num</em>.</td></tr>
<tr><td><code>--defaults-file=&lt;config template&gt;</code></td><td>Use fixed config template for all tests.</td></tr>
<tr><td><code>--defaults_extra_file=&lt;config template&gt;</code></td><td>Extra config template to add to all generated configs.</td></tr>
<tr><td><code>--combination=&lt;opt&gt;</code></td><td>Extra options to pass to mysqld. The value should consist of one or more comma-separated mysqld options. This option is similar to <code>--mysqld</code> but should be given two or more times. mysql-test-run.pl executes multiple test runs, using the options for each instance of <code>--combination</code> in successive runs. If <code>--combination</code> is given only once, it has no effect. For test runs specific to a given test suite, an alternative to the use of <code>--combination</code> is to create a combinations file in the suite directory. The file should contain a section of options for each test run.</td></tr>
<tr><td><code>--dry-run</code></td><td>Don't run any tests, print the list of tests that were selected for execution.</td></tr>
</tbody></table>

### Options to Control Directories to Use

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--tmpdir=DIR</code></td><td>The directory where temporary files are stored (default: ./var/tmp). The environment variable MYSQL_TMP_DIR will be set to the path for this directory, whether it has the default value or has been set explicitly. This may be referred to in tests.</td></tr>
<tr><td><code>--vardir=DIR</code></td><td>The directory where files generated from the test run is stored (default: ./var). Specifying a ramdisk or tmpfs will speed up tests. The environment variable MYSQLTEST_VARDIR will be set to the path for this directory, whether it has the default value or has been set explicitly. This may be referred to in tests.</td></tr>
<tr><td><code>--mem</code></td><td>Run testsuite in "memory" using tmpfs or ramdisk. This can decrease test times significantly, in particular if you would otherwise be running over a remote file system. Attempts to find a suitable location using a builtin list of standard locations for tmpfs (/dev/shm). The option can also be set using environment variable MTR_MEM=[DIR]. If DIR is given, it is added to the beginning of the list of locations to search, so it takes precedence over any built-in locations. Once you have run tests with --mem within a mysql-testdirectory, a soflink var will have been set up to the temporary directory, and this will be re-used the next time, until the soflink is deleted. Thus, you do not have to repeat the <code>--mem</code> option next time.</td></tr>
<tr><td><code>--client-bindir=PATH</code></td><td>Path to the directory where client binaries are located.</td></tr>
<tr><td><code>--client-libdir=PATH</code></td><td>Path to the directory where client libraries are located.</td></tr>
</tbody></table>

### Options to Control What Test Suites or Cases to Run

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--force</code></td><td>Normally, mysql-test-run.pl exits if a test case fails.  <code>--force</code> causes execution to continue regardless of test case failure.</td></tr>
<tr><td><code>--with-ndbcluster-only</code></td><td>Run only tests that include "ndb" in the filename.</td></tr>
<tr><td><code>--skip-ndb[cluster]</code></td><td>Skip all tests that need cluster. Default.</td></tr>
<tr><td><code>--do-test=PREFIX or REGEX</code></td><td>Run test cases with names prefixed with PREFIX or which fulfil the REGEX. For example, <code>--do-test=testa</code> matches tests that begin with <em>testa</em>, <code>--do-test=main.testa</code> matches tests in the main test suite that begin with <em>testa</em>, and <code>--do-test=main.*testa</code> matches test names that contain <em>main</em> followed by <em>testa</em> with anything in between. In the latter case, the pattern match is not anchored to the beginning of the test name, so it also matches names such as <em>xmainytestz</em>.</td></tr>
<tr><td><code>--skip-test=PREFIX or REGEX</code></td><td>Skip test cases with names prefixed with PREFIX or which fulfil the REGEX. See <code>-do-test</code> for examples.</td></tr>
<tr><td><code>--start-from=PREFIX</code></td><td>Sorts the list of names of the test cases to be run, and then starts with the test prefixed with PREFIX, where the prefix may be <em>suite.testname</em> or just <em>testname</em>.</td></tr>
<tr><td><code>--suite[s]=NAME1,..,NAMEN</code></td><td>Comma separated list of suite names to run. The default, as of <a href="/kb/en/mariadb-1045-release-notes/">MariaDB 10.4.5</a>, is:<br>"main-, archive-, binlog-, binlog_encryption-, csv-, compat/oracle-, encryption-, federated-, funcs_1-, funcs_2-, gcol-, handler-, heap-, innodb-, innodb_fts-, innodb_gis-, innodb_zip-, json-, maria-, mariabackup-, multi_source-, optimizer_unfixed_bugs-, parts-, perfschema-, plugins-, roles-, rpl-, sys_vars-, sql_sequence-, unit-, vcol-, versioning-,period-".</td></tr>
<tr><td><code>--skip-rpl</code></td><td>Skip the replication test cases.</td></tr>
<tr><td><code>--big-test</code></td><td>Allow tests marked as "big" to run. Tests can be thus marked by including the line <code>--source include/big_test.inc</code>, and they will only be run if this option is given, or if the environment variable BIG_TEST is set to 1. Repeat this option twice to run only "big" tests. This is typically used for tests that take a very long to run, or that use many resources, so that they are not suitable for running as part of a normal test suite run</td></tr>
<tr><td><code>--staging-run</code></td><td>Run a limited number of tests (no slow tests). Used for running staging trees with valgrind.</td></tr>
<tr><td><code>--enable-disabled</code></td><td>Ignore any <em>disabled.def</em> file, and also run tests marked as disabled. Success or failure of those tests will be reported the same way as other tests.</td></tr>
<tr><td><code>--print-testcases</code></td><td>Don't run the tests but print details about all the selected tests, in the order they would be run.</td></tr>
<tr><td><code>--skip-test-list=FILE</code></td><td>Skip the tests listed in FILE. Each line in the file is an entry and should be formatted as: &lt;TESTNAME&gt; : &lt;COMMENT&gt;</td></tr>
</tbody></table>

### Options That Specify Ports

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--[mtr-]port-base=num</code></td><td>Base for port numbers. Ports from this number to number+9 are reserved. Should be divisible by 10; if not it will be rounded down. May be set with environment variable MTR_PORT_BASE. If this value is set and is not "auto", it overrides build-thread.</td></tr>
<tr><td><code>--[mtr-]build-thread=num</code></td><td>Specify unique number to calculate port number(s) from. Can be set in environment variable MTR_BUILD_THREAD. Set  MTR_BUILD_THREAD="auto" to automatically acquire a build thread id that is unique to current host. The more logical <code>--port-base</code> is supported as an alternative.</td></tr>
</tbody></table>

### Options For Test Case Authoring

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--record TESTNAME </code></td><td>(Re)generate the result file for TESTNAME.</td></tr>
<tr><td><code>--check-testcases</code></td><td>Check testcases for side-effects. This is done by checking system state before and after each test case; if there is any difference, a warning to that effect will be written, but the test case will not be marked as failed because of it. This check is enabled by default. Use <code>--nocheck-testcases</code> to disable.</td></tr>
<tr><td><code>mark-progress</code></td><td>Log line number and elapsed time to &lt;testname&gt;.progress</td></tr>
</tbody></table>

### Options That Pass On Options

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--mysqld=ARGS</code></td><td>Specify additional arguments to "mysqld"</td></tr>
<tr><td><code>--mysqltest=ARGS</code></td><td>Specify additional arguments to "mysqltest". Use additional <code>--mysqld-env</code> options to set more than one variable.</td></tr>
</tbody></table>

### Options to Run Test On Running Server

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>extern option=value</code></td><td>Use an already running server. The option/value pair is what is needed by the mysql client to connect to the server. Each <code>--extern</code> option can only take one option/value pair as an argument, so you need to repeat <code>--extern</code> for each pair needed. Example: <code> ./mysql-test-run.pl --extern socket=var/tmp/mysqld.1.sock alias</code>. Note: If a test case has an .opt file that requires the server to be restarted with specific options, the file will not be used. The test case likely will fail as a result.</td></tr>
</tbody></table>

### Options For Debugging the Product

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--boot-dbx</code></td><td>Run the mysqld server used for bootstrapping the database through the dbx debugger.</td></tr>
<tr><td><code>--boot-ddd</code></td><td>Run the mysqld server used for bootstrapping the database through the ddd debugger.</td></tr>
<tr><td><code>--boot-gdb</code></td><td>Run the mysqld server used for bootstrapping the database through the gdb debugger using a new xterm window.</td></tr>
<tr><td><code>--client-dbx</code></td><td>Start <a href="/kb/en/mysqltest/">mysqltest</a> client in the dbx debugger.</td></tr>
<tr><td><code>--client-ddd</code></td><td>Start <a href="/kb/en/mysqltest/">mysqltest</a> client in the ddd debugger.</td></tr>
<tr><td><code>--client-debugger=NAME</code></td><td>Start <a href="/kb/en/mysqltest/">mysqltest</a> in the selected debugger.</td></tr>
<tr><td><code>--client-gdb</code></td><td>Start <a href="/kb/en/mysqltest/">mysqltest</a> client in the gdb debugger using a new xterm window.</td></tr>
<tr><td><code>--dbx</code></td><td>Start the mysqld(s) in the dbx debugger.</td></tr>
<tr><td><code>--ddd</code></td><td>Start the mysqld(s) in the ddd debugger.</td></tr>
<tr><td><code>--debug</code></td><td>Dump trace output for all servers and client programs.</td></tr>
<tr><td><code>--debug-common</code></td><td>Same as <code>--debug</code>, but sets the 'd' debug flags to "query,info,error,enter,exit"</td></tr>
<tr><td><code>--debug-server</code></td><td>Use debug version of server, but without turning on tracing.</td></tr>
<tr><td><code>--debugger=NAME</code></td><td>Start mysqld in the selected debugger.</td></tr>
<tr><td><code>--gdb[=COMMANDS]</code></td><td>Start the mysqld(s) in the gdb debugger using a new xterm window. From <a href="/kb/en/mariadb-1036-release-notes/">MariaDB 10.3.6</a>, optionally execute specified commands (semicolon-separated) in gdb.</td></tr>
<tr><td><code>--manual-debug</code></td><td>Let user manually start mysqld in debugger, before running test(s).</td></tr>
<tr><td><code>--manual-dbx</code></td><td>Let user manually start mysqld in the dbx debugger, before running test(s).</td></tr>
<tr><td><code>--manual-ddd</code></td><td>Let user manually start mysqld in the ddd debugger, before running test(s).</td></tr>
<tr><td><code>--manual-gdb</code></td><td>Let user manually start mysqld in the gdb debugger, before running test(s).</td></tr>
<tr><td><code>--manual-lldb</code></td><td>Let user manually start mysqld in the lldb debugger, before running test(s).</td></tr>
<tr><td><code>--max-save-core</code></td><td>Limit the number of core files saved (to avoid filling up disks for heavily crashing server). Defaults to 5, set to 0 for no limit. Set its default with MTR_MAX_SAVE_CORE.</td></tr>
<tr><td><code>--max-save-datadir</code></td><td>Limit the number of datadir saved (to avoid filling up disks for heavily crashing server). Defaults to 20, set to 0 for no limit. Set its default with MTR_MAX_SAVE_DATDIR.</td></tr>
<tr><td><code>--max-test-fail</code></td><td>Limit the number of test failurs before aborting the current test run. Defaults to 10, set to 0 for no limit. Set its default with MTR_MAX_TEST_FAIL.</td></tr>
</tbody></table>

### Options For Valgrind

To get less warnings when running the test system with [Valgrind](http://www.valgrind.org), you should consider compiling MariaDB with the `BUILD/compile-pentium64-valgrind-max` script. This will initialize some structures, that normally don't have to be initialized, to remove some false positive warnings from Valgrind.

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--valgrind</code></td><td>Run the "mysqltest" and "mysqld" executables using valgrind with default options. This and the following valgrind options require that the executables have been built with valgrind support.</td></tr>
<tr><td><code>--valgrind-all</code></td><td>Synonym for --valgrind.</td></tr>
<tr><td><code>--valgrind-mysqld</code></td><td>Run the "mysqld" executable with valgrind.</td></tr>
<tr><td><code>--valgrind-mysqltest</code></td><td>Run the "mysqltest" and "mysql_client_test" executables with valgrind.</td></tr>
<tr><td><code>--valgrind-options=ARGS</code></td><td>Deprecated, use <code>--valgrind-option</code>.</td></tr>
<tr><td><code>--valgrind-option=ARGS</code></td><td>Option to give valgrind. Replaces default option(s). Can be specified more then once.</td></tr>
<tr><td><code>--valgrind-path=&lt;EXE&gt;</code></td><td>Path to the valgrind executable.</td></tr>
<tr><td><code>--callgrind</code></td><td>Instruct valgrind to use callgrind.</td></tr>
</tbody></table>

### Options For Strace

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--strace</code></td><td>Run the "mysqld" executables using strace. Default options are <code>-f -o 'var'/log/'mysqld-name'.strace</code>.</td></tr>
<tr><td><code>--client-strace</code></td><td>Trace the "mysqltest".</td></tr>
<tr><td><code>--strace-option=ARGS</code></td><td>Option to give strace, appends to existing options.</td></tr>
<tr><td><code>--stracer=&lt;EXE&gt;</code></td><td>Specify name and path to the trace program to use. Default is "strace". Example: <code>./mysql-test-run.pl --stracer=ktrace</code>.</td></tr>
</tbody></table>

### Options For rr (Record and Replay)

Record and replay options were added in [MariaDB 10.2.33](/kb/en/mariadb-10233-release-notes/), [MariaDB 10.3.24](/kb/en/mariadb-10324-release-notes/), [MariaDB 10.4.14](/kb/en/mariadb-10414-release-notes/) and [MariaDB 10.5.5](/kb/en/mariadb-1055-release-notes/).

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--rr</code></td><td>Run the "mysqld" executables using <code>rr</code>. Default run option is <code>rr record mysqld mysqld_options</code>.</td></tr>
<tr><td><code>--boot-rr</code></td><td>Start bootstrap server in rr.</td></tr>
<tr><td><code>--rr-arg=ARG</code></td><td>Option to give rr record, can be specified more then once.</td></tr>
<tr><td><code>--rr-dir=DIR</code></td><td>The directory where rr recordings are stored. Defaults to 'vardir'/rr.0 (rr.boot for bootstrap instance and rr.1, ..., rr.N for slave instances).</td></tr>
</tbody></table>

### Misc Options

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--user=USER</code></td><td>User for connecting to mysqld (default: root)</td></tr>
<tr><td><code>--comment=STR</code></td><td>Write STR to the output within lines filled with #, as a form of banner.</td></tr>
<tr><td><code>--timer</code></td><td>Show test case execution time. Use <code>no-timer</code> to disable.</td></tr>
<tr><td><code>--verbose</code></td><td>More verbose output(use multiple times for even more)</td></tr>
<tr><td><code>--verbose-restart</code></td><td>Write when and why servers are restarted between test cases.</td></tr>
<tr><td><code>--start</code></td><td>Only initialize and start the servers, using the startup settings for the first specified test case Example: <code>./mysql-test-run.pl --start alias &amp; start-dirty</code> Only start the servers (without initialization) for the first specified test case</td></tr>
<tr><td><code>--start-and-exit</code></td><td>Same as <code>--start</code>, but <em>mysql-test-run</em> terminates and leaves just the server running.</td></tr>
<tr><td><code>--start-dirty</code></td><td>This is similar to <code>--start</code>, but will skip the database initialization phase and assume that database files are already available. Usually this means you must have run another test first.</td></tr>
<tr><td><code>--user-args</code></td><td>In combination with start* and no test name, drops arguments to mysqld except those specified with <code>--mysqld</code> (if any).</td></tr>
<tr><td><code>--wait-all</code></td><td>If <code>--start</code> or <code>--start-dirty</code> option is used, wait for all servers to exit before finishing the process. Otherwise, it will terminate if one (of several) servers is restarted.</td></tr>
<tr><td><code>--fast</code></td><td>Do not perform controlled shutdown when servers need to be restarted or at the end of the test run. This is equivalent to using <code>--shutdown-timeout=0</code>.</td></tr>
<tr><td><code>--force-restart</code></td><td>Always restart servers between tests.</td></tr>
<tr><td><code>--parallel=N</code></td><td>Run tests in N parallel threads (default 1) Use parallel=auto for auto-setting of N.</td></tr>
<tr><td><code>--repeat=N</code></td><td>Run each test N number of times.</td></tr>
<tr><td><code>--retry=N</code></td><td>If a test fails, it is retried up to a maximum of N runs (default 1). Retries are also limited by the maximum number of failures before stopping, set with the <code>--retry-failure</code> option. This option has no effect unless <code>--force</code> is also used; without it, test execution will terminate after the first failure. The <code>--retry</code> and <code>--retry-failure</code> options do not affect how many times a test repeated with <code>--repeat</code> may fail in total, as each repetition is considered a new test case, which may in turn be retried if it fails.</td></tr>
<tr><td><code>--retry-failure=N</code></td><td>When using the --retry option to retry failed tests, stop when N failures have occured (default 2). Setting it to 0 or 1 effectively turns off retries.</td></tr>
<tr><td><code>--reorder</code></td><td>Reorder tests to get fewer server restarts. This is the default behavior. There is no guarantee that a particular set of tests will always end up in the same order. Use <code>-no-reorder</code> to disable.</td></tr>
<tr><td><code>--help</code></td><td>Display help text.</td></tr>
<tr><td><code>--testcase-timeout=MINUTES</code></td><td>Max test case run time in minutes (default 15).</td></tr>
<tr><td><code>--suite-timeout=MINUTES</code></td><td>Max test suite run time in minutes (default 360).</td></tr>
<tr><td><code>--shutdown-timeout=SECONDS</code></td><td>Max number of seconds to wait for server shutdown before killing servers (default 10).</td></tr>
<tr><td><code>--warnings</code></td><td>Scan the log files for warnings and report any suspicious ones; if any are found, the test will be marked as failed. Use <code>--nowarnings</code> to turn off.</td></tr>
<tr><td><code>--stop-file=file</code></td><td>If this file is detected, <a href="/kb/en/mysqltest/">mysqltest</a> will not start new tests until the file is removed (also MTR_STOP_FILE environment variable).</td></tr>
<tr><td><code>--stop-keep-alive=sec</code></td><td>Works with <code>--stop-file</code>, print messages every <em>sec</em> seconds when mysqltest is waiting to remove the file (for buildbot) (also MTR_STOP_KEEP_ALIVE environment variable).</td></tr>
<tr><td><code>--sleep=SECONDS</code></td><td>Passed to <a href="/kb/en/mysqltest/">mysqltest</a>; will be used as fixed sleep time.</td></tr>
<tr><td><code>--debug-sync-timeout=NUM</code></td><td>Set default timeout for WAIT_FOR debug sync actions. Disable facility with NUM=0.</td></tr>
<tr><td><code>--gcov</code></td><td>Collect coverage information after the test. The result is a <a href="/kb/en/dgcov/">dgcov</a> file per source and header file and a <code>last_changes.dgcov</code> file in the vardir with the coverage for the uncommitted changes if any (or the last commit).</td></tr>
<tr><td><code>--gprof</code></td><td>Collect profiling information using the gprof profiling tool.</td></tr>
<tr><td><code>--experimental=&lt;file&gt;</code></td><td>Specify a file that contains a list of test cases that should be displayed with the [ exp-fail ] code rather than [ fail ] if they fail. For an example of a file that might be specified via this option, see <em>mysql-test/collections/default.experimental</em>.</td></tr>
<tr><td><code>--report-features</code></td><td>First run a "test" that reports MariaDB features, displaying the output of <a href="/kb/en/show-engines/">SHOW ENGINES</a> and <a href="/kb/en/show-variables/">SHOW VARIABLES</a>. This can be used to verify that binaries are built with all required features.</td></tr>
<tr><td><code>--timestamp</code></td><td>Print timestamp before each test report line, showing when the test ended.</td></tr>
<tr><td><code>--timediff</code></td><td>Used with <code>--timestamp</code>, also print time passed since the previous test started.</td></tr>
<tr><td><code>--max-connections=N</code></td><td>Maximum number of simultaneous server connections that may be used per test. Default is 128. Minimum is 8, maximum is 5120. Corresponds to the same option for <a href="/kb/en/mysqltest-and-mysqltest-embedded/">mysqltest</a>.</td></tr>
<tr><td><code>--default-myisam</code></td><td>Set default storage engine to <a href="/kb/en/myisam/">MyISAM</a> for non-innodb tests. This is needed after switching default storage engine to <a href="/kb/en/innodb/">InnoDB</a>.</td></tr>
<tr><td><code>--report-times</code></td><td>Report how much time has been spent on different phases of test execution.</td></tr>
<tr><td><code>--stress=ARGS</code></td><td>Run stress test, providing options to <a href="/kb/en/mysql-stress-test/">mysql-stress-test.pl</a>. Options are separated by comma.</td></tr>
<tr><td><code>xml-report=&lt;file&gt;</code></td><td>Output jUnit xml file of the results. From <a href="/kb/en/mariadb-10145-release-notes/">MariaDB 10.1.45</a>, <a href="/kb/en/mariadb-10232-release-notes/">MariaDB 10.2.32</a>, <a href="/kb/en/mariadb-10323-release-notes/">MariaDB 10.3.23</a>, <a href="/kb/en/mariadb-10413-release-notes/">MariaDB 10.4.13</a>, <a href="/kb/en/mariadb-1053-release-notes/">MariaDB 10.5.3</a></td></tr>
<tr><td><code>tail-lines=N</code></td><td>Number of lines of the result to include in a failure report. From <a href="/kb/en/mariadb-1034-release-notes/">MariaDB 10.3.4</a>.</td></tr>
</tbody></table>