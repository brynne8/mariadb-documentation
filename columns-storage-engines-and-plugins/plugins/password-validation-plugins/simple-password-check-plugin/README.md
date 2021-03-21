# Simple Password Check Plugin

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

The `simple_password_check` plugin was first released in [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/).

`simple_password_check` is a [password validation](/kb/en/password-validation-plugin-api/) plugin. It can check whether a password contains at least a certain number of characters of a specific type. When first installed, a password is required to be at least eight characters, and requires at least one digit, one uppercase character, one lowercase character, and one character that is neither a digit nor a letter.

Note that passwords can be directly set as a hash, bypassing the password validation, if the [strict_password_validation](/kb/en/server-system-variables/#strict_password_validation) variable is `OFF` (it is `ON` by default).

## Installing the Plugin

Although the plugin's shared library is distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin). For example:

```sql
INSTALL SONAME 'simple_password_check';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariadb]
...
plugin_load_add = simple_password_check
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin). For example:

```sql
UNINSTALL SONAME 'simple_password_check';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Example

When creating a new password, if the criteria are not met, the following error is returned:

```sql
SET PASSWORD FOR 'bob'@'%.loc.gov' = PASSWORD('abc');
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
```

## Known Issues

### Issues with PAM Authentication Plugin

Prior to [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/), all [password validation plugins](/columns-storage-engines-and-plugins/plugins/password-validation-plugins) are incompatible with the <a undefined>pam</a> authentication plugin. See [Authentication Plugin - PAM: Conflicts with Password Validation](/kb/en/authentication-plugin-pam/#conflicts-with-password-validation) for more information.

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>1.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10118-release-notes/">MariaDB 10.1.18</a></td></tr>
<tr><td>1.0</td><td>Gamma</td><td><a href="/kb/en/mariadb-10113-release-notes/">MariaDB 10.1.13</a></td></tr>
<tr><td>1.0</td><td>Beta</td><td><a href="/kb/en/mariadb-10111-release-notes/">MariaDB 10.1.11</a></td></tr>
<tr><td>1.0</td><td>Alpha</td><td><a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a></td></tr>
</tbody></table>

## System Variables

#### `simple_password_check_digits`

- <strong>Description:</strong> A password must contain at least this many digits.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--simple-password-check-digits=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `1000`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

---

#### `simple_password_check_letters_same_case`

- <strong>Description:</strong> A password must contain at least this many upper-case and this many lower-case letters.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--simple-password-check-letters-same-case=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `1000`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

---

#### `simple_password_check_minimal_length`

- <strong>Description:</strong> A password must contain at least this many characters.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--simple-password-check-minimal-length=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `8`
- <strong>Range:</strong> `0` to `1000`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

---

#### `simple_password_check_other_characters`

- <strong>Description:</strong> A password must contain at least this many characters that are neither digits nor letters.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--simple-password-check-other-characters=#</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> Yes
- <strong>Data Type:</strong> `numeric`
- <strong>Default Value:</strong> `1`
- <strong>Range:</strong> `0` to `1000`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

---

## Options

#### `simple_password_check`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the <a undefined>mysql.plugins</a> table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--simple-password-check=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

---

## See also

- [Password Validation](/kb/en/password-validation/)
- [cracklib_password_check plugin](/kb/en/cracklib_password_check/) - use the Cracklib password-strength checking library