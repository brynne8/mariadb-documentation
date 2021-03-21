# SELECT INTO OUTFILE

## Syntax

```sql
SELECT ... INTO OUTFILE 'file_name'
        [CHARACTER SET charset_name]
        [export_options]

export_options:
    [{FIELDS | COLUMNS}
        [TERMINATED BY 'string']
        [[OPTIONALLY] ENCLOSED BY 'char']
        [ESCAPED BY 'char']
    ]
    [LINES
        [STARTING BY 'string']
        [TERMINATED BY 'string']
    ]
```

## Description

`SELECT INTO OUTFILE` writes the resulting rows to a file, and allows the use of column and row terminators to specify a particular output format. The default is to terminate fields with tabs (`\t`) and lines with newlines (`\n`).

The file must not exist. It cannot be overwritten. A user needs the [FILE](/kb/en/grant/#global-privileges) privilege to run this statement. Also, MariaDB needs permission to write files in the specified location. If the [secure_file_priv](/kb/en/server-system-variables/#secure_file_priv) system variable is set to a non-empty directory name, the file can only be written to that directory.

The <a undefined>LOAD DATA INFILE</a> statement complements `SELECT INTO OUTFILE`.

### Character-sets

The `CHARACTER SET` clause specifies the [character set](/kb/en/data-types-character-sets-and-collations/) in which the results are to be written. Without the clause, no conversion takes place (the binary character set). In this case, if there are multiple character sets, the output will contain these too, and may not easily be able to be reloaded.

In cases where you have two servers using different character-sets, using `SELECT INTO OUTFILE` to transfer data from one to the other can have unexpected results.  To ensure that MariaDB correctly interprets the escape sequences, use the `CHARACTER SET` clause on both the `SELECT INTO OUTFILE` statement and the subsequent <a undefined>LOAD DATA INFILE</a> statement.

## Example

The following example produces a file in the CSV format:

```sql
SELECT customer_id, firstname, surname INTO OUTFILE '/exportdata/customers.txt'
  FIELDS TERMINATED BY ',' OPTIONALLY ENCLOSED BY '"'
  LINES TERMINATED BY '\n'
  FROM customers;
```

## See Also

- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/)
- [LOAD_DATA()](/built-in-functions/string-functions/load_file/) function
- [LOAD DATA INFILE](/kb/en/load-data-infile/)
- [SELECT INTO Variable](/kb/en/select-into-variable/)
- [SELECT INTO DUMPFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-dumpfile/)