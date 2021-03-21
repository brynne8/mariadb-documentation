# aria_pack

aria_pack is a tool for compressing [Aria](/columns-storage-engines-and-plugins/storage-engines/aria/) tables. The resulting table are read-only, and usually about 40% to 70% smaller.

aria_pack is run as follows

```sql
aria_pack [options] file_name [file_name2...]
```

The file name is the .MAI index file. The extension can be omitted, although keeping it permits wildcards, such as

```sql
aria_pack *.MAI
```

to compress all the files.

aria_pack compresses each column separately, and, when the resulting data is read, only the individual rows and columns required need to be decompressed, allowing for quicker reading.

Once a table has been packed, use [aria_chk -rq](/clients-utilities/aria-clients-and-utilities/aria_chk/) (the quick and recover options) to rebuild its indexes.

## Options

The following variables can be set while passed as commandline options to aria_pack, or set in the [ariapack] section in your [my.cnf](/kb/en/configuring-mariadb-with-mycnf/) file.

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td>-b, --backup</td><td>Make a backup of the table as table_name.OLD.</td></tr>
<tr><td>--character-sets-dir=name</td><td>Directory where character sets are.</td></tr>
<tr><td>-h, --datadir</td><td>Path for control file (and logs if <code>--logdir</code> not used). From <a href="/kb/en/mariadb-1053-release-notes/">MariaDB 10.5.3</a></td></tr>
<tr><td>-#, --debug[=name]</td><td>Output debug log. Often this is 'd:t:o,filename'.</td></tr>
<tr><td>-?, --help</td><td>Display help and exit.</td></tr>
<tr><td>-f, --force</td><td>Force packing of table even if it gets bigger or if tempfile exists.</td></tr>
<tr><td>--ignore-control-file</td><td>Ignore the control file. From <a href="/kb/en/mariadb-1053-release-notes/">MariaDB 10.5.3</a>.</td></tr>
<tr><td>-j, --join=name</td><td>Join all given tables into 'new_table_name'. All tables MUST have identical layouts.</td></tr>
<tr><td>--require-control-file</td><td>Abort if cannot find control file. From <a href="/kb/en/mariadb-1053-release-notes/">MariaDB 10.5.3</a>.</td></tr>
<tr><td>-s, --silent</td><td>Only write output when an error occurs.</td></tr>
<tr><td>-t, --test</td><td>Don't pack table, only test packing it.</td></tr>
<tr><td>-T, --tmpdir=name</td><td>Use temporary directory to store temporary table.</td></tr>
<tr><td>-v, --verbose</td><td>Write info about progress and packing result. Use many -v for more verbosity!</td></tr>
<tr><td>-V, --version</td><td>Output version information and exit.</td></tr>
<tr><td>-w, --wait</td><td>Wait and retry if table is in use.</td></tr>
</tbody></table>

## Unpacking

To unpack a table compressed with aria_pack, use the [aria_chk -u](/clients-utilities/aria-clients-and-utilities/aria_chk/) option.

## Example

```sql
> aria_pack /my/data/test/posts
Compressing /my/data/test/posts.MAD: (1690 records)
- Calculating statistics
- Compressing file
37.71%     
> aria_chk -rq --ignore-control-file /my/data/test/posts
- check record delete-chain
- recovering (with keycache) Aria-table '/my/data/test/posts'
Data records: 1690
State updated
```

## See Also

- [FLUSH TABLES FOR EXPORT](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush-tables-for-export/)
- [myisamchk](/clients-utilities/myisam-clients-and-utilities/myisamchk/)