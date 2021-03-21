# SHOW CHARACTER SET

## Syntax

```sql
SHOW CHARACTER SET
    [LIKE 'pattern' | WHERE expr]
```

## Description

The <code class="fixed" style="white-space:pre-wrap">SHOW CHARACTER SET</code> statement shows all available [character sets](/kb/en/data-types-character-sets-and-collations/).  The <code class="fixed" style="white-space:pre-wrap">LIKE</code> clause, if present on its own, indicates which character
set names to match. The <code class="highlight fixed" style="white-space:pre-wrap">WHERE</code> and <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clauses can be given to select rows using more general conditions, as discussed in [Extended SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/extended-show/).

The same information can be queried from the [information_schema.CHARACTER_SETS](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-character_sets-table/) table.

See [Setting Character Sets and Collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/setting-character-sets-and-collations/) for details on specifying the character set at the server, database, table and column levels.

## Examples

```sql
SHOW CHARACTER SET LIKE 'latin%';
+---------+-----------------------------+-------------------+--------+
| Charset | Description                 | Default collation | Maxlen |
+---------+-----------------------------+-------------------+--------+
| latin1  | cp1252 West European        | latin1_swedish_ci |      1 |
| latin2  | ISO 8859-2 Central European | latin2_general_ci |      1 |
| latin5  | ISO 8859-9 Turkish          | latin5_turkish_ci |      1 |
| latin7  | ISO 8859-13 Baltic          | latin7_general_ci |      1 |
+---------+-----------------------------+-------------------+--------+
```

```sql
SHOW CHARACTER SET WHERE Maxlen LIKE '2';
+---------+---------------------------+-------------------+--------+
| Charset | Description               | Default collation | Maxlen |
+---------+---------------------------+-------------------+--------+
| big5    | Big5 Traditional Chinese  | big5_chinese_ci   |      2 |
| sjis    | Shift-JIS Japanese        | sjis_japanese_ci  |      2 |
| euckr   | EUC-KR Korean             | euckr_korean_ci   |      2 |
| gb2312  | GB2312 Simplified Chinese | gb2312_chinese_ci |      2 |
| gbk     | GBK Simplified Chinese    | gbk_chinese_ci    |      2 |
| ucs2    | UCS-2 Unicode             | ucs2_general_ci   |      2 |
| cp932   | SJIS for Windows Japanese | cp932_japanese_ci |      2 |
+---------+---------------------------+-------------------+--------+
```