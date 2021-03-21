# aria_read_log

<strong>aria_read_log</strong> is a tool for displaying and applying log records from an [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) transaction log.

Note: Aria is compiled without -DIDENTICAL_PAGES_AFTER_RECOVERY
which means that the table files are not byte-to-byte identical to
files created during normal execution. This should be ok, except for
test scripts that try to compare files before and after recovery.

Usage:

```sql
aria_read_log OPTIONS
```

You need to use one of `-d` or `-a`.

## Options

The following variables can be set while passed as commandline options to aria_read_log, or set in the `[aria_read_log]` section in your [my.cnf](/kb/en/configuring-mariadb-with-mycnf/) file.

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td>-a, --apply</td><td>Apply log to tables: modifies tables! you should make a backup first!  Displays a lot of information if not run with --silent.</td></tr>
<tr><td>--character-sets-dir=name</td><td>Directory where character sets are.</td></tr>
<tr><td>-c, --check</td><td>if --display-only, check if record is fully readable (for debugging).</td></tr>
<tr><td>-?, --help</td><td>Display help and exit.</td></tr>
<tr><td>-d, --display-only</td><td>Display brief info read from records' header.</td></tr>
<tr><td>-e, --end-lsn=#</td><td>Stop applying at this lsn. If end-lsn is used, UNDO:s will not be applied</td></tr>
<tr><td>-h, --aria-log-dir-path=name</td><td>Path to the directory where to store transactional log</td></tr>
<tr><td>-P, --page-buffer-size=#</td><td>The size of the buffer used for index blocks for Aria tables.</td></tr>
<tr><td>-o, --start-from-lsn=#</td><td>Start reading log from this lsn.</td></tr>
<tr><td>-C, --start-from-checkpoint</td><td>Start applying from last checkpoint.</td></tr>
<tr><td>-s, --silent</td><td>Print less information during apply/undo phase.</td></tr>
<tr><td>-T, --tables-to-redo=name</td><td>List of comma-separated tables that we should apply REDO on. Use this if you only want to recover some tables.</td></tr>
<tr><td>-t, --tmpdir=name</td><td>Path for temporary files. Multiple paths can be specified, separated by colon (:)</td></tr>
<tr><td>--translog-buffer-size=#</td><td>The size of the buffer used for transaction log for Aria tables.</td></tr>
<tr><td>-u, --undo</td><td>Apply UNDO records to tables. (disable with --disable-undo) (Defaults to on; use --skip-undo to disable.)</td></tr>
<tr><td>-v, --verbose</td><td>Print more information during apply/undo phase.</td></tr>
<tr><td>-V, --version</td><td>Print version and exit.</td></tr>
</tbody></table>