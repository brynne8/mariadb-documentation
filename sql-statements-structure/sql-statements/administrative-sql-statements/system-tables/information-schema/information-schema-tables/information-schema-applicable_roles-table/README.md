# Information Schema APPLICABLE_ROLES Table

The [Information Schema](/kb/en/information_schema/) `APPLICABLE_ROLES` table shows the [role authorizations](/mariadb-administration/user-server-security/user-account-management/roles/) that the current user may use.

It contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th><th>Added</th></tr>
<tr><td><code>GRANTEE</code></td><td>Account that the role was granted to.</td><td></td></tr>
<tr><td><code>ROLE_NAME</code></td><td>Name of the role.</td><td></td></tr>
<tr><td><code>IS_GRANTABLE</code></td><td>Whether the role can be granted or not.</td><td></td></tr>
<tr><td><code>IS_DEFAULT</code></td><td>Whether the role is the user's default role or not</td><td><a href="/kb/en/mariadb-1013-release-notes/">MariaDB 10.1.3</a></td></tr>
</tbody></table>

The current role is in the [ENABLED_ROLES](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-enabled_roles-table/) Information Schema table.

## Example

```sql
SELECT * FROM information_schema.APPLICABLE_ROLES;
+----------------+-------------+--------------+------------+
| GRANTEE        | ROLE_NAME   | IS_GRANTABLE | IS_DEFAULT |
+----------------+-------------+--------------+------------+
| root@localhost | journalist  | YES          | NO         |
| root@localhost | staff       | YES          | NO         |
| root@localhost | dd          | YES          | NO         |
| root@localhost | dog         | YES          | NO         |
+----------------+-------------+--------------+------------+
```