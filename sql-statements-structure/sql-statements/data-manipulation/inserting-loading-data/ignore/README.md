# IGNORE

The `IGNORE` option tells the server to ignore some common errors.

`IGNORE` can be used with the following statements:

- [DELETE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/delete)
- [INSERT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert) (see also [INSERT IGNORE](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-ignore))
- [LOAD DATA INFILE](/kb/en/load-data-infile/)
- [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update)
- [ALTER TABLE](/sql-statements-structure/sql-statements/data-definition/alter/alter-table)
- [CREATE TABLE ... SELECT](/kb/en/create-table/#create-select)
- [INSERT ... SELECT](/sql-statements-structure/sql-statements/data-manipulation/inserting-loading-data/insert-select)

The logic used:

- Variables out of ranges are replaced with the maximum/minimum value.

- [SQL_MODEs](/kb/en/sql_mode/) `STRICT_TRANS_TABLES`, `STRICT_ALL_TABLES`, `NO_ZERO_IN_DATE`, `NO_ZERO_DATE` are ignored.

- Inserting `NULL` in a `NOT NULL` field will insert 0 ( in a numerical
  field), 0000-00-00 ( in a date field) or an empty string ( in a character
  field).

- Rows that cause a duplicate key error or break a foreign key constraint are
  not inserted, updated, or deleted.

The following errors are ignored:

<table><tbody><tr><th>Error number</th><th>Symbolic error name</th><th>Description</th></tr>
<tr><td><code>1022</code></td><td><code>ER_DUP_KEY</code></td><td>Can't write; duplicate key in table '%s'</td></tr>
<tr><td><code>1048</code></td><td><code>ER_BAD_NULL_ERROR</code></td><td>Column '%s' cannot be null</td></tr>
<tr><td><code>1062</code></td><td><code>ER_DUP_ENTRY</code></td><td>Duplicate entry '%s' for key %d</td></tr>
<tr><td><code>1242</code></td><td><code>ER_SUBQUERY_NO_1_ROW</code></td><td>Subquery returns more than 1 row</td></tr>
<tr><td><code>1264</code></td><td><code>ER_WARN_DATA_OUT_OF_RANGE</code></td><td>Out of range value for column '%s' at row %ld</td></tr>
<tr><td><code>1265</code></td><td><code>WARN_DATA_TRUNCATED</code></td><td>Data truncated for column '%s' at row %ld</td></tr>
<tr><td><code>1292</code></td><td><code>ER_TRUNCATED_WRONG_VALUE</code></td><td>Truncated incorrect %s value: '%s'</td></tr>
<tr><td><code>1366</code></td><td><code>ER_TRUNCATED_WRONG_VALUE_FOR_FIELD</code></td><td>Incorrect integer value</td></tr>
<tr><td><code>1369</code></td><td><code>ER_VIEW_CHECK_FAILED</code></td><td><code>CHECK OPTION failed '%s.%s'</code></td></tr>
<tr><td><code>1451</code></td><td><code>ER_ROW_IS_REFERENCED_2</code></td><td>Cannot delete or update a parent row</td></tr>
<tr><td><code>1452</code></td><td><code>ER_NO_REFERENCED_ROW_2</code></td><td>Cannot add or update a child row: a foreign key constraint fails (%s)</td></tr>
<tr><td><code>1526</code></td><td><code>ER_NO_PARTITION_FOR_GIVEN_VALUE</code></td><td>Table has no partition for value %s</td></tr>
<tr><td><code>1586</code></td><td><code>ER_DUP_ENTRY_WITH_KEY_NAME</code></td><td>Duplicate entry '%s' for key '%s'</td></tr>
<tr><td><code>1591</code></td><td><code>ER_NO_PARTITION_FOR_GIVEN_VALUE_SILENT</code></td><td>Table has no partition for some existing values</td></tr>
<tr><td><code>1748</code></td><td><code>ER_ROW_DOES_NOT_MATCH_GIVEN_PARTITION_SET</code></td><td>Found a row not matching the given partition set</td></tr>
</tbody></table>

Ignored errors normally generate a warning.

A property of the `IGNORE` clause consists in causing transactional engines and non-transactional engines (like XtraDB and Aria) to behave the same way. For example, normally a multi-row insert which tries to violate a `UNIQUE` contraint is completely rolled back on XtraDB/InnoDB, but might be partially executed on Aria. With the `IGNORE` clause, the statement will be partially executed in both engines.

Duplicate key errors also generate warnings. The [OLD_MODE](/kb/en/old_mode/) server variable can be used to prevent this.