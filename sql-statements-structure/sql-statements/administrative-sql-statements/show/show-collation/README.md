# SHOW COLLATION

## Syntax

```sql
SHOW COLLATION
    [LIKE 'pattern' | WHERE expr]
```

## Description

The output from <code class="highlight fixed" style="white-space:pre-wrap">SHOW COLLATION</code> includes all available
[collations](/kb/en/data-types-character-sets-and-collations/). The <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clause, if present on its own, indicates which collation names to match. The <code class="highlight fixed" style="white-space:pre-wrap">WHERE</code> and <code class="highlight fixed" style="white-space:pre-wrap">LIKE</code> clauses can be given to select rows using more general conditions, as discussed in [Extended SHOW](/sql-statements-structure/sql-statements/administrative-sql-statements/show/extended-show/).

The same information can be queried from the [information_schema.COLLATIONS](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-collations-table/) table.

See [Setting Character Sets and Collations](/columns-storage-engines-and-plugins/data-types/string-data-types/character-sets/setting-character-sets-and-collations/) for details on specifying the collation at the server, database, table and column levels.

## Examples

```sql
SHOW COLLATION LIKE 'latin1%';
+-------------------+---------+----+---------+----------+---------+
| Collation         | Charset | Id | Default | Compiled | Sortlen |
+-------------------+---------+----+---------+----------+---------+
| latin1_german1_ci | latin1  |  5 |         | Yes      |       1 |
| latin1_swedish_ci | latin1  |  8 | Yes     | Yes      |       1 |
| latin1_danish_ci  | latin1  | 15 |         | Yes      |       1 |
| latin1_german2_ci | latin1  | 31 |         | Yes      |       2 |
| latin1_bin        | latin1  | 47 |         | Yes      |       1 |
| latin1_general_ci | latin1  | 48 |         | Yes      |       1 |
| latin1_general_cs | latin1  | 49 |         | Yes      |       1 |
| latin1_spanish_ci | latin1  | 94 |         | Yes      |       1 |
+-------------------+---------+----+---------+----------+---------+
```

```sql
SHOW COLLATION WHERE Sortlen LIKE '8' AND Charset LIKE 'utf8';
+--------------------+---------+-----+---------+----------+---------+
| Collation          | Charset | Id  | Default | Compiled | Sortlen |
+--------------------+---------+-----+---------+----------+---------+
| utf8_unicode_ci    | utf8    | 192 |         | Yes      |       8 |
| utf8_icelandic_ci  | utf8    | 193 |         | Yes      |       8 |
| utf8_latvian_ci    | utf8    | 194 |         | Yes      |       8 |
| utf8_romanian_ci   | utf8    | 195 |         | Yes      |       8 |
| utf8_slovenian_ci  | utf8    | 196 |         | Yes      |       8 |
| utf8_polish_ci     | utf8    | 197 |         | Yes      |       8 |
| utf8_estonian_ci   | utf8    | 198 |         | Yes      |       8 |
| utf8_spanish_ci    | utf8    | 199 |         | Yes      |       8 |
| utf8_swedish_ci    | utf8    | 200 |         | Yes      |       8 |
| utf8_turkish_ci    | utf8    | 201 |         | Yes      |       8 |
| utf8_czech_ci      | utf8    | 202 |         | Yes      |       8 |
| utf8_danish_ci     | utf8    | 203 |         | Yes      |       8 |
| utf8_lithuanian_ci | utf8    | 204 |         | Yes      |       8 |
| utf8_slovak_ci     | utf8    | 205 |         | Yes      |       8 |
| utf8_spanish2_ci   | utf8    | 206 |         | Yes      |       8 |
| utf8_roman_ci      | utf8    | 207 |         | Yes      |       8 |
| utf8_persian_ci    | utf8    | 208 |         | Yes      |       8 |
| utf8_esperanto_ci  | utf8    | 209 |         | Yes      |       8 |
| utf8_hungarian_ci  | utf8    | 210 |         | Yes      |       8 |
| utf8_sinhala_ci    | utf8    | 211 |         | Yes      |       8 |
| utf8_croatian_ci   | utf8    | 213 |         | Yes      |       8 |
+--------------------+---------+-----+---------+----------+---------+
```