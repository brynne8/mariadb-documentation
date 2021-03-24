# LOAD XML

## Syntax

```sql
LOAD XML [LOW_PRIORITY | CONCURRENT] [LOCAL] INFILE 'file_name'
    [REPLACE | IGNORE]
    INTO TABLE [db_name.]tbl_name
    [CHARACTER SET charset_name]
    [ROWS IDENTIFIED BY '<tagname>']
    [IGNORE number {LINES | ROWS}]
    [(column_or_user_var,...)]
    [SET col_name = expr,...]
```

## Description

The LOAD XML statement reads data from an XML file into a table. The
`file_name` must be given as a literal string. The `tagname` in the
optional ROWS IDENTIFIED BY clause must also be given as a literal
string, and must be surrounded by angle brackets (&lt; and &gt;).

LOAD XML acts as the complement of running the [mysql client](/clients-utilities/mysql-client/mysql-command-line-client/) in XML
output mode (that is, starting the client with the --xml option). To
write data from a table to an XML file, use a command such as the
following one from the system shell:

```sql
shell> mysql --xml -e 'SELECT * FROM mytable' > file.xml
```

To read the file back into a table, use LOAD XML INFILE. By default,
the &lt;row&gt; element is considered to be the equivalent of a database
table row; this can be changed using the ROWS IDENTIFIED BY clause.

This statement supports three different XML formats:

- Column names as attributes and column values as attribute values:

```sql
<row column1="value1" column2="value2" .../>
```

- Column names as tags and column values as the content of these tags:

```sql
<row>
  <column1>value1</column1>
  <column2>value2</column2>
</row>
```

- Column names are the name attributes of &lt;field&gt; tags, and values are
  the contents of these tags:

```sql
<row>
  <field name='column1'>value1</field>
  <field name='column2'>value2</field>
</row>
```

This is the format used by other tools, such as [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/).

All 3 formats can be used in the same XML file; the import routine
automatically detects the format for each row and interprets it
correctly. Tags are matched based on the tag or attribute name and the
column name.

The following clauses work essentially the same way for LOAD XML as
they do for LOAD DATA:

- LOW_PRIORITY or CONCURRENT
- LOCAL
- REPLACE or IGNORE
- CHARACTER SET
- (column_or_user_var,...)
- SET

See [LOAD DATA](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/load-data-into-tables-or-index/load-data-infile/) for more information about these clauses.

The IGNORE number LINES or IGNORE number ROWS clause causes the first
number rows in the XML file to be skipped. It is analogous to the LOAD
DATA statement's IGNORE ... LINES clause.

If the <a undefined>LOW_PRIORITY</a> keyword is used, insertions are delayed until no other clients are reading from the table. The `CONCURRENT` keyword allowes the use of [concurrent inserts](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/concurrent-inserts/). These clauses cannot be specified together.

This statement activates INSERT [triggers](/programming-customizing-mariadb/triggers-events/triggers/).

## See also

- The [CONNECT](/columns-storage-engines-and-plugins/storage-engines/connect/) storage engine has an [XML table type](/kb/en/connect-table-types-data-files/#xml-table-type).