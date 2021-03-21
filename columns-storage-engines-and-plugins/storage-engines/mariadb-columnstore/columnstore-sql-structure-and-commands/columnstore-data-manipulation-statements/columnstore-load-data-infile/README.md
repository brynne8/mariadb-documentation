# ColumnStore LOAD DATA INFILE

The LOAD DATA INFILE statement reads rows from a text file into a table at a very high speed. The file name must be given as a literal string.

```sql
LOAD DATA [LOCAL] INFILE 'file_name' 
  INTO TABLE tbl_name
  [CHARACTER SET charset_name]
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

- ColumnStore ignores the ON DUPLICATE KEY clause.
- Non-transactional LOAD DATA INFILE is directed to ColumnStores cpimport tool by default, which significantly increases performance.
- Transactional LOAD DATA INFILE statements (that is with AUTOCOMMIT off or after a START TRANSACTION) are processed through normal DML processes.
- When using LOAD DATA LOCAL INFILE with the <em>mcsmysql</em> utility , use the [--local-infile](/kb/en/server-system-variables/#local_infile) command-line option.
- Use cpimport for importing UTF-8 data that contains multi-byte values

The following example loads data into the a simple 5 column table: A file named /simpletable.tbl<em> has the following data in it.</em>

```sql
1|100|1000|10000|Test Number 1|
2|200|2000|20000|Test Number 2|
3|300|3000|30000|Test Number 3|
```

The data can then be loaded into the simpletable table with the following syntax:

```sql
LOAD DATA INFILE 'simpletable.tbl' INTO TABLE simpletable FIELDS TERMINATED BY '|'
```

If the default mode is set to use cpimport internally any output error files will be written to /usr/local/mariadb/columnstore/mysql/db directory (or equivalent directory for non root install). These can be consulted for troubleshooting any errors reported.

### See Also

[LOAD DATA INFILE](/kb/en/load-data-infile/)