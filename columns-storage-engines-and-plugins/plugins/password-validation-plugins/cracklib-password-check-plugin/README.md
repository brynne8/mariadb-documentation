# Cracklib Password Check Plugin

##### MariaDB starting with [10.1.2](/kb/en/mariadb-1012-release-notes/)

The `cracklib_password_check` plugin was first released in [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/).

The plugin requires at least cracklib 2.9.0, so it is not available on Debian/Ubuntu builds before Debian 8 Jessie/Ubuntu 14.04 Trusty, RedHat Enterprise Linux / CentOS 6. (see [MDEV-7305](https://jira.mariadb.org/browse/MDEV-7305)).

`cracklib_password_check` is a [password validation](/kb/en/password-validation-plugin-api/) plugin. It uses the [CrackLib](http://sourceforge.net/projects/cracklib/) library to check the strength of new passwords. CrackLib is installed by default in many Linux distributions, since the system's [Pluggable Authentication Module (PAM)](https://en.wikipedia.org/wiki/Pluggable_authentication_module) authentication framework is usually configured to check the strength of new passwords with the <a undefined>pam_cracklib</a> PAM module.

Note that passwords can be directly set as a hash, bypassing the password validation, if the [strict_password_validation](/kb/en/server-system-variables/#strict_password_validation) variable is `OFF` (it is `ON` by default).

## Installing the Plugin's Package

The `cracklib_password_check` plugin's shared library is included in MariaDB packages as the `cracklib_password_check.so` or `cracklib_password_check.dll` shared library on systems where it can be built. The plugin was first included in [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/).

### Installing on Linux

The `cracklib_password_check` plugin is included in `systemd` [binary tarballs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs) on Linux, but not in the older generic and `glibc_214` tarballs.

#### Installing with a Package Manager

The `cracklib_password_check` plugin can also be installed via a package manager on Linux. In order to do so, your system needs to be configured to install from one of the MariaDB repositories.

You can configure your package manager to install it from MariaDB Corporation's MariaDB Package Repository by using the [MariaDB Package Repository setup script](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage).

You can also configure your package manager to install it from MariaDB Foundation's MariaDB Repository by using the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/).

##### Installing with yum/dnf

On RHEL, CentOS, Fedora, and other similar Linux distributions, it is highly recommended to install the relevant [RPM package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm) from MariaDB's
repository using [yum](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/yum) or <a undefined>dnf</a>. Starting with RHEL 8 and Fedora 22, `yum` has been replaced by `dnf`, which is the next major version of `yum`. However, `yum` commands still work on many systems that use `dnf`. For example:

```sql
sudo yum install MariaDB-cracklib-password-check
```

##### Installing with apt-get

On Debian, Ubuntu, and other similar Linux distributions, it is highly recommended to install the relevant [DEB package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) from MariaDB's
repository using <a undefined>apt-get</a>. For example:

```sql
sudo apt-get install mariadb-plugin-cracklib-password-check
```

##### Installing with zypper

On SLES, OpenSUSE, and other similar Linux distributions, it is highly recommended to install the relevant [RPM package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm) from MariaDB's repository using [zypper](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/installing-mariadb-with-zypper). For example:

```sql
sudo zypper install MariaDB-cracklib-password-check
```

## Installing the Plugin

Once the shared library is in place, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin). For example:

```sql
INSTALL SONAME 'cracklib_password_check';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariadb]
...
plugin_load_add = cracklib_password_check
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin). For example:

```sql
UNINSTALL SONAME 'cracklib_password_check';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Viewing CrackLib Errors

If password validation fails, then the original CrackLib error message can be viewed by executing [SHOW WARNINGS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-warnings).

## Example

When creating a new password, if the criteria are not met, the following error is returned:

```sql
SET PASSWORD FOR 'bob'@'%.loc.gov' = PASSWORD('abc');
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements
```

## Known Issues

### Issues with PAM Authentication Plugin

Prior to [MariaDB 10.4.0](/kb/en/mariadb-1040-release-notes/), all [password validation plugins](/columns-storage-engines-and-plugins/plugins/password-validation-plugins) are incompatible with the <a undefined>pam</a> authentication plugin. See [Authentication Plugin - PAM: Conflicts with Password Validation](/kb/en/authentication-plugin-pam/#conflicts-with-password-validation) for more information.

### SELinux

When using the standard [SELinux](/mariadb-administration/user-server-security/securing-mariadb/selinux) policy with the [mode](/kb/en/selinux/#changing-selinuxs-mode) set to `enforcing`, `mysqld` does not have access to `/usr/share/cracklib`, and you may see the following error when attempting to use the `cracklib_password_check` plugin:

```sql
CREATE USER `user`@`hostname` IDENTIFIED BY 's0mePwd123.';
ERROR 1819 (HY000): Your password does not satisfy the current policy requirements

SHOW WARNINGS;
+---------+------+----------------------------------------------------------------+
| Level   | Code | Message                                                        |
+---------+------+----------------------------------------------------------------+
| Warning | 1819 | cracklib: error loading dictionary                             |
| Error   | 1819 | Your password does not satisfy the current policy requirements |
| Error   | 1396 | Operation CREATE USER failed for 'user'@'hostname'             |
+---------+------+----------------------------------------------------------------+
```

And the SELinux `audit.log` will contain errors like the following:

```sql
type=AVC msg=audit(1548371977.821:66): avc:  denied  { read } for  pid=3537 comm="mysqld" name="pw_dict.pwd" dev="xvda2" ino=564747 scontext=system_u:system_r:mysqld_t:s0 tcontext=system_u:object_r:crack_db_t:s0 tclass=file
type=SYSCALL msg=audit(1548371977.821:66): arch=c000003e syscall=2 success=no exit=-13 a0=7fdd2a674580 a1=0 a2=1b6 a3=1b items=0 ppid=1 pid=3537 auid=4294967295 uid=995 gid=992 euid=995 suid=995 fsuid=995 egid=992 sgid=992 fsgid=992 tty=(none) ses=4294967295 comm="mysqld" exe="/usr/sbin/mysqld" subj=system_u:system_r:mysqld_t:s0 key=(null)
```

This can be fixed by creating an SELinux policy that allows `mysqld` to load the CrackLib dictionary. For example:

```sql
cd /usr/share/mysql/policy/selinux/
tee ./mariadb-plugin-cracklib-password-check.te <<EOF

module mariadb-plugin-cracklib-password-check 1.0;

require {
        type mysqld_t;
        type crack_db_t;
        class file { execute setattr read create getattr execute_no_trans write ioctl open append unlink };
        class dir { write search getattr add_name read remove_name open };
}

allow mysqld_t crack_db_t:dir { search read open };
allow mysqld_t crack_db_t:file { getattr read open };
EOF
sudo yum install selinux-policy-devel
make -f /usr/share/selinux/devel/Makefile mariadb-plugin-cracklib-password-check.pp
sudo semodule -i mariadb-plugin-cracklib-password-check.pp
```

See [MDEV-18374](https://jira.mariadb.org/browse/MDEV-18374) for more information.

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>1.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10118-release-notes/">MariaDB 10.1.18</a></td></tr>
<tr><td>1.0</td><td>Gamma</td><td><a href="/kb/en/mariadb-10113-release-notes/">MariaDB 10.1.13</a></td></tr>
<tr><td>1.0</td><td>Alpha</td><td><a href="/kb/en/mariadb-1012-release-notes/">MariaDB 10.1.2</a></td></tr>
</tbody></table>

## System Variables

#### `cracklib_password_check_dictionary`

- <strong>Description:</strong> Sets the path to the CrackLib dictionary. If not set, the default CrackLib dictionary path is used. The parameter expects the base name of a cracklib dictionary (a set of three files with endings `.hwm`, `.pwd`, `.pwi`), not a directory path.
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--cracklib-password-check-dictionary=value</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> Depends on the system. Often `/usr/share/cracklib/pw_dict`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

---

## Options

#### `cracklib_password_check`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the <a undefined>mysql.plugins</a> table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--cracklib-password-check=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`
- <strong>Introduced:</strong> [MariaDB 10.1.2](/kb/en/mariadb-1012-release-notes/)

---

## See Also

- [Password Validation](/kb/en/password-validation/)
- [simple_password_check plugin](/kb/en/simple_password_check/) - permits the setting of basic criteria for passwords