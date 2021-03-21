# Information Schema VIEWS Table

The [Information Schema](/kb/en/information_schema/) `VIEWS` table contains information about [views](/programming-customizing-mariadb/views). The `SHOW VIEW` [privilege](/sql-statements-structure/sql-statements/account-management-sql-commands/grant) is required to view the table.

It has the following columns:

<table><tbody><tr><th>Column</th><th>Description</th><th>Added</th></tr>
<tr><td><code>TABLE_CATALOG</code></td><td>Aways <code>def</code>.</td></tr>
<tr><td><code>TABLE_SCHEMA</code></td><td>Database name containing the view.</td><td></td></tr>
<tr><td><code>TABLE_NAME</code></td><td>View table name.</td><td></td></tr>
<tr><td><code>VIEW_DEFINITION</code></td><td>Definition of the view.</td><td></td></tr>
<tr><td><code>CHECK_OPTION</code></td><td><code>YES</code> if the <code>WITH CHECK_OPTION</code> clause has been specified, <code>NO</code> otherwise.</td><td></td></tr>
<tr><td><code>IS_UPDATABLE</code></td><td>Whether the view is updatable or not.</td><td></td></tr>
<tr><td><code>DEFINER</code></td><td>Account specified in the DEFINER clause (or the default when created).</td><td></td></tr>
<tr><td><code>SECURITY_TYPE</code></td><td><code>SQL SECURITY</code> characteristic, either <code>DEFINER</code> or <code>INVOKER</code>.</td><td></td></tr>
<tr><td><code>CHARACTER_SET_CLIENT</code></td><td>The client <a href="/kb/en/data-types-character-sets-and-collations/">character set</a> when the view was created, from the session value of the <code><a href="/kb/en/server-system-variables/#character_set_client">character_set_client</a></code> system variable.</td><td></td></tr>
<tr><td><code>COLLATION_CONNECTION</code></td><td>The client <a href="/kb/en/data-types-character-sets-and-collations/">collation</a> when the view was created, from the session value of the <a href="/kb/en/server-system-variables/#collation_connection">collation_connection</a> system variable.</td><td></td></tr>
<tr><td><code>ALGORITHM</code></td><td>The algorithm used in the view. See <a href="/kb/en/view-algorithms/">View Algorithms</a>.</td><td><a href="/kb/en/mariadb-1013-release-notes/">MariaDB 10.1.3</a></td></tr>
</tbody></table>

## Example

```sql
SELECT * FROM information_schema.VIEWS\G
*************************** 1. row ***************************
       TABLE_CATALOG: def
        TABLE_SCHEMA: test
          TABLE_NAME: v
     VIEW_DEFINITION: select `test`.`t`.`qty` AS `qty`,`test`.`t`.`price` AS `price`,(`test`.`t`.`qty` * `test`.`t`.`price`) AS `value` from `test`.`t`
        CHECK_OPTION: NONE
        IS_UPDATABLE: YES
             DEFINER: root@localhost
       SECURITY_TYPE: DEFINER
CHARACTER_SET_CLIENT: utf8
COLLATION_CONNECTION: utf8_general_ci
           ALGORITHM: UNDEFINED
```

## See also

- [CREATE VIEW](/programming-customizing-mariadb/views/create-view)
- [ALTER VIEW](/programming-customizing-mariadb/views/alter-view)
- [DROP VIEW](/programming-customizing-mariadb/views/drop-view)
- <a undefined>SHOW CREATE VIEWS</a>