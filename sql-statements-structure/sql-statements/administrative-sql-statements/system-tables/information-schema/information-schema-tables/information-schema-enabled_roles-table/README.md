# Information Schema ENABLED_ROLES Table

##### MariaDB [10.0.5](/kb/en/mariadb-1005-release-notes/)

The `ENABLED_ROLES` table was introduced in [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/)

The [Information Schema](/kb/en/information_schema/) `ENABLED_ROLES` table shows the enabled [roles](/mariadb-administration/user-server-security/user-account-management/roles/)  for the current session.

It contains the following column:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>ROLE_NAME</code></td><td>The enabled role name, or <code>NULL</code>.</td></tr>
</tbody></table>

This table lists all roles that are currently enabled, one role per row — the current role, roles granted to the current role, roles granted to these roles and so on. If no role is set, the row contains a `NULL` value.

The roles that the current user can enable are listed in the [APPLICABLE_ROLES](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-applicable_roles-table/) Information Schema table.

See also [CURRENT_ROLE()](/built-in-functions/secondary-functions/information-functions/current_role/).

## Examples

```sql
SELECT * FROM information_schema.ENABLED_ROLES;
+-----------+
| ROLE_NAME |
+-----------+
| NULL      |
+-----------+

SET ROLE staff;

SELECT * FROM information_schema.ENABLED_ROLES;
+-----------+
| ROLE_NAME |
+-----------+
| staff     |
+-----------+
```