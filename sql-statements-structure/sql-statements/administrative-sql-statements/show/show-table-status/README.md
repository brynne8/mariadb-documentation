# SHOW TABLE STATUS

## Syntax

```sql
SHOW TABLE STATUS [{FROM | IN} db_name]
    [LIKE 'pattern' | WHERE expr]
```

## Description

<code class="highlight fixed" style="white-space:pre-wrap">SHOW TABLE STATUS</code> works like 
 <code class="highlight fixed" style="white-space:pre-wrap">[SHOW TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-tables)</code>, but provides more extensive information about each non-<code class="highlight fixed" style="white-space:pre-wrap">TEMPORARY</code> table.

The <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clause, if present on its own, indicates which table names to
match. The <code class="highlight fixed" style="white-space:pre-wrap">WHERE</code> and <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clauses can be given to select rows using more general conditions, as discussed in [Extended SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/extended-show).

The following information is returned:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td>Name</td><td>Table name.</td></tr>
<tr><td>Engine</td><td>Table <a href="/kb/en/storage-engines/">storage engine</a>.</td></tr>
<tr><td>Version</td><td>Version number from the table's .frm file.</td></tr>
<tr><td>Row_format</td><td>Row format (see <a href="/kb/en/xtradbinnodb-storage-formats/">InnoDB</a>, <a href="/kb/en/aria-storage-formats/">Aria</a> and <a href="/kb/en/myisam-storage-formats/">MyISAM</a> row formats).</td></tr>
<tr><td>Rows</td><td>Number of rows in the table. Some engines, such as <a href="/kb/en/innodb/">XtraDB and InnoDB</a> may store an estimate.</td></tr>
<tr><td>Avg_row_length</td><td>Average row length in the table.</td></tr>
<tr><td>Data_length</td><td>For <a href="/kb/en/innodb/">InnoDB/XtraDB</a>, the index size, in pages, multiplied by the page size. For <a href="/kb/en/aria/">Aria</a> and <a href="/kb/en/myisam/">MyISAM</a>, length of the data file, in bytes. For <a href="/kb/en/memory-storage-engine/">MEMORY</a>, the approximate allocated memory.</td></tr>
<tr><td>Max_data_length</td><td>Maximum length of the data file, ie the total number of bytes that could be stored in the table. Not used in <a href="/kb/en/innodb/">XtraDB and InnoDB</a>.</td></tr>
<tr><td>Index_length</td><td>Length of the index file.</td></tr>
<tr><td>Data_free</td><td>Bytes allocated but unused. For InnoDB tables in a shared tablespace, the free space of the shared tablespace with small safety margin. An estimate in the case of partitioned tables - see the <code><a href="/kb/en/information-schema-partitions-table/">PARTITIONS</a></code> table.</td></tr>
<tr><td>Auto_increment</td><td>Next <code><a href="/kb/en/auto_increment/">AUTO_INCREMENT</a></code> value.</td></tr>
<tr><td>Create_time</td><td>Time the table was created.</td></tr>
<tr><td>Update_time</td><td>Time the table was last updated. On Windows, the timestamp is not updated on update, so MyISAM values will be inaccurate. In <a href="/kb/en/innodb/">InnoDB</a>, if shared tablespaces are used, will be <code>NULL</code>, while buffering can also delay the update, so the value will differ from the actual time of the last <code>UPDATE</code>, <code>INSERT</code> or <code>DELETE</code>.</td></tr>
<tr><td>Check_time</td><td>Time the table was last checked. Not kept by all storage engines, in which case will be <code>NULL</code>.</td></tr>
<tr><td>Collation</td><td><a href="/kb/en/data-types-character-sets-and-collations/">Character set and collation</a>.</td></tr>
<tr><td>Checksum</td><td>Live checksum value, if any.</td></tr>
<tr><td>Create_options</td><td>Extra <code><a href="/kb/en/create-table/">CREATE TABLE</a></code> options.</td></tr>
<tr><td>Comment</td><td>Table comment provided when MariaDB created the table.</td></tr>
<tr><td>Max_index_length</td><td>Maximum index length (supported by MyISAM and Aria tables). Added in <a href="/kb/en/mariadb-1035-release-notes/">MariaDB 10.3.5</a>.</td></tr>
<tr><td>Temporary</td><td>Placeholder to signal that a table is a temporary table. Currently always "N", except "Y" for generated information_schema tables and NULL for <a href="/kb/en/views/">views</a>. Added in <a href="/kb/en/mariadb-1035-release-notes/">MariaDB 10.3.5</a>.</td></tr>
</tbody></table>

Similar information can be found in the <a undefined>information_schema.TABLES</a> table as well as by using [mysqlshow](/clients-utilities/mysqlshow):

```sql
mysqlshow --status db_name
```

## Example

```sql
show table status\G
*************************** 1. row ***************************
           Name: bus_routes
         Engine: InnoDB
        Version: 10
     Row_format: Dynamic
           Rows: 5
 Avg_row_length: 3276
    Data_length: 16384
Max_data_length: 0
   Index_length: 0
      Data_free: 0
 Auto_increment: NULL
    Create_time: 2017-05-24 11:17:46
    Update_time: NULL
     Check_time: NULL
      Collation: latin1_swedish_ci
       Checksum: NULL
 Create_options: 
        Comment:
```