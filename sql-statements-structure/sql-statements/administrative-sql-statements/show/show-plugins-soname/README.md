# SHOW PLUGINS SONAME

##### MariaDB [10.0.2](/kb/en/mariadb-1002-release-notes/)

`SHOW PLUGINS SONAME` was introduced in [MariaDB 10.0.2](/kb/en/mariadb-1002-release-notes/)

## Syntax

```sql
SHOW PLUGINS SONAME { library | LIKE 'pattern' | WHERE expr };
```

## Description

<code class="highlight fixed" style="white-space:pre-wrap">SHOW PLUGINS SONAME</code> displays information about compiled-in and all server plugins in the <a undefined>plugin_dir</a> directory, including plugins that haven't been installed.

## Examples

```sql
SHOW PLUGINS SONAME 'ha_example.so';
+----------+---------------+----------------+---------------+---------+
| Name     | Status        | Type           | Library       | License |
+----------+---------------+----------------+---------------+---------+
| EXAMPLE  | NOT INSTALLED | STORAGE ENGINE | ha_example.so | GPL     |
| UNUSABLE | NOT INSTALLED | DAEMON         | ha_example.so | GPL     |
+----------+---------------+----------------+---------------+---------+
```

There is also a corresponding <a undefined>information_schema</a> table, called <a undefined>ALL_PLUGINS</a>, which contains more complete information.