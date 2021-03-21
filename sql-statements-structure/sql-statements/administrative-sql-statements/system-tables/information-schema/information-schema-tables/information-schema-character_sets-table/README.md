# Information Schema CHARACTER_SETS Table

The [Information Schema](/kb/en/information_schema/) `CHARACTER_SETS` table contains a list of supported [character sets](/kb/en/data-types-character-sets-and-collations/), their default collations and maximum lengths.

It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>CHARACTER_SET_NAME</code></td><td>Name of the character set.</td></tr>
<tr><td><code>DEFAULT_COLLATE_NAME</code></td><td>Default collation used.</td></tr>
<tr><td><code>DESCRIPTION</code></td><td>Character set description.</td></tr>
<tr><td><code>MAXLEN</code></td><td>Maximum length.</td></tr>
</tbody></table>

The [SHOW CHARACTER SET](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-character-set/) statement returns the same results (although in a different order), and both can be refined in the same way. For example, the following two statements return the same results:

```sql
SHOW CHARACTER SET WHERE Maxlen LIKE '2';
```

and

```sql
SELECT * FROM information_schema.CHARACTER_SETS 
WHERE MAXLEN LIKE '2';
```

See [Setting Character Sets and Collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/setting-character-sets-and-collations/) for details on specifying the character set at the server, database, table and column levels, and [Supported Character Sets and Collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/supported-character-sets-and-collations/) for a full list of supported characters sets and collations.

## Example

```sql
SELECT CHARACTER_SET_NAME FROM information_schema.CHARACTER_SETS 
WHERE DEFAULT_COLLATE_NAME LIKE '%chinese%';
+--------------------+
| CHARACTER_SET_NAME |
+--------------------+
| big5               |
| gb2312             |
| gbk                |
+--------------------+
```