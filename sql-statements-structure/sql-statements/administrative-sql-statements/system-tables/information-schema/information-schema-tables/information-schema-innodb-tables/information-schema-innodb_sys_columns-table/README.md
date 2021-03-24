# Information Schema INNODB_SYS_COLUMNS Table

##### MariaDB starting with [5.5](/kb/en/what-is-mariadb-55/)

The [Information Schema](/kb/en/information_schema/) `INNODB_SYS_COLUMNS` table contains information about InnoDB fields.

The `PROCESS` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>TABLE_ID</code></td><td>Table identifier, matching the value from <a href="/kb/en/information-schema-innodb_sys_tables-table/">INNODB_SYS_TABLES.TABLE_ID</a>.</td></tr>
<tr><td><code>NAME</code></td><td>Column name.</td></tr>
<tr><td><code>POS</code></td><td>Ordinal position of the column in the table, starting from <code>0</code>. This value is adjusted when columns are added or removed.</td></tr>
<tr><td><code>MTYPE</code></td><td>Numeric column type identifier, (see the table below for an explanation of its values).</td></tr>
<tr><td><code>PRTYPE</code></td><td>Binary value of the InnoDB precise type, representing the data type, character set code and nullability.</td></tr>
<tr><td><code>LEN</code></td><td>Column length. For multi-byte character sets, represents the length in bytes.</td></tr>
</tbody></table>

The column `MTYPE` uses a numeric column type identifier, which has the following values:

<table><tbody><tr><th>Column Type Identifier</th><th>Description</th></tr>
<tr><td><code>1</code></td><td><code><a href="/kb/en/varchar/">VARCHAR</a></code></td></tr>
<tr><td><code>2</code></td><td><code><a href="/kb/en/char/">CHAR</a></code></td></tr>
<tr><td><code>3</code></td><td><code>FIXBINARY</code></td></tr>
<tr><td><code>4</code></td><td><code><a href="/kb/en/binary/">BINARY</a></code></td></tr>
<tr><td><code>5</code></td><td><code><a href="/kb/en/blob/">BLOB</a></code></td></tr>
<tr><td><code>6</code></td><td><code><a href="/kb/en/int/">INT</a></code></td></tr>
<tr><td><code>7</code></td><td><code>SYS_CHILD</code></td></tr>
<tr><td><code>8</code></td><td><code>SYS</code></td></tr>
<tr><td><code>9</code></td><td><code><a href="/kb/en/float/">FLOAT</a></code></td></tr>
<tr><td><code>10</code></td><td><code><a href="/kb/en/double/">DOUBLE</a></code></td></tr>
<tr><td><code>11</code></td><td><code><a href="/kb/en/decimal/">DECIMAL</a></code></td></tr>
<tr><td><code>12</code></td><td><code>VARMYSQL</code></td></tr>
<tr><td><code>13</code></td><td><code>MYSQL</code></td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM information_schema.INNODB_SYS_COLUMNS LIMIT 3\G
*************************** 1. row ***************************
TABLE_ID: 11
    NAME: ID
     POS: 0
   MTYPE: 1
  PRTYPE: 524292
     LEN: 0
*************************** 2. row ***************************
TABLE_ID: 11
    NAME: FOR_NAME 
     POS: 0
   MTYPE: 1
  PRTYPE: 524292
    LEN: 0
*************************** 3. row ***************************
TABLE_ID: 11
    NAME: REF_NAME 
     POS: 0
   MTYPE: 1
  PRTYPE: 524292
     LEN: 0
3 rows in set (0.00 sec)
```