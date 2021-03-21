# LOAD_FILE

## Syntax

```sql
LOAD_FILE(file_name)
```

## Description

Reads the file and returns the file contents as a string. To use this function, the file must be located on the server host, you must specify the full path name to the file, and you must have the FILE privilege.  The file must be readable by all and it must be less than the size, in bytes, of the [max_allowed_packet](/kb/en/server-system-variables/#max_allowed_packet) system variable. If the [secure_file_priv](/kb/en/server-system-variables/#secure_file_priv) system variable is set to a non-empty directory name, the file to be loaded must be located in that directory.

If the file does not exist or cannot be read because one of the preceding conditions is not satisfied, the function returns NULL.

Since [MariaDB 5.1](/kb/en/what-is-mariadb-51/), the [character_set_filesystem](/kb/en/server-system-variables/#character_set_filesystem) system variable has controlled interpretation of file names that are given as literal strings.

Statements using the LOAD_FILE() function are not [safe for statement based replication](/kb/en/unsafe-statements-for-replication/). This is because the slave will execute the LOAD_FILE() command itself.  If the file doesn't exist on the slave, the function will return NULL.

## Examples

```sql
UPDATE t SET blob_col=LOAD_FILE('/tmp/picture') WHERE id=1;
```

## See Also

- [SELECT INTO DUMPFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-dumpfile/)