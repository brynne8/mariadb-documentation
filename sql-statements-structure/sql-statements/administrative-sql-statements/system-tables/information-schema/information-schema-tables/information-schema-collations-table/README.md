# Information Schema COLLATIONS Table

The [Information Schema](/kb/en/information_schema/) `COLLATIONS` table contains a list of supported [collations](/kb/en/data-types-character-sets-and-collations/).

It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>COLLATION_NAME</code></td><td>Name of the collation.</td></tr>
<tr><td><code>CHARACTER_SET_NAME</code></td><td>Associated character set.</td></tr>
<tr><td><code>ID</code></td><td>Collation id.</td></tr>
<tr><td><code>IS_DEFAULT</code></td><td>Whether the collation is the character set's default.</td></tr>
<tr><td><code>IS_COMPILED</code></td><td>Whether the collation is compiled into the server.</td></tr>
<tr><td><code>SORTLEN</code></td><td>Sort length, used for determining the memory used to sort strings in this collation.</td></tr>
</tbody></table>

The [SHOW COLLATION](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-collation) statement returns the same results and both can be reduced in a similar way. For example, the following two statements return the same results:

```sql
SHOW COLLATION WHERE Charset LIKE 'utf8';
```

and

```sql
SELECT * FROM information_schema.COLLATIONS 
WHERE CHARACTER_SET_NAME LIKE 'utf8';
```

## NO PAD collations

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

`NO PAD` collations regard trailing spaces as normal characters. You can get a list of all `NO PAD` collations as follows:

```sql
SELECT collation_name FROM information_schema.COLLATIONS
WHERE collation_name LIKE "%nopad%";  
+------------------------------+
| collation_name               |
+------------------------------+
| big5_chinese_nopad_ci        |
| big5_nopad_bin               |
...
```

## Example

```sql
SELECT * FROM information_schema.COLLATIONS;
+------------------------------+--------------------+------+------------+-------------+---------+
| COLLATION_NAME               | CHARACTER_SET_NAME | ID   | IS_DEFAULT | IS_COMPILED | SORTLEN |
+------------------------------+--------------------+------+------------+-------------+---------+
| big5_chinese_ci              | big5               |    1 | Yes        | Yes         |       1 |
| big5_bin                     | big5               |   84 |            | Yes         |       1 |
| big5_chinese_nopad_ci        | big5               | 1025 |            | Yes         |       1 |
| big5_nopad_bin               | big5               | 1108 |            | Yes         |       1 |
| dec8_swedish_ci              | dec8               |    3 | Yes        | Yes         |       1 |
| dec8_bin                     | dec8               |   69 |            | Yes         |       1 |
| dec8_swedish_nopad_ci        | dec8               | 1027 |            | Yes         |       1 |
| dec8_nopad_bin               | dec8               | 1093 |            | Yes         |       1 |
| cp850_general_ci             | cp850              |    4 | Yes        | Yes         |       1 |
| cp850_bin                    | cp850              |   80 |            | Yes         |       1 |
...
```

## See Also

- [Setting Character Sets and Collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/setting-character-sets-and-collations) - specifying the character set at the server, database, table and column levels
- [Supported Character Sets and Collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/supported-character-sets-and-collations) - full list of supported characters sets and collations.