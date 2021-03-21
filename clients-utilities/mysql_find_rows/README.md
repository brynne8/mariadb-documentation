# mysql_find_rows

##### MariaDB starting with [10.4.6](/kb/en/mariadb-1046-release-notes/)

From [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), `mariadb-find-rows` is a symlink to `mysql_find_rows`.

##### MariaDB starting with [10.5.2](/kb/en/mariadb-1052-release-notes/)

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), `mariadb-find-rows` is the name of the binary, with `mysql_find_rows` a symlink .

`mysql_find_rows` reads files containing SQL statements and extracts statements that match a given regular expression or that contain [USE db_name](/sql-statements-structure/sql-statements/administrative-sql-statements/use) or [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set) statements. The utility was written for use with update log files (as used prior to MySQL 5.0) and as such expects statements to be terminated with semicolon (;) characters. It may be useful with other files that contain SQL statements as long as statements are terminated with semicolons.

## Usage

```sql
mysql_find_rows [options] [file_name ...]
```

Each file_name argument should be the name of file containing SQL statements. If no file names are given, <em>mysql_find_rows</em> reads the standard input.

## Options

mysql_find_rows supports the following options:

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>--help</code>, <code>--Information</code></td><td>Display help and exit.</td></tr>
<tr><td><code>--regexp=pattern</code></td><td>Display queries that match the pattern.</td></tr>
<tr><td><code>--rows=N</code></td><td>Quit after displaying N queries.</td></tr>
<tr><td><code>--skip-use-db</code></td><td>Do not include <a href="/kb/en/use/">USE db_name</a> statements in the output.</td></tr>
<tr><td><code>--start_row=N</code></td><td>Start output from this row (first row is 1).</td></tr>
</tbody></table>

## Examples

```sql
mysql_find_rows --regexp=problem_table --rows=20 < update.log
mysql_find_rows --regexp=problem_table  update-log.1 update-log.2
```