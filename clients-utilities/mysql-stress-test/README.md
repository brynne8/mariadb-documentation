# mysql-stress-test

<em>mysql-stress-test.pl</em> is a Perl script that performs stress-testing of the MariaDB server. It requires a version of Perl that has been built with threads support.

## Syntax

```sql
mysql-stress-test.pl [options]
```

## Options

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--help</code></td><td>Display a help message and exit.</td></tr>
<tr><td><code>--abort-on-error=N</code></td><td>Causes the program to abort if an error with severity less than or equal to N was encountered. Set to 1 to abort on any error.</td></tr>
<tr><td><code>--check-tests-file</code></td><td>Periodically check the file that lists the tests to be run. If it has been modified, reread the file. This can be useful if you update the list of tests to be run during a stress test.</td></tr>
<tr><td><code>--cleanup</code></td><td>Force cleanup of the working directory.</td></tr>
<tr><td><code>--log-error-details</code></td><td>Log error details in the global <a href="/kb/en/error-log/">error log</a> file.</td></tr>
<tr><td><code>--loop-count=N</code></td><td>In sequential test mode, the number of loops to execute before exiting.</td></tr>
<tr><td><code>--mysqltest=path</code></td><td>The path name to the <a href="/kb/en/mysqltest/">mysqltest</a> program.</td></tr>
<tr><td><code>--server-database=db_name</code></td><td>The database to use for the tests. The default is test.</td></tr>
<tr><td><code>--server-host=host_name</code></td><td>he host name of the local host to use for making a TCP/IP connection to the local server. By default, the connection is made to localhost using a Unix socket file.</td></tr>
<tr><td><code>--server-logs-dir=path</code></td><td>This option is required.  path is the directory where all client session logs will be stored. Usually this is the shared directory that is associated with the server used for testing.</td></tr>
<tr><td><code>--server-password=password</code></td><td>The password to use when connecting to the server.</td></tr>
<tr><td><code>--server-port=port_num</code></td><td>The TCP/IP port number to use for connecting to the server. The default is 3306.</td></tr>
<tr><td><code>--server-socket=file_name</code></td><td>For connections to localhost, the Unix socket file to use, or, on Windows, the name of the named pipe to use. The default is <code>/tmp/mysql.sock</code>.</td></tr>
<tr><td><code>--server-user=user_name</code></td><td>The MariaDB user name to use when connecting to the server. The default is root.</td></tr>
<tr><td><code>--sleep-time=N</code></td><td>The delay in seconds between test executions.</td></tr>
<tr><td><code>--stress-basedir=path</code></td><td>This option is required and specified the path is the working directory for the test run. It is used as the temporary location for result tracking during testing.</td></tr>
<tr><td><code>--stress-datadir=path</code></td><td>The directory of data files to be used during testing. The default location is the data directory under the location given by the <code>--stress-suite-basedir</code> option.</td></tr>
<tr><td><code>--stress-init-file[=path]</code></td><td><em>file_name</em> is the location of the file that contains the list of tests to be run once to initialize the database for the testing. If missing, the default file is <code>stress_init.txt</code> in the test suite directory.</td></tr>
<tr><td><code>--stress-mode=mode</code></td><td>This option indicates the test order in stress-test mode. The mode value is either <code>random</code> to select tests in random order or <code>seq</code> to run tests in each thread in the order specified in the test list file. The default mode is <code>random</code>.</td></tr>
<tr><td><code>--stress-suite-basedir=path</code></td><td>This option is required and specifies the directory that has the <em>t</em> and <em>r</em> subdirectories containing the test case and result files. This directory is also the default location of the <code>stress-test.txt</code> file that contains the list of tests. (A different location can be specified with the <code>--stress-tests-file</code> option.)</td></tr>
<tr><td><code>--stress-tests-file[=file_name]</code></td><td>Use this option to run the stress tests. <em>file_name</em> is the location of the file that contains the list of tests. If omitted, the default file is <code>stress-test.txt</code> in the stress suite directory. (See <code>--stress-suite-basedir</code>.)</td></tr>
<tr><td><code>--suite=suite_name</code></td><td>Run the named test suite. The default name is <code>main</code> (the regular test suite located in the <code>mysql-test</code> directory).</td></tr>
<tr><td><code>--test-count=N</code></td><td>The number of tests to execute before exiting.</td></tr>
<tr><td><code>--test-duration=N</code></td><td>The duration of stress testing in seconds.</td></tr>
<tr><td><code>--threads=N</code></td><td>The number of threads. The default is 1.</td></tr>
<tr><td><code>--verbose</code></td><td>Verbose mode. Print more information about what the program does</td></tr>
</tbody></table>