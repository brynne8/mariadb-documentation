# SAVEPOINT

## Syntax

```sql
SAVEPOINT identifier
ROLLBACK [WORK] TO [SAVEPOINT] identifier
RELEASE SAVEPOINT identifier
```

## Description

InnoDB supports the SQL statements <code class="highlight fixed" style="white-space:pre-wrap">SAVEPOINT</code>,
<code class="highlight fixed" style="white-space:pre-wrap">ROLLBACK TO SAVEPOINT</code>, <code class="highlight fixed" style="white-space:pre-wrap">RELEASE SAVEPOINT</code>
and the optional <code class="highlight fixed" style="white-space:pre-wrap">WORK</code> keyword for
<code class="highlight fixed" style="white-space:pre-wrap">ROLLBACK</code>.

Each savepoint must have a legal [MariaDB identifier](/sql-statements-structure/sql-language-structure/identifier-names/). A savepoint is a named sub-transaction.

Normally [ROLLBACK](/sql-statements-structure/sql-statements/transactions/rollback/) undoes the changes performed by the whole transaction. When used with the TO clause, it undoes the changes performed after the specified savepoint, and erases all subsequent savepoints. However, all locks that have been acquired after the save point will survive. RELEASE SAVEPOINT does not rollback or commit any changes, but removes the specified savepoint.

When the execution of a trigger or a stored function begins, it is not possible to use statements which reference a savepoint which was defined from out of that stored program.

When a [COMMIT](/sql-statements-structure/sql-statements/transactions/commit/) (including implicit commits) or a ROLLBACK statement (with no TO clause) is performed, they act on the whole transaction, and all savepoints are removed.

## Errors

If COMMIT or ROLLBACK is issued and no transaction was started, no error is reported.

If SAVEPOINT is issued and no transaction was started, no error is reported but no savepoint is created. When ROLLBACK TO SAVEPOINT or RELEASE SAVEPOINT is called for a savepoint that does not exist, an error like this is issued:

```sql
ERROR 1305 (42000): SAVEPOINT svp_name does not exist
```