# DO

## Syntax

```sql
DO expr [, expr] ...
```

## Description

<code class="highlight fixed" style="white-space:pre-wrap">DO</code> executes the expressions but does not return any
results. In most respects, <code class="highlight fixed" style="white-space:pre-wrap">DO</code> is shorthand for
 <code class="highlight fixed" style="white-space:pre-wrap">SELECT expr, ...,</code> but has the advantage that it is slightly
faster when you do not care about the result.

<code class="highlight fixed" style="white-space:pre-wrap">DO</code> is useful primarily with functions that have side
 effects, such as <code class="highlight fixed" style="white-space:pre-wrap">[RELEASE_LOCK()](/built-in-functions/secondary-functions/miscellaneous-functions/release_lock/)</code>.