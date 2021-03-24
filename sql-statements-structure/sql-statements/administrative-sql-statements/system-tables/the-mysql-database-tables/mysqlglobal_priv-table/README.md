# mysql.global_priv Table

##### MariaDB starting with [10.4.1](/kb/en/mariadb-1041-release-notes/)

The `mysql.global_priv` table was introduced in [MariaDB 10.4.1](/kb/en/mariadb-1041-release-notes/) to replace the [mysql.user](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables/mysqluser-table/) table.

The `mysql.global_priv` table contains information about users that have permission to access the MariaDB server, and their global privileges.

Note that the MariaDB privileges occur at many levels. A user may not be granted `create` privilege at the user level, but may still have `create` permission on certain tables or databases, for example. See [privileges](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) for a more complete view of the MariaDB privilege system.

The `mysql.global_priv` table contains the following fields:

<table><tbody><tr><th>Field</th><th>Type</th><th>Null</th><th>Key</th><th>Default</th><th>Description</th></tr>
<tr><td><code>Host</code></td><td><code>char(60)</code></td><td>NO</td><td>PRI</td><td></td><td>Host (together with <code>User</code> makes up the unique identifier for this account).</td></tr>
<tr><td><code>User</code></td><td><code>char(80)</code></td><td>NO</td><td>PRI</td><td></td><td>User (together with <code>Host</code> makes up the unique identifier for this account).</td></tr>
<tr><td><code>Priv</code></td><td><code>longtext</code></td><td>NO</td><td></td><td></td><td>Global privileges, granted to the account and other account properties</td></tr>
</tbody></table>

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/), in order to help the server understand which version a privilege record was written by, the `priv` field contains a new JSON field, `version_id` ([MDEV-21704](https://jira.mariadb.org/browse/MDEV-21704)).

## Examples

```sql
select * from mysql.global_priv;
+-----------+-------------+---------------------------------------------------------------------------------------------------------------------------------------+
| Host      | User        | Priv                                                                                                                                  |
+-----------+-------------+---------------------------------------------------------------------------------------------------------------------------------------+
| localhost | root        | {"access": 18446744073709551615,"plugin":"mysql_native_password","authentication_string":"*6C387FC3893DBA1E3BA155E74754DA6682D04747"} |
| 127.%     | msandbox    | {"access":1073740799,"plugin":"mysql_native_password","authentication_string":"*6C387FC3893DBA1E3BA155E74754DA6682D04747"}            |
| localhost | msandbox    | {"access":1073740799,"plugin":"mysql_native_password","authentication_string":"*6C387FC3893DBA1E3BA155E74754DA6682D04747"}            |
| localhost | msandbox_rw | {"access":487487,"plugin":"mysql_native_password","authentication_string":"*6C387FC3893DBA1E3BA155E74754DA6682D04747"}                |
| 127.%     | msandbox_rw | {"access":487487,"plugin":"mysql_native_password","authentication_string":"*6C387FC3893DBA1E3BA155E74754DA6682D04747"}                |
| 127.%     | msandbox_ro | {"access":262145,"plugin":"mysql_native_password","authentication_string":"*6C387FC3893DBA1E3BA155E74754DA6682D04747"}                |
| localhost | msandbox_ro | {"access":262145,"plugin":"mysql_native_password","authentication_string":"*6C387FC3893DBA1E3BA155E74754DA6682D04747"}                |
| 127.%     | rsandbox    | {"access":524288,"plugin":"mysql_native_password","authentication_string":"*B07EB15A2E7BD9620DAE47B194D5B9DBA14377AD"}                |
+-----------+-------------+---------------------------------------------------------------------------------------------------------------------------------------+
```

Readable format:

```sql
SELECT CONCAT(user, '@', host, ' => ', JSON_DETAILED(priv)) FROM mysql.global_priv;

+--------------------------------------------------------------------------------------+
| CONCAT(user, '@', host, ' => ', JSON_DETAILED(priv))                                 |
+--------------------------------------------------------------------------------------+
| root@localhost => {
    "access": 18446744073709551615,
    "plugin": "mysql_native_password",
    "authentication_string": "*6C387FC3893DBA1E3BA155E74754DA6682D04747"
} |
| msandbox@127.% => {
    "access": 1073740799,
    "plugin": "mysql_native_password",
    "authentication_string": "*6C387FC3893DBA1E3BA155E74754DA6682D04747"
}           |
+--------------------------------------------------------------------------------------+
```

A particular user:

```sql
SELECT CONCAT(user, '@', host, ' => ', JSON_DETAILED(priv)) FROM mysql.global_priv 
  WHERE user='marijn';
+--------------------------------------------------------------------------------------+
| CONCAT(user, '@', host, ' => ', JSON_DETAILED(priv))                                 |
+--------------------------------------------------------------------------------------+
| marijn@localhost => {
    "access": 0,
    "plugin": "mysql_native_password",
    "authentication_string": "",
    "account_locked": true,
    "password_last_changed": 1558017158
} |
+--------------------------------------------------------------------------------------+
```

From [MariaDB 10.5.2](/kb/en/mariadb-1052-release-notes/):

```sql
GRANT FILE ON *.* TO user1@localhost;
SELECT Host, User, JSON_DETAILED(Priv) FROM mysql.global_priv WHERE user='user1'\G

*************************** 1. row ***************************
               Host: localhost
               User: user1
JSON_DETAILED(Priv): {
    "access": 512,
    "plugin": "mysql_native_password",
    "authentication_string": "",
    "password_last_changed": 1581070979,
    "version_id": 100502
}
```