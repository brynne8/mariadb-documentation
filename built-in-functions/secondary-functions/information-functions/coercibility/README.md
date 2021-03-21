# COERCIBILITY

## Syntax

```sql
COERCIBILITY(str)
```

## Description

Returns the collation coercibility value of the string argument. Coercibility defines what will be converted to what in case of collation conflict, with an expression with higher coercibility being converted to the collation of an expression with lower coercibility.

<table><tbody><tr><th>Coercibility</th><th>Description</th><th>Example</th></tr>
<tr><td>0</td><td>Explicit</td><td>Value using a COLLATE clause</td></tr>
<tr><td>1</td><td>No collation</td><td>Concatenated strings using different collations</td></tr>
<tr><td>2</td><td>Implicit</td><td>Column value</td></tr>
<tr><td>3</td><td>Constant</td><td>USER() return value</td></tr>
<tr><td>4</td><td>Coercible</td><td>Literal string</td></tr>
<tr><td>5</td><td>Ignorable</td><td>NULL or derived from NULL</td></tr>
</tbody></table>

## Examples

```sql
SELECT COERCIBILITY('abc' COLLATE latin1_swedish_ci);
+-----------------------------------------------+
| COERCIBILITY('abc' COLLATE latin1_swedish_ci) |
+-----------------------------------------------+
|                                             0 |
+-----------------------------------------------+

SELECT COERCIBILITY(USER());
+----------------------+
| COERCIBILITY(USER()) |
+----------------------+
|                    3 |
+----------------------+

SELECT COERCIBILITY('abc');
+---------------------+
| COERCIBILITY('abc') |
+---------------------+
|                   4 |
+---------------------+
```