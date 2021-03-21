# my_print_defaults

`my_print_defaults` displays the options from option groups of option files. It is useful to see which options a particular tool will use.

Output is one option per line, displayed in the form in which they would be specified on the command line.

## Usage

```sql
my_print_defaults [OPTIONS] [groups]
```

## Options

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-c</code>, <code>--config-file=name</code></td><td>Deprecated, please use --defaults-file instead. Name of config file to read; if no extension is given, default extension (e.g., .ini or .cnf) will be added.</td></tr>
<tr><td><code>-# </code>,<code>--debug[=#]</code></td><td>In debug versions, write a debugging log. A typical debug_options string is <code>d:t:o,file_name</code>. The default is <code>d:t:o,/tmp/my_print_defaults.trace</code>.</td></tr>
<tr><td><code>-c, --defaults-file=name</code></td><td>Like --config-file, except: if first option, then read this file only, do not read global or per-user config files; should be the first option.</td></tr>
<tr><td><code>-e</code>, <code>--defaults-extra-file=name</code></td><td>Read this file after the global config file and before the config file in the users home directory; should be the first option.</td></tr>
<tr><td><code>-g</code>, <code>--defaults-group-suffix=name</code></td><td>In addition to the given groups, read also groups with this suffix.</td></tr>
<tr><td><code>-e</code>, <code>--extra-file=name</code></td><td>Deprecated. Synonym for --defaults-extra-file.</td></tr>
<tr><td><code>--mysqld</code></td><td>Read the same set of groups that the mysqld binary does.</td></tr>
<tr><td><code>-n</code>, <code>--no-defaults</code></td><td>Return an empty string (useful for scripts).</td></tr>
<tr><td><code>?</code>, <code>--help</code></td><td>Display this help message and exit.</td></tr>
<tr><td><code>-v</code>, <code>--verbose</code></td><td>Increase the output level.</td></tr>
<tr><td><code>-V</code>, <code>--version</code></td><td>Output version information and exit.</td></tr>
</tbody></table>

## Examples

```sql
my_print_defaults --defaults-file=example.cnf client client-server mysql
```

[mysqlcheck](/sql-statements-structure/sql-statements/table-statements/mysqlcheck/) reads from the [mysqlcheck] and [client] sections, so the following would display the mysqlcheck options.

```sql
my_print_defaults mysqlcheck client
```