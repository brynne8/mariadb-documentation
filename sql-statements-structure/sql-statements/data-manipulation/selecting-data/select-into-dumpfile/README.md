# SELECT INTO DUMPFILE

## Syntax

```sql
SELECT ... INTO DUMPFILE 'file_path'
```

## Description

`SELECT ... INTO DUMPFILE` is a [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) clause which writes the resultset into a single unformatted row, without any separators, in a file. The results will not be returned to the client.

<em>file_path</em> can be an absolute path, or a relative path starting from the data directory. It can only be specified as a [string literal](/sql-statements-structure/sql-language-structure/string-literals/), not as a variable. However, the statement can be dynamically composed and executed as a prepared statement to work around this limitation.

This statement is binary-safe and so is particularly useful for writing [BLOB](/columns-storage-engines-and-plugins/data-types/string-data-types/blob/) values to file. It can be used, for example, to copy an image or an audio document from the database to a file. SELECT ... INTO FILE can be used to save a text file.

The file must not exist. It cannot be overwritten. A user needs the [FILE](/kb/en/grant/#global-privileges) privilege to run this statement. Also, MariaDB needs permission to write files in the specified location. If the [secure_file_priv](/kb/en/server-system-variables/#secure_file_priv) system variable is set to a non-empty directory name, the file can only be written to that directory.

Since [MariaDB 5.1](/kb/en/what-is-mariadb-51/), the [character_set_filesystem](/kb/en/server-system-variables/#character_set_filesystem) system variable has controlled interpretation of file names that are given as literal strings.

## Example

```sql
SELECT _utf8'Hello world!' INTO DUMPFILE '/tmp/world';

SELECT LOAD_FILE('/tmp/world') AS world;
+--------------+
| world        |
+--------------+
| Hello world! |
+--------------+
```

## See Also

- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)
- [LOAD_FILE()](/built-in-functions/string-functions/load_file/)
- [SELECT INTO Variable](/kb/en/select-into-variable/)
- [SELECT INTO OUTFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-outfile/)