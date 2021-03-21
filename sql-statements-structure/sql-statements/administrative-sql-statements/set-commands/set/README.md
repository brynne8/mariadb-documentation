# SET

## Syntax

```sql
SET variable_assignment [, variable_assignment] ...

variable_assignment:
      user_var_name = expr
    | [GLOBAL | SESSION] system_var_name = expr
    | [@@global. | @@session. | @@]system_var_name = expr
```

One can also set a user variable in any expression with this syntax:

```sql
user_var_name:= expr
```

## Description

The <code class="fixed" style="white-space:pre-wrap">SET</code> statement assigns values to different types of
variables that affect the operation of the server or your client. Older
versions of MySQL employed <code class="fixed" style="white-space:pre-wrap">SET OPTION</code>, but this syntax was
deprecated in favor of <code class="fixed" style="white-space:pre-wrap">SET</code> without <code class="fixed" style="white-space:pre-wrap">OPTION</code>, and was removed in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

Changing a system variable by using the SET statement does not make the change permanently. To do so, the change must be made in a [configuration file](/mariadb-administration/getting-installing-and-upgrading-mariadb/mysqld-configuration-files-and-groups/).

For setting variables on a per-query basis (from [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)), see [SET STATEMENT](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set-statement/).

See [SHOW VARIABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables/) for documentation on viewing server system variables.

See [Server System Variables](/replication/optimization-and-tuning/system-variables/server-system-variables/) for a list of all the system variables.

### GLOBAL / SESSION

When setting a system variable, the scope can be specified as either GLOBAL or SESSION.

A global variable change affects all new sessions. It does not affect any currently open sessions, including the one that made the change.

A session variable change affects the current session only.

If the variable has a session value, not specifying either GLOBAL or SESSION will be the same as specifying SESSION. If the variable only has a global value, not specifying GLOBAL or SESSION will apply to the change to the global value.

### DEFAULT

Setting a global variable to DEFAULT will restore it to the server default, and setting a session variable to DEFAULT will restore it to the current global value.

## Examples

- [innodb_sync_spin_loops](/kb/en/xtradbinnodb-server-system-variables/#innodb_sync_spin_loops) is a global variable.
- [skip_parallel_replication](/kb/en/replication-and-binary-log-server-system-variables/#skip_parallel_replication) is a session variable.
- [max_error_count](/kb/en/server-system-variables/#max_error_count) is both global and session.

```sql
SELECT VARIABLE_NAME, SESSION_VALUE, GLOBAL_VALUE FROM
 INFORMATION_SCHEMA.SYSTEM_VARIABLES WHERE 
  VARIABLE_NAME IN ('max_error_count', 'skip_parallel_replication', 'innodb_sync_spin_loops');
+---------------------------+---------------+--------------+
| VARIABLE_NAME             | SESSION_VALUE | GLOBAL_VALUE |
+---------------------------+---------------+--------------+
| MAX_ERROR_COUNT           | 64            | 64           |
| SKIP_PARALLEL_REPLICATION | OFF           | NULL         |
| INNODB_SYNC_SPIN_LOOPS    | NULL          | 30           |
+---------------------------+---------------+--------------+
```

Setting the session values:

```sql
SET max_error_count=128;Query OK, 0 rows affected (0.000 sec)

SET skip_parallel_replication=ON;Query OK, 0 rows affected (0.000 sec)

SET innodb_sync_spin_loops=60;
ERROR 1229 (HY000): Variable 'innodb_sync_spin_loops' is a GLOBAL variable 
  and should be set with SET GLOBAL

SELECT VARIABLE_NAME, SESSION_VALUE, GLOBAL_VALUE FROM
 INFORMATION_SCHEMA.SYSTEM_VARIABLES WHERE 
  VARIABLE_NAME IN ('max_error_count', 'skip_parallel_replication', 'innodb_sync_spin_loops');
+---------------------------+---------------+--------------+
| VARIABLE_NAME             | SESSION_VALUE | GLOBAL_VALUE |
+---------------------------+---------------+--------------+
| MAX_ERROR_COUNT           | 128           | 64           |
| SKIP_PARALLEL_REPLICATION | ON            | NULL         |
| INNODB_SYNC_SPIN_LOOPS    | NULL          | 30           |
+---------------------------+---------------+--------------+
```

Setting the global values:

```sql
SET GLOBAL max_error_count=256;

SET GLOBAL skip_parallel_replication=ON;
ERROR 1228 (HY000): Variable 'skip_parallel_replication' is a SESSION variable 
  and can't be used with SET GLOBAL

SET GLOBAL innodb_sync_spin_loops=120;

SELECT VARIABLE_NAME, SESSION_VALUE, GLOBAL_VALUE FROM
 INFORMATION_SCHEMA.SYSTEM_VARIABLES WHERE 
  VARIABLE_NAME IN ('max_error_count', 'skip_parallel_replication', 'innodb_sync_spin_loops');
+---------------------------+---------------+--------------+
| VARIABLE_NAME             | SESSION_VALUE | GLOBAL_VALUE |
+---------------------------+---------------+--------------+
| MAX_ERROR_COUNT           | 128           | 256          |
| SKIP_PARALLEL_REPLICATION | ON            | NULL         |
| INNODB_SYNC_SPIN_LOOPS    | NULL          | 120          |
+---------------------------+---------------+--------------+
```

[SHOW VARIABLES](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-variables/) will by default return the session value unless the variable is global only.

```sql
SHOW VARIABLES LIKE 'max_error_count';
+-----------------+-------+
| Variable_name   | Value |
+-----------------+-------+
| max_error_count | 128   |
+-----------------+-------+

SHOW VARIABLES LIKE 'skip_parallel_replication';
+---------------------------+-------+
| Variable_name             | Value |
+---------------------------+-------+
| skip_parallel_replication | ON    |
+---------------------------+-------+

SHOW VARIABLES LIKE 'innodb_sync_spin_loops';
+------------------------+-------+
| Variable_name          | Value |
+------------------------+-------+
| innodb_sync_spin_loops | 120   |
+------------------------+-------+
```

Using the inplace syntax:

```sql
SELECT (@a:=1);
+---------+
| (@a:=1) |
+---------+
|       1 |
+---------+

SELECT @a;
+------+
| @a   |
+------+
|    1 |
+------+
```

## See Also

- [Using last_value() to return data of used rows](/built-in-functions/secondary-functions/information-functions/last_value/)
- [SET STATEMENT](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set-statement/)
- [SET Variable](/programming-customizing-mariadb/programmatic-compound-statements/set-variable/)
- [SET Data Type](/columns-storage-engines-and-plugins/data-types/string-data-types/set-data-type/)
- [DECLARE Variable](/programming-customizing-mariadb/programmatic-compound-statements/declare-variable/)