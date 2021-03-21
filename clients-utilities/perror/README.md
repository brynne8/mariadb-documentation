# perror

<em>perror</em> is a utility that displays descriptions for system or storage engine error codes.

See [MariaDB Error Codes](/sql-statements-structure/sql-language-structure/mariadb-error-codes) for a full list of MariaDB error codes, and [Operating System Error Codes](/kb/en/operating-system-error-codes/) for a list of Linux and Windows error codes.

## Usage

```sql
perror [OPTIONS] [ERRORCODE [ERRORCODE...]]
```

If you need to describe a negative error code, use `--` before the first error code to end the options.

## Options

<table><tbody><tr><th>Option</th><th>Description</th></tr>
<tr><td><code>-?</code>, <code>--help</code></td><td>Display help and exit.</td></tr>
<tr><td><code>-I</code>, <code>--info</code></td><td>Synonym for <code>--help</code>.</td></tr>
<tr><td><code>-s</code>, <code>--silent</code></td><td>Only print the error message.</td></tr>
<tr><td><code>-v</code>, <code>--verbose</code></td><td>Print error code and message (default). (Defaults to on; use <code>--skip-verbose</code> to disable.)</td></tr>
<tr><td><code>-V</code>, <code>--version</code></td><td>Displays version information and exits.</td></tr>
</tbody></table>

## Examples

System error code:

```sql
shell> perror 96
OS error code  96:  Protocol family not supported
```

MariaDB/MySQL [error code](/sql-statements-structure/sql-language-structure/mariadb-error-codes):

```sql
shell> perror 1005 1006
MySQL error code 1005 (ER_CANT_CREATE_TABLE): Can't create table %`s.%`s (errno: %M)
MySQL error code 1006 (ER_CANT_CREATE_DB): Can't create database '%-.192s' (errno: %M)
```

```sql
shell> perror --silent 1979
You are not owner of query %lu
```