# NEXT VALUE for sequence_name

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

SEQUENCEs were introduced in [MariaDB 10.3](/kb/en/what-is-mariadb-103/)

## Syntax

```sql
NEXT VALUE FOR sequence
```

or

```sql
NEXTVAL(sequence_name)
```

or in Oracle mode ([SQL_MODE=ORACLE](/mariadb-administration/variables-and-modes/sql-mode/))

```sql
sequence_name.nextval
```

`NEXT VALUE FOR` is ANSI SQL syntax while `NEXTVAL()` is PostgreSQL syntax.

## Description

Generate next value for a `SEQUENCE`.

- You can greatly speed up `NEXT VALUE` by creating the sequence with the `CACHE` option. If not, every `NEXT VALUE` usage will cause changes in the stored `SEQUENCE` table.
- When using `NEXT VALUE` the value will be reserved at once and will not be reused, except if the `SEQUENCE` was created with `CYCLE`. This means that when you are using `SEQUENCE`s you have to expect gaps in the generated sequence numbers.
- If one updates the `SEQUENCE` with [SETVAL()](/sql-statements-structure/sequences/sequence-functions/setval/) or [ALTER SEQUENCE ... RESTART](/sql-statements-structure/sequences/alter-sequence/), `NEXT VALUE FOR` will notice this and start from the next requested value.
- [FLUSH TABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) will close the sequence and the next sequence number generated will be according to what's stored in the `SEQUENCE` object. In effect, this will discard the cached values.
- A server restart (or closing the current connection) also causes a drop of all cached values. The cached sequence numbers are reserved only for the current connection.
- `NEXT VALUE` requires the `INSERT` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/).

##### MariaDB starting with [10.3.3](/kb/en/mariadb-1033-release-notes/)

- You can also use `NEXT VALUE FOR sequence` for column `DEFAULT`.

## See Also

- [Sequence Overview](/sql-statements-structure/sequences/sequence-overview/)
- [CREATE SEQUENCE](/sql-statements-structure/sequences/create-sequence/)
- [ALTER SEQUENCE](/sql-statements-structure/sequences/alter-sequence/)
- [PREVIOUS VALUE FOR](/sql-statements-structure/sequences/sequence-functions/previous-value-for-sequence_name/)
- [SETVAL()](/sql-statements-structure/sequences/sequence-functions/setval/). Set next value for the sequence.
- [AUTO_INCREMENT](/columns-storage-engines-and-plugins/data-types/auto_increment/)