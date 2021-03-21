# Mroonga Overview

<br><br>
Once Mroonga has been installed (see [About Mroonga](/columns-storage-engines-and-plugins/storage-engines/mroonga/about-mroonga/)), its basic usage is similar to that of a [regular fulltext index](/replication/optimization-and-tuning/optimization-and-indexes/full-text-indexes/).
<br><br><br><br>
For example:<br><br><br>

```sql
CREATE TABLE ft_mroonga(copy TEXT,FULLTEXT(copy)) ENGINE=Mroonga;

INSERT INTO ft_mroonga(copy) VALUES ('Once upon a time'),
    ('There was a wicked witch'), ('Who ate everybody up');

SELECT * FROM ft_mroonga WHERE MATCH(copy) AGAINST('wicked');
+--------------------------+
| copy                     |
+--------------------------+
| There was a wicked witch |
+--------------------------+
```

## Score

Mroonga can also order by weighting. For example, first add another record:

```sql
INSERT INTO ft_mroonga(copy) VALUES ('She met a wicked, wicked witch');
```

Records can be returned by weighting, for example, the newly added record has two occurences of the word 'wicked' and a higher weighting:

```sql
SELECT *, MATCH(copy) AGAINST('wicked') AS score FROM ft_mroonga 
   WHERE MATCH(copy) AGAINST('wicked') ORDER BY score DESC;
+--------------------------------+--------+
| copy                           | score  |
+--------------------------------+--------+
| She met a wicked, wicked witch | 299594 |
| There was a wicked witch       | 149797 |
+--------------------------------+--------+
```

## Parser

Mroonga permits you to set a different parser for searching by specifying the parser in the `CREATE TABLE` statement as a comment or, in older versions, changing the value of the [mroonga_default_parser](/kb/en/mroonga-system-variables/#mroonga_default_parser) system variable.

For example:

```sql
CREATE TABLE ft_mroonga(copy TEXT,FULLTEXT(copy) COMMENT 'parser "TokenDelimitNull"') 
  ENGINE=Mroonga;, 
```

or

```sql
SET GLOBAL mroonga_default_parser = 'TokenBigramSplitSymbol';
```

The following parser settings are available:

<table><tbody><tr><th>Setting</th><th>Description</th></tr>
<tr><td>off</td><td>No tokenizing is performed.</td></tr>
<tr><td>TokenBigram</td><td>Default value. Continuous alphabetical characters,  numbers or symbols are treated as a token.</td></tr>
<tr><td>TokenBigramIgnoreBlank</td><td>Same as <code>TokenBigram</code> except that white spaces are ignored.</td></tr>
<tr><td>TokenBigramIgnoreBlankSplitSymbol</td><td>Same as <code>TokenBigramSplitSymbol</code>. except that white spaces are ignore.</td></tr>
<tr><td>TokenBigramIgnoreBlankSplitSymbolAlpha</td><td>Same as <code>TokenBigramSplitSymbolAlpha</code> except that white spaces are ignored.</td></tr>
<tr><td>TokenBigramIgnoreBlankSplitSymbolAlphaDigit</td><td>Same as <code>TokenBigramSplitSymbolAlphaDigit</code> except that white spaces are ignored.</td></tr>
<tr><td>TokenBigramSplitSymbol</td><td>Same as <code>TokenBigram</code> except that continuous symbols are not treated as a token, but tokenised in bigram.</td></tr>
<tr><td>TokenBigramSplitSymbolAlpha</td><td>Same as <code>TokenBigram</code> except that continuous alphabetical characters are not treated as a token, but tokenised in bigram.</td></tr>
<tr><td>TokenDelimit</td><td>Tokenises by splitting on white spaces.</td></tr>
<tr><td>TokenDelimitNull</td><td>Tokenises by splitting on null characters (\0).</td></tr>
<tr><td>TokenMecab</td><td>Tokenise using MeCab. Required Groonga to be buillt with MeCab support.</td></tr>
<tr><td>TokenTrigram</td><td>Tokenises in trigrams but continuous alphabetical characters, numbers or symbols are treated as a token.</td></tr>
<tr><td>TokenUnigram</td><td>Tokenises in unigrams but continuous alphabetical characters, numbers or symbols are treated as a token.</td></tr>
</tbody></table>

### Examples

#### TokenBigram vs TokenBigramSplitSymbol

`TokenBigram` failing to match partial symbols which `TokenBigramSplitSymbol` matches, since `TokenBigramSplitSymbol` does not treat continuous symbols as a token.

```sql
DROP TABLE ft_mroonga;
CREATE TABLE ft_mroonga(copy TEXT,FULLTEXT(copy) COMMENT 'parser "TokenBigram"') 
  ENGINE=Mroonga;
INSERT INTO ft_mroonga(copy) VALUES ('Once upon a time'),   
  ('There was a wicked witch'), 
  ('Who ate everybody up'), 
  ('She met a wicked, wicked witch'), 
  ('A really wicked, wicked witch!!?!');
SELECT * FROM ft_mroonga WHERE MATCH(copy) AGAINST('!?');
Empty set (0.00 sec)

DROP TABLE ft_mroonga;
CREATE TABLE ft_mroonga(copy TEXT,FULLTEXT(copy) COMMENT 'parser "TokenBigramSplitSymbol"') 
  ENGINE=Mroonga;
INSERT INTO ft_mroonga(copy) VALUES ('Once upon a time'),   
  ('There was a wicked witch'), 
  ('Who ate everybody up'), 
  ('She met a wicked, wicked witch'), 
  ('A really wicked, wicked witch!!?!');
SELECT * FROM ft_mroonga WHERE MATCH(copy) AGAINST('!?');
+-----------------------------------+
| copy                              |
+-----------------------------------+
| A really wicked, wicked witch!!?! |
+-----------------------------------+
```

#### TokenBigram vs TokenBigramSplitSymbolAlpha

```sql
DROP TABLE ft_mroonga;
CREATE TABLE ft_mroonga(copy TEXT,FULLTEXT(copy) COMMENT 'parser "TokenBigram"') 
  ENGINE=Mroonga;
INSERT INTO ft_mroonga(copy) VALUES ('Once upon a time'),   
  ('There was a wicked witch'), 
  ('Who ate everybody up'), 
  ('She met a wicked, wicked witch'), 
  ('A really wicked, wicked witch!!?!');
SELECT * FROM ft_mroonga WHERE MATCH(copy) AGAINST('ick');
Empty set (0.00 sec)

DROP TABLE ft_mroonga;
CREATE TABLE ft_mroonga(copy TEXT,FULLTEXT(copy) COMMENT 'parser "TokenBigramSplitSymbolAlpha"') 
  ENGINE=Mroonga;
INSERT INTO ft_mroonga(copy) VALUES ('Once upon a time'),   
  ('There was a wicked witch'), 
  ('Who ate everybody up'), 
  ('She met a wicked, wicked witch'), 
  ('A really wicked, wicked witch!!?!');
SELECT * FROM ft_mroonga WHERE MATCH(copy) AGAINST('ick');
+-----------------------------------+
| copy                              |
+-----------------------------------+
| There was a wicked witch          |
| She met a wicked, wicked witch    |
| A really wicked, wicked witch!!?! |
+-----------------------------------+
```