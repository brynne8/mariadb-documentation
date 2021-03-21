# SLEEP

## Syntax

```sql
SLEEP(duration)
```

## Description

Sleeps (pauses) for the number of seconds given by the duration argument, then
returns <code class="highlight fixed" style="white-space:pre-wrap">0</code>. If <code class="highlight fixed" style="white-space:pre-wrap">SLEEP()</code> is interrupted, it
returns <code class="highlight fixed" style="white-space:pre-wrap">1</code>. The duration may have a fractional part given in
microseconds.

Statements using the SLEEP() function are not [safe for replication](/kb/en/unsafe-statements-for-replication/).

## Example

```sql
SELECT SLEEP(5.5);
+------------+
| SLEEP(5.5) |
+------------+
|          0 |
+------------+
1 row in set (5.50 sec)
```