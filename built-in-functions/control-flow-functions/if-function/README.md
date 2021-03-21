# IF Function

## Syntax

```sql
IF(expr1,expr2,expr3)
```

## Description

If `expr1` is `TRUE` (`expr1 &lt;&gt; 0` and `expr1 &lt;&gt; NULL`) then `IF()`
returns `expr2`; otherwise it returns `expr3`. `IF()` returns a numeric
or string value, depending on the context in which it is used.

<strong>Note:</strong> There is also an [IF statement](/kb/en/if-statement/) which differs from the
`IF()` function described here.

## Examples

```sql
SELECT IF(1>2,2,3);
+-------------+
| IF(1>2,2,3) |
+-------------+
|           3 |
+-------------+
```

```sql
SELECT IF(1<2,'yes','no');
+--------------------+
| IF(1<2,'yes','no') |
+--------------------+
| yes                |
+--------------------+
```

```sql
SELECT IF(STRCMP('test','test1'),'no','yes');
+---------------------------------------+
| IF(STRCMP('test','test1'),'no','yes') |
+---------------------------------------+
| no                                    |
+---------------------------------------+
```

## See Also

There is also an [IF statement](/kb/en/if-statement/), which differs from the `IF()` function described above.