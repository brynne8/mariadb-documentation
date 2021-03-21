# mysqlhotcopy

mysqlhotcopy is currently deprecated.

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/),  `mariadb-hotcopy` is a symlink to `mysqlhotcopy`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadb-hotcopy` is the name of the script, with `mysqlhotcopy` a symlink .

`mysqlhotcopy` is a Perl script that was originally written
and contributed by Tim Bunce. It uses [FLUSH TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/), [LOCK TABLES](/sql-statements-structure/sql-statements/transactions/lock-tables/), and cp or scp to make a database backup. It is a fast way to make a backup of the database or single tables, but it can be run only on the same machine where the database
directories are located. `mysqlhotcopy`&gt; works only for backing up [MyISAM](/kb/en/myisam/) and [ARCHIVE](/columns-storage-engines-and-plugins/storage-engines/archive/) tables. It runs on Unix and NetWare.

To use <code class="highlight fixed" style="white-space:pre-wrap">mysqlhotcopy</code>, you must have read access to the files
for the tables that you are backing up, the SELECT [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) for
those tables, the RELOAD privilege (to be able to execute FLUSH TABLES), and
the LOCK TABLES privilege (to be able to lock the tables).

```sql
shell> mysqlhotcopy db_name [/path/to/new_directory]
shell> mysqlhotcopy db_name_1 ... db_name_n /path/to/new_directory
```

Back up tables in the given database that match a regular expression:

```sql
shell> mysqlhotcopy db_name./regex/
```

The regular expression for the table name can be negated by prefixing it with a
tilde (“`~`”):

```sql
shell> mysqlhotcopy db_name./~regex/
```

<code class="highlight fixed" style="white-space:pre-wrap">mysqlhotcopy</code> supports the following options, which can be
specified on the command line or in the [<code class="highlight fixed" style="white-space:pre-wrap">mysqlhotcopy</code>] and
[<code class="highlight fixed" style="white-space:pre-wrap">client</code>] option file groups.

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--help</code>, <code>-?</code></td><td>Display a help message and exit.</td></tr>
<tr><td><code>--addtodest</code></td><td>Do not rename target directory (if it exists); merely add files to it.</td></tr>
<tr><td><code>--allowold</code></td><td>Do not abort if a target exists; rename it by adding an _old suffix.</td></tr>
<tr><td><code>--checkpoint=db_name.tbl_name</code></td><td>Insert checkpoint entries into the specified database <code>db_name</code> and table <code>tbl_name</code>.</td></tr>
<tr><td><code>--chroot=path</code></td><td>Base directory of the chroot jail in which mysqld operates. The path value should match that of the <code>--chroot</code> option given to mysqld.</td></tr>
<tr><td><code>--debug</code></td><td>Enable debug output.</td></tr>
<tr><td><code>--dryrun</code>, <code>-n</code></td><td>Report actions without performing them.</td></tr>
<tr><td><code>--flushlog</code></td><td>Flush logs after all tables are locked.</td></tr>
<tr><td><code>--host=host_name</code>, <code>-h host_name</code></td><td>The host name of the local host to use for making a TCP/IP connection to the local server. By default, the connection is made to localhost using a Unix socket file.</td></tr>
<tr><td><code>--keepold</code></td><td>Do not delete previous (renamed) target when done.</td></tr>
<tr><td><code>--method=command</code></td><td>The method for copying files (cp or scp). The default is cp.</td></tr>
<tr><td><code>--noindices</code></td><td>Do not include full index files for MyISAM tables in the backup. This makes the backup smaller and faster. The indexes for reloaded tables can be reconstructed later with <a href="/kb/en/myisamchk/">myisamchk -rq</a>.</td></tr>
<tr><td><code>--old-server</code></td><td>Connect to old MySQL-server (before v5.5) which doesn't have <a href="/kb/en/flush/">FLUSH TABLES WITH READ LOCK</a> fully implemented.</td></tr>
<tr><td><code>--password=password</code>, <code>-ppassword</code></td><td>The password to use when connecting to the server. The password value is not optional for this option, unlike for other MariaDB programs.<br><br> Specifying a password on the command line should be considered insecure. You can use an option file to avoid giving the password on the command line.</td></tr>
<tr><td><code>--port=port_num</code>, <code>-P port_num</code></td><td>The TCP/IP port number to use when connecting to the local server.</td></tr>
<tr><td><code>--quiet</code>, <code>-q</code></td><td>Be silent except for errors.</td></tr>
<tr><td><code>--record_log_pos=db_name.tbl_name</code></td><td>Record master and slave status in the specified database db_name and table tbl_name.</td></tr>
<tr><td><code>--regexp=expr</code></td><td>Copy all databases with names that match the given regular expression.</td></tr>
<tr><td><code>--resetmaster</code></td><td>Reset the binary log after locking all the tables.</td></tr>
<tr><td><code>--resetslave</code></td><td>Reset the master.info file after locking all the tables.</td></tr>
<tr><td><code>--socket=path</code>, <code>-S path</code></td><td>The Unix socket file to use for connections to localhost.</td></tr>
<tr><td><code>--suffix=str</code></td><td>The suffix to use for names of copied databases.</td></tr>
<tr><td><code>--tmpdir=path</code></td><td>The temporary directory. The default is /tmp.</td></tr>
<tr><td><code>--user=username</code>, <code>-u username</code></td><td>The MariaDB username to use when connecting to the server.</td></tr>
</tbody></table>

Use perldoc for additional <code class="highlight fixed" style="white-space:pre-wrap">mysqlhotcopy</code> documentation,
including information about the structure of the tables needed for the
<code class="highlight fixed" style="white-space:pre-wrap">--checkpoint</code> and <code class="highlight fixed" style="white-space:pre-wrap">--record_log_pos</code> options:

```sql
shell> perldoc mysqlhotcopy
```

## See Also

- [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/)
- [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/)