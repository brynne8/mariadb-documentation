# Pausing mysql-test-run.pl

Sometimes you need to work when your computer is busy running
[mysql-test-run.pl](/clients-utilities/mysqltest/mysql-test-runpl-options). The mysql-test-run.pl script allows you to stop it temporarily so you can use
your computer and then restart the tests when you're ready.

There are two ways to enable this:

1 <strong>Command-line:</strong> The <code class="highlight fixed" style="white-space:pre-wrap">--stop-file</code> and
  <code class="highlight fixed" style="white-space:pre-wrap">--stop-keep-alive</code> options.
2 <strong>Environment Variables:</strong> If you are calling mysql-test-run.pl indirectly
  (i.e from a script or program such as buildbot) you can set
  <code class="highlight fixed" style="white-space:pre-wrap">MTR_STOP_FILE</code> and <code class="highlight fixed" style="white-space:pre-wrap">MTR_STOP_KEEP_ALIVE</code>.

### Keep Alive

If you plan on using this feature with other programs, such as buildbot, you should set the &lt;code&gt;MTR_STOP_KEEP_ALIVE&lt;/code&gt; environment variable or the &lt;code&gt;--stop-keep-alive&lt;/code&gt; command-line option with a value in seconds. This will make the script print messages to whatever program is calling mysql-test-run.pl at the interval you set to prevent timeouts.

If you are calling mysql-test-run.pl directly, you do not need to specify a timeout.

### The mysql-test-run Stop File

The stop file is a temporary file that you create on your system when you want
to pause the execution of mysql-test-run. When enabled via the command-line or
environment variable options, mysql-test-run will periodically check for the
existence of the file and if it exists it will stop until the file is no longer
present.

### Examples

Command-line:

```sql
mysql-test-run.pl --stop-file="/path/to/stop/file" --stop-keep-alive=120
```

Environment Variables:

```sql
export MTR_STOP_FILE="/path/to/stop/file"
export MTR_STOP_KEEP_ALIVE=120
mysql-test-run.pl
```

### Fixes

The following mysql-test-run bugs have been fixed in [MariaDB 5.1](/kb/en/what-is-mariadb-51/):

- Windows: mysql-test-run --log-error fixed to not add --console.
- mysql-test-run sometimes terminated mysqld early, causing loss of memory leak error reports from Valgrind and GCov test coverage output