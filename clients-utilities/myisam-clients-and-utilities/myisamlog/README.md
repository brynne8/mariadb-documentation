# myisamlog

`myisamlog` processes and returns the contents of a [MyISAM log file](/mariadb-administration/server-monitoring-logs/myisam-log/).

Invoke `myisamlog` like this:

```sql
shell> myisamlog [options] [log_file [tbl_name] ...]
shell> isamlog [options] [log_file [tbl_name] ...]
```

The default operation is update (`-u`). If a recovery is done (`-r`), all
writes and possibly updates and deletes are done and errors are only counted.
The default log file name is `myisam.log` for `myisamlog` and `isam.log`
for `isamlog` if no `log_file` argument is given. If tables are named on
the command line, only those tables are updated.

`myisamlog` supports the following options:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-?</code>, <code>-I</code></td><td>Display a help message and exit.</td></tr>
<tr><td><code>-c <em>N</em></code></td><td>Execute only <em><code>N</code></em> commands.</td></tr>
<tr><td><code>-f <em>N</em></code></td><td>Specify the maximum number of open files.</td></tr>
<tr><td><code>-i</code></td><td>Display extra information before exiting.</td></tr>
<tr><td><code>-o <em>offset</em></code></td><td>Specify the starting offset.</td></tr>
<tr><td><code>-p <em>N</em></code></td><td>Remove <em><code>N</code></em> components from path.</td></tr>
<tr><td><code>-r</code></td><td>Perform a recovery operation.</td></tr>
<tr><td><code>-R <em>record_pos_file record_pos</em></code></td><td>Specify record position file and record position.</td></tr>
<tr><td><code>-u</code></td><td>Displays update operations.</td></tr>
<tr><td><code>-v</code></td><td>Verbose mode. Print more output about what the program does. This option can be given multiple times (<em>-vv</em>, <em>-vvv</em>) to produce more and more output.</td></tr>
<tr><td><code>-w <em>write_file</em></code></td><td>Specify the write file.</td></tr>
<tr><td><code>-V</code></td><td>Display version information.</td></tr>
</tbody></table>