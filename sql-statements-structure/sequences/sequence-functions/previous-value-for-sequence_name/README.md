# PREVIOUS VALUE FOR sequence_name

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

SEQUENCEs were introduced in [MariaDB 10.3](/kb/en/what-is-mariadb-103/).

## Syntax

```sql
PREVIOUS VALUE FOR sequence_name
```

or

```sql
LASTVAL(sequence_name)
```

or in Oracle mode ([SQL_MODE=ORACLE](/mariadb-administration/variables-and-modes/sql-mode/))

```sql
sequence_name.currval
```

`PREVIOUS VALUE FOR` is IBM DB2 syntax while `LASTVAL()` is PostgreSQL syntax.

## Description

Get last value in the current connection generated from a sequence.

- If the sequence has not yet been used by the connection, `PREVIOUS VALUE FOR` returns `NULL` (the same thing applies with a new connection which doesn't see a last value for an existing sequence).
- If a `SEQUENCE` has been dropped and re-created then it's treated as a new `SEQUENCE` and `PREVIOUS VALUE FOR` will return `NULL`.
- [FLUSH TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) has no effect on `PREVIOUS VALUE FOR`.
- Previous values for all used sequences are stored per connection until connection ends.
- `PREVIOUS VALUE FOR` requires the [SELECT privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/).

## Example

```sql
MariaDB [test]> CREATE SEQUENCE s START WITH 100 INCREMENT BY 10;
Query OK, 0 rows affected (0.026 sec)

MariaDB [test]> SELECT PREVIOUS VALUE FOR s;
+----------------------+
| PREVIOUS VALUE FOR s |
+----------------------+
|                 NULL |
+----------------------+
1 row in set (0.000 sec)

# The function works for sequences only, if the table is used an error is generated
MariaDB [test]> SELECT PREVIOUS VALUE FOR t;
ERROR 4089 (42S02): 'test.t' is not a SEQUENCE

# Call the NEXT VALUE FOR s:
MariaDB [test]> SELECT NEXT VALUE FOR s;
+------------------+
| NEXT VALUE FOR s |
+------------------+
|              100 |
+------------------+
1 row in set (0.000 sec)

MariaDB [test]> SELECT PREVIOUS VALUE FOR s;
+----------------------+
| PREVIOUS VALUE FOR s |
+----------------------+
|                  100 |
+----------------------+
1 row in set (0.000 sec)
```

Now try to start the new connection and check that the last value is still NULL, before updating the value in the new connection after the output of the new connection gets current value (110 in the example below). Note that first connection cannot see this change and the result of last value still remains the same (100 in the example above).

```sql
$ .mysql -uroot test -e"SELECT PREVIOUS VALUE FOR s; SELECT NEXT VALUE FOR s; SELECT PREVIOUS VALUE FOR s;"
+----------------------+
| PREVIOUS VALUE FOR s |
+----------------------+
|                 NULL |
+----------------------+
+------------------+
| NEXT VALUE FOR s |
+------------------+
|              110 |
+------------------+
+----------------------+
| PREVIOUS VALUE FOR s |
+----------------------+
|                  110 |
+----------------------+
```

## See Also

- [Sequence Overview](/sql-statements-structure/sequences/sequence-overview/)
- [CREATE SEQUENCE](/sql-statements-structure/sequences/create-sequence/)
- [ALTER SEQUENCE](/sql-statements-structure/sequences/alter-sequence/)
- [NEXT VALUE FOR](/sql-statements-structure/sequences/sequence-functions/next-value-for-sequence_name/)
- [SETVAL()](/sql-statements-structure/sequences/sequence-functions/setval/). Set next value for the sequence.
- [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/)