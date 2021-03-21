# SETVAL()

##### MariaDB starting with [10.3.1](/kb/en/mariadb-1031-release-notes/)

SEQUENCEs were introduced in [MariaDB 10.3](/kb/en/what-is-mariadb-103/).

## Syntax

```sql
SETVAL(sequence_name, next_value, [is_used, [round]])
```

## Description

Set the next value to be returned for a `SEQUENCE`.

This function is compatible with PostgreSQL syntax, extended
with the `round` argument.

If the `is_used` argument is not given or is `1` or `true`, then the next used value will
one after the given value. If `is_used` is `0` or `false` then the next generated value
will be the given value.

If `round` is used then it will set the `round` value (or the internal cycle count, starting at zero) for the sequence.
If `round` is not used, it's assumed to be 0.

`next_value` must be an integer literal.

For `SEQUENCE` tables defined with `CYCLE` (see [CREATE SEQUENCE](/sql-statements-structure/sequences/create-sequence)) one should use both `next_value` and `round` to define the next value. In this case the
current sequence value is defined to be `round`, `next_value`.

The result returned by `SETVAL()` is `next_value` or NULL if the given `next_value` and `round`  is smaller than the current value.

`SETVAL()` will not set the `SEQUENCE` value to a something that is less than
its current value. This is needed to ensure that `SETVAL()`
is replication safe.  If you want to set the SEQUENCE to a smaller number
use [ALTER SEQUENCE](/sql-statements-structure/sequences/alter-sequence).

If `CYCLE` is used, first `round` and then `next_value` are compared
to see if the value is bigger than the current value.

Internally, in the MariaDB server, `SETVAL()` is used to inform
slaves that a `SEQUENCE` has changed value.  The slave may get
`SETVAL()` statements out of order, but this is ok as only the
biggest one will have an effect.

`SETVAL` requires the [INSERT privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant).

## Examples

```sql
SELECT setval(foo, 42);           -- Next nextval will return 43
SELECT setval(foo, 42, true);     -- Same as above
SELECT setval(foo, 42, false);    -- Next nextval will return 42
```

SETVAL setting higher and lower values on a sequence with an increment of 10:

```sql
SELECT NEXTVAL(s);
+------------+
| NEXTVAL(s) |
+------------+
|         50 |
+------------+

SELECT SETVAL(s, 100);
+----------------+
| SETVAL(s, 100) |
+----------------+
|            100 |
+----------------+

SELECT NEXTVAL(s);
+------------+
| NEXTVAL(s) |
+------------+
|        110 |
+------------+

SELECT SETVAL(s, 50);
+---------------+
| SETVAL(s, 50) |
+---------------+
|          NULL |
+---------------+

SELECT NEXTVAL(s);
+------------+
| NEXTVAL(s) |
+------------+
|        120 |
+------------+
```

Example demonstrating `round`:

```sql
CREATE OR REPLACE SEQUENCE s1
  START WITH 1
  MINVALUE 1
  MAXVALUE 99
  INCREMENT BY 1 
  CACHE 20 
  CYCLE;

SELECT SETVAL(s1, 99, 1, 0);
+----------------------+
| SETVAL(s1, 99, 1, 0) |
+----------------------+
|                   99 |
+----------------------+

SELECT NEXTVAL(s1);
+-------------+
| NEXTVAL(s1) |
+-------------+
|           1 |
+-------------+
```

The following statement returns NULL, as the given `next_value` and `round` is smaller than the current value.

```sql
SELECT SETVAL(s1, 99, 1, 0);
+----------------------+
| SETVAL(s1, 99, 1, 0) |
+----------------------+
|                 NULL |
+----------------------+

SELECT NEXTVAL(s1);
+-------------+
| NEXTVAL(s1) |
+-------------+
|           2 |
+-------------+
```

Increasing the round from zero to 1 will allow `next_value` to be returned.

```sql
SELECT SETVAL(s1, 99, 1, 1);
+----------------------+
| SETVAL(s1, 99, 1, 1) |
+----------------------+
|                   99 |
+----------------------+

SELECT NEXTVAL(s1);
+-------------+
| NEXTVAL(s1) |
+-------------+
|           1 |
+-------------+
```

## See Also

- [Sequence Overview](/sql-statements-structure/sequences/sequence-overview)
- [ALTER SEQUENCE](/sql-statements-structure/sequences/alter-sequence)
- [CREATE SEQUENCE](/sql-statements-structure/sequences/create-sequence)
- [NEXT VALUE FOR](/sql-statements-structure/sequences/sequence-functions/next-value-for-sequence_name)
- [PREVIOUS VALUE FOR](/sql-statements-structure/sequences/sequence-functions/previous-value-for-sequence_name)