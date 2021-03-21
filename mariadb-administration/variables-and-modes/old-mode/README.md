# OLD_MODE

##### MySQL starting with 5.5.35

<code class="fixed" style="white-space:pre-wrap">OLD_MODE</code> was introduced in [MariaDB 5.5.35](/kb/en/mariadb-5535-release-notes/) to replace the <code class="fixed" style="white-space:pre-wrap">old</code> variable with a new one with better granularity.

MariaDB supports several different modes which allow you to tune it to suit your needs.

The most important ways for doing this are with [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) and `OLD_MODE`.

[SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) is used for getting MariaDB to emulate behavior from other SQL servers, while <code class="fixed" style="white-space:pre-wrap">OLD_MODE</code> is used for emulating behavior from older MariaDB or MySQL versions.

<code class="fixed" style="white-space:pre-wrap">OLD_MODE</code> is a string with different options separated by commas ('`,`') without spaces. The options are case insensitive.

Normally <code class="fixed" style="white-space:pre-wrap">OLD_MODE</code> should be empty. It's mainly used to get old behavior when switching to MariaDB or to a new major version of MariaDB, until you have time to fix your application.

Between major versions of MariaDB various options supported by <code class="fixed" style="white-space:pre-wrap">OLD_MODE</code> may be removed.  This is intentional as we assume that the application will be fixed to conform with the new MariaDB behavior between releases.

You can check the local and global value of it with:

```sql
SELECT @@OLD_MODE, @@GLOBAL.OLD_MODE;
```

You can set the <code class="fixed" style="white-space:pre-wrap"><span class="n">OLD_MODE</span>
</code> either from the
[command line](/kb/en/mysqld-options-full-list/) (option <code class="fixed" style="white-space:pre-wrap">--old-mode</code>) or by setting the [old_mode](/kb/en/server-system-variables/#old_mode) system variable.

The different values of `OLD_MODE` are:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>NO_DUP_KEY_WARNINGS_WITH_IGNORE</code></td><td>Don't print duplicate key warnings when using INSERT <a href="/kb/en/ignore/">IGNORE</a></td></tr>
<tr><td><code>NO_PROGRESS_INFO</code></td><td>Don't show progress information in <code><a href="/kb/en/show-processlist/">SHOW PROCESSLIST</a></code></td></tr>
<tr><td><code>ZERO_DATE_TIME_CAST</code></td><td>When a <code>TIME</code> value is casted to a <code>DATETIME</code>, the date part will be <code>0000-00-00</code>, not <code>CURRENT_DATE</code> (as dictated by the SQL standard)</td></tr>
</tbody></table>

## OLD_MODE and Stored Programs

In contrast to [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode), [stored programs](/kb/en/stored-programs-and-views/) use the current user's <code class="fixed" style="white-space:pre-wrap">OLD_MODE</code>value.

Changes to <code class="fixed" style="white-space:pre-wrap">OLD_MODE</code> are not replicated to slaves.

## Examples

This example shows how to get a readable list of enabled OLD_MODE flags:

```sql
SELECT REPLACE(@@OLD_MODE, ',', '\n');
+-------------------------------------------------------------------------+
| REPLACE(@@OLD_MODE, ',', '\n')                                          |
+-------------------------------------------------------------------------+
| NO_DUP_KEY_WARNINGS_WITH_IGNORE,
NO_PROGRESS_INFO |
+-------------------------------------------------------------------------+
```

Adding a new flag:

```sql
SET @@OLD_MODE = CONCAT(@@OLD_MODE, ',NO_PROGRESS_INFO');
```

If the specified flag is already ON, the above example has no effect but does not produce an error.

How to unset a flag:

```sql
SET @@OLD_MODE = REPLACE(@@OLD_MODE, 'NO_PROGRESS_INFO', '');
```

How to check if a flag is set:

```sql
SELECT @@OLD_MODE LIKE '%NO_PROGRESS_INFO';
+------------------------------------+
| @@OLD_MODE LIKE '%NO_PROGESS_INFO' |
+------------------------------------+
|                                  1 |
+------------------------------------+
```