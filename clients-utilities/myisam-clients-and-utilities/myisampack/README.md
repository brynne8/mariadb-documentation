# myisampack

`myisampack` is a tool for compressing [MyISAM](/kb/en/myisam/) tables. The resulting tables
are read-only, and usually about 40% to 70% smaller. It is run as follows:

```sql
myisampack [options] file_name [file_name2...]
```

The `file_name` is the `.MYI` index file. The extension can be omitted,
although keeping it permits wildcards, such as:

```sql
myisampack *.MYI
```

...to compress all the files.

`myisampack` compresses each column separately, and, when the resulting data
is read, only the individual rows and columns required need to be decompressed,
allowing for quicker reading.

Once a table has been packed, use [myisamchk -rq](/clients-utilities/myisam-clients-and-utilities/myisamchk/) (the quick
and recover options) to rebuild its indexes.

`myisampack` does not support partitioned tables.

Do not run myisampack if the tables could be updated during the operation, and
[skip_external_locking](/kb/en/server-system-variables/#skip_external_locking) has
been set.

## Options

The following variables can be set while passed as commandline options to
`myisampack`, or set with a `[myisampack]` section in your
[my.cnf](/kb/en/configuring-mariadb-with-mycnf/) file.

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-b</code>, <code>--backup</code></td><td>Make a backup of the table as <code>table_name.OLD</code>.</td></tr>
<tr><td><code>--character-sets-dir=name</code></td><td>Directory where character sets are.</td></tr>
<tr><td><code>-# </code>, <code>--debug[=name]</code></td><td>Output debug log. Often this is <code>'d:t:o,filename'</code>.</td></tr>
<tr><td><code>-f</code>, <code>--force</code></td><td>Force packing of table even if it gets bigger or if tempfile exists.</td></tr>
<tr><td><code>-j</code>, <code>--join=name</code></td><td>Join all given tables into <code>'new_table_name'</code>. All tables <strong>must</strong> have identical layouts.</td></tr>
<tr><td><code>-?</code>, <code>--help</code></td><td>Display help and exit.</td></tr>
<tr><td><code>-s</code>, <code>--silent</code></td><td>Only write output when an error occurs</td></tr>
<tr><td><code>-T</code>, <code>--tmpdir=name</code></td><td>Use temporary directory to store temporary table.</td></tr>
<tr><td><code>-t</code>, <code>--test</code></td><td>Don't pack table, only test packing it.</td></tr>
<tr><td><code>-v</code>, <code>--verbose</code></td><td>Write info about progress and packing result. Use multiple <code>-v</code> flags for more verbosity.</td></tr>
<tr><td><code>-V</code>, <code>--version</code></td><td>Output version information and exit.</td></tr>
<tr><td><code>-w</code>, <code>--wait</code></td><td>Wait and retry if table is in use.</td></tr>
</tbody></table>

## Uncompressing

To uncompress a table compressed with `myisampack`, use the
[myisamchk -u](/clients-utilities/myisam-clients-and-utilities/myisamchk/) option.

## Examples

```sql
> myisampack /var/lib/mysql/test/posts
Compressing /var/lib/mysql/test/posts.MYD: (1680 records)
- Calculating statistics
- Compressing file
37.71%
> myisamchk -rq /var/lib/mysql/test/posts
- check record delete-chain
- recovering (with sort) MyISAM-table '/var/lib/mysql/test/posts'
Data records: 1680
- Fixing index 1
- Fixing index 2
```

## See Also

- [FLUSH TABLES FOR EXPORT](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush-tables-for-export/)
- [myisamchk](/clients-utilities/myisam-clients-and-utilities/myisamchk/)