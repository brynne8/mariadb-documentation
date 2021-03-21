# Authentication Plugin - GSSAPI

##### MariaDB starting with [10.1.11](/kb/en/mariadb-10111-release-notes/)

The `gssapi` authentication plugin was first released in [MariaDB 10.1.11](/kb/en/mariadb-10111-release-notes/).

The `gssapi` authentication plugin allows the user to authenticate with services that use the [Generic Security Services Application Program Interface (GSSAPI)](https://en.wikipedia.org/wiki/Generic_Security_Services_Application_Program_Interface). Windows has a slightly different but very similar API called [Security Support Provider Interface (SSPI)](https://docs.microsoft.com/en-us/windows/desktop/secauthn/sspi). The GSSAPI is a standardized API described in [RFC2743](https://tools.ietf.org/html/rfc2743.html) and [RFC2744](https://tools.ietf.org/html/rfc2744.html). The client and server negotiate using a standardized protocol described in [RFC7546](https://tools.ietf.org/html/rfc7546.html).

On Windows, this authentication plugin supports [Kerberos](https://docs.microsoft.com/en-us/windows/desktop/secauthn/microsoft-kerberos) and [NTLM](https://docs.microsoft.com/en-us/windows/desktop/secauthn/microsoft-ntlm) authentication. Windows authentication is supported regardless of whether a [domain](https://en.wikipedia.org/wiki/Windows_domain) is used in the environment.

On Unix systems, the most dominant GSSAPI service is [Kerberos](https://en.wikipedia.org/wiki/Kerberos_(protocol)). However, it is less commonly used on Unix systems than it is on Windows. Regardless, this authentication plugin also supports Kerberos authentication on Unix.

The `gssapi` authentication plugin is most often used for authenticating with [Microsoft Active Directory](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview).

This article gives instructions on configuring the `gssapi` authentication plugin
for MariaDB for passwordless login.

## Installing the Plugin's Package

The `gssapi` authentication plugin's shared library is included in MariaDB packages as the `auth_gssapi.so` or `auth_gssapi.dll` shared library on systems where it can be built. The plugin was first included in [MariaDB 10.1.11](/kb/en/mariadb-10111-release-notes/).

### Installing on Linux

The `gssapi` authentication plugin is included in [binary tarballs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs) on Linux.

#### Installing with a Package Manager

The `gssapi` authentication plugin can also be installed via a package manager on Linux. In order to do so, your system needs to be configured to install from one of the MariaDB repositories.

You can configure your package manager to install it from MariaDB Corporation's MariaDB Package Repository by using the [MariaDB Package Repository setup script](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/mariadb-package-repository-setup-and-usage).

You can also configure your package manager to install it from MariaDB Foundation's MariaDB Repository by using the [MariaDB Repository Configuration Tool](https://downloads.mariadb.org/mariadb/repositories/).

##### Installing with yum/dnf

On RHEL, CentOS, Fedora, and other similar Linux distributions, it is highly recommended to install the relevant [RPM package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm) from MariaDB's
repository using [yum](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/yum) or <a undefined>dnf</a>. Starting with RHEL 8 and Fedora 22, `yum` has been replaced by `dnf`, which is the next major version of `yum`. However, `yum` commands still work on many systems that use `dnf`. For example:

```sql
sudo yum install MariaDB-gssapi-server
```

##### Installing with apt-get

On Debian, Ubuntu, and other similar Linux distributions, it is highly recommended to install the relevant [DEB package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files) from MariaDB's
repository using <a undefined>apt-get</a>. For example:

```sql
sudo apt-get install mariadb-plugin-gssapi-server
```

##### Installing with zypper

On SLES, OpenSUSE, and other similar Linux distributions, it is highly recommended to install the relevant [RPM package](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm) from MariaDB's repository using [zypper](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/installing-mariadb-with-zypper). For example:

```sql
sudo zypper install MariaDB-gssapi-server
```

### Installing on Windows

The `gssapi` authentication plugin is included in [MSI](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-msi-packages-on-windows) and [ZIP](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-windows-zip-packages) packages on Windows.

## Installing the Plugin

Although the plugin's shared library is distributed with MariaDB by default, the plugin is not actually installed by MariaDB by default. There are two methods that can be used to install the plugin with MariaDB.

The first method can be used to install the plugin without restarting the server. You can install the plugin dynamically by executing [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin). For example:

```sql
INSTALL SONAME 'auth_gssapi';
```

The second method can be used to tell the server to load the plugin when it starts up. The plugin can be installed this way by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariadb]
...
plugin_load_add = auth_gssapi
```

## Uninstalling the Plugin

You can uninstall the plugin dynamically by executing [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin). For example:

```sql
UNINSTALL SONAME 'auth_gssapi';
```

If you installed the plugin by providing the <a undefined>--plugin-load</a> or the <a undefined>--plugin-load-add</a> options in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), then those options should be removed to prevent the plugin from being loaded the next time the server is restarted.

## Configuring the Plugin

If the MariaDB server is running on Unix, then some additional configuration steps will need to be implemented in order to use the plugin.

If the MariaDB server is running on Windows, then no special configuration steps will need to be implemented in order to use the plugin, as long as the following is true:

- The Windows server is joined to a domain.
- The MariaDB server process is running as either a [NetworkService Account](https://docs.microsoft.com/en-us/windows/desktop/services/networkservice-account) or a [Domain User Account](https://docs.microsoft.com/en-us/windows/desktop/ad/domain-user-accounts).

### Creating a Keytab File on Unix

If the MariaDB server is running on Unix, then the KDC server will need to create a keytab file for the MariaDB server. The keytab file contains the service principal name, which is the identity that the MariaDB server will use to communicate with the KDC server. The keytab will need to be transferred to the MariaDB server, and the `mysqld` server process will need read access to this keytab file.

How this keytab file is generated depends on whether the KDC server is <strong>[Microsoft Active Directory KDC](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)</strong> or <strong>[MIT Kerberos KDC](http://web.mit.edu/Kerberos/krb5-1.12/doc/index.html)</strong>.

#### Creating a Keytab File with Microsoft Active Directory

If you are using <strong>[Microsoft Active Directory KDC](https://docs.microsoft.com/en-us/windows-server/identity/ad-ds/get-started/virtual-dc/active-directory-domain-services-overview)</strong>, then you may need to create a keytab using the <a undefined>ktpass.exe</a> utility on a Windows host. The service principal will need to be mapped to an existing domain user. To do so, follow the steps listed below.

Be sure to replace the following items in the step below:

- Replace `${HOST}` with the fully qualified DNS name for the MariaDB server host.
- Replace `${DOMAIN}` with the Active Directory domain.
- Replace `${AD_USER}` with the existing domain user.
- Replace `${PASSWORD}` with the password for the service principal.

To create the service principal, execute the following:

```sql
ktpass.exe /princ mariadb/${HOST}@${DOMAIN} /mapuser ${AD_USER} /pass ${PASSWORD} /out mariadb.keytab /crypto all /ptype KRB5_NT_PRINCIPAL /mapop set
```

#### Creating a Keytab File with MIT Kerberos

If you are using <strong>[MIT Kerberos KDC](http://web.mit.edu/Kerberos/krb5-1.12/doc/index.html)</strong>, then you can create a [keytab](http://web.mit.edu/Kerberos/krb5-1.12/doc/admin/install_appl_srv.html#the-keytab-file) file using the <a undefined>kadmin</a> utility. To do so, follow the steps listed below.

In the following steps, be sure to replace `${HOST}` with the fully qualified DNS name for the MariaDB server host.

First, create the service principal using the <a undefined>kadmin</a> utility. For example:

```sql
kadmin -q "addprinc -randkey mariadb/${HOST}"
```

Then, export the newly created user to the keytab file using the <a undefined>kadmin</a> utility. For example:

```sql
kadmin -q "ktadd -k /path/to/mariadb.keytab mariadb/${HOST}"
```

More details can be found at the following links:

- [MIT Kerberos Documentation: Database administration](http://web.mit.edu/Kerberos/krb5-1.12/doc/admin/database.html)
- [MIT Kerberos Documentation: Application servers](http://web.mit.edu/Kerberos/krb5-1.12/doc/admin/appl_servers.html)

### Configuring the Path to the Keytab File on Unix

If the MariaDB server is running on Unix, then the path to the keytab file that was previously created can be set by configuring the <a undefined>gssapi_keytab_path</a> system variable. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariadb]
...
gssapi_keytab_path=/path/to/mariadb.keytab
```

### Configuring the Service Principal Name

The service principal name can be set by configuring the <a undefined>gssapi_principal_name</a> system variable. This can be specified as a command-line argument to [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options) or it can be specified in a relevant server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). For example:

```sql
[mariadb]
...
gssapi_principal_name=service_principal_name/host.domain.com@REALM
```

If a service principal name is not provided, then the plugin will try to use `mariadb/host.domain.com@REALM` by default.

If the MariaDB server is running on Unix, then the plugin needs a service principal name in order to function.

If the MariaDB server is running on Windows, then the plugin does not usually need a service principal in order to function. However, if you want to use one anyway, then one can be created with the <a undefined>setspn</a> utility.

Different KDC implementations may use different canonical forms to identify principals. See [RFC2744: Section 3.10](https://tools.ietf.org/html/rfc2744.html#section-3.10) to learn what the standard says about principal names.

More details can be found at the following links:

- [Active Directory Domain Services: Service Principal Names](https://docs.microsoft.com/en-us/windows/win32/ad/service-principal-names)
- [MIT Kerberos Documentation: Realm configuration decisions](http://web.mit.edu/Kerberos/krb5-1.12/doc/admin/realm_config.html)
- [MIT Kerberos Documentation: Principal names and DNS](http://web.mit.edu/Kerberos/krb5-1.12/doc/admin/princ_dns.html)

## Creating Users

To create a user account via [CREATE USER](/sql-statements-structure/sql-statements/account-management-sql-commands/create-user), specify the name of the plugin in the <a undefined>IDENTIFIED VIA</a> clause. For example:

```sql
CREATE USER username@hostname IDENTIFIED VIA gssapi;
```

If [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) does not have `NO_AUTO_CREATE_USER` set, then you can also create the user account via [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant). For example:

```sql
GRANT SELECT ON db.* TO username@hostname IDENTIFIED VIA gssapi;
```

You can also specify the user's [realm](https://docs.microsoft.com/en-us/windows-server/networking/technologies/nps/nps-crp-realm-names) for MariaDB with the `USING` clause. For example:

```sql
CREATE USER username@hostname IDENTIFIED VIA gssapi USING 'username@EXAMPLE.COM';
```

The format of the realm depends on the specific authentication mechanism that is used. For example, the format would need to be `machine\\username` for Windows users authenticating with NTLM.

If the realm is not provided in the user account's definition, then the realm is <strong>not</strong> used for comparison. Therefore, 'usr1@EXAMPLE.COM', 'usr1@EXAMPLE.CO.UK' and 'mymachine\usr1' would all identify as the following user account:

```sql
CREATE USER usr1@hostname IDENTIFIED VIA gssapi;
```

## Client Authentication Plugins

For clients that use the `libmysqlclient` or [MariaDB Connector/C](/kb/en/mariadb-connector-c/) libraries, MariaDB provides one client authentication plugin that is compatible with the `gssapi` authentication plugin:

- `auth_gssapi_client`

When connecting with a [client or utility](/clients-utilities) to a server as a user account that authenticates with the `gssapi` authentication plugin, you may need to tell the client where to find the relevant client authentication plugin by specifying the `--plugin-dir` option. For example:

```sql
mysql --plugin-dir=/usr/local/mysql/lib64/mysql/plugin --user=alice
```

### `auth_gssapi_client`

The `auth_gssapi_client` client authentication plugin receives the principal name from the server, and then uses either the <a undefined>gss_init_sec_context</a> function (on Unix) or the <a undefined>InitializeSecurityContext</a> function (on Windows) to establish a security context on the client.

## Support in Client Libraries

### Using the Plugin with MariaDB Connector/C

[MariaDB Connector/C](/kb/en/mariadb-connector-c/) supports `gssapi` authentication using the [client authentication plugins](#client-authentication-plugins) mentioned in the previous section since MariaDB Connector/C 3.0.1.

### Using the Plugin with MariaDB Connector/ODBC

[MariaDB Connector/ODBC](/kb/en/mariadb-connector-odbc/) supports `gssapi` authentication using the [client authentication plugins](#client-authentication-plugins) mentioned in the previous section since MariaDB Connector/ODBC 3.0.0.

### Using the Plugin with MariaDB Connector/J

[MariaDB Connector/J](/kb/en/mariadb-connector-j/) supports `gssapi` authentication since MariaDB Connector/J 1.4.0. Current documentation can be found [here](/kb/en/gssapi-authentication-with-mariadb-connectorj/).

### Using the Plugin with MariaDB Connector/Node.js

[MariaDB Connector/Node.js](/kb/en/nodejs-connector/) does not yet support `gssapi` authentication. See [CONJS-72](https://jira.mariadb.org/browse/CONJS-72) for more information.

### Using the Plugin with MySqlConnector for .NET

[MySqlConnector for ADO.NET](/kb/en/mysqlconnector-for-adonet/) supports `gssapi` authentication since MySqlConnector 0.47.0.

The support is transparent. Normally, the connector only needs to be provided the correct user name, and no other parameters are required.

However, this connector also supports the <a undefined>ServerSPN</a> connection string parameter, which can be used for mutual authentication.

#### .NET specific problems/workarounds

When connecting from Unix client to Windows server with ADO.NET, in an Active Directory domain environment, be aware that .NET Core on Unix does not support principal names in UPN(User Principal Name) form, which is default on Windows (e.g machine$@domain.com) . Thus, upon encountering an authentication exception with "server not found in Kerberos database", use one of workarounds below

- Force host-based SPN on server side.
<ul start="1"><li>For example, this can be done by setting the <a undefined>gssapi_principal_name</a> system variable to `HOST/machine` in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).
</li></ul>
- Pass host-based SPN on client side.
<ul start="1"><li>For example, this can be done by setting the connector's <a undefined>ServerSPN</a> connection string parameter to `HOST/machine`.
</li></ul>

## Versions

<table><tbody><tr><th>Version</th><th>Status</th><th>Introduced</th></tr>
<tr><td>1.0</td><td>Stable</td><td><a href="/kb/en/mariadb-10115-release-notes/">MariaDB 10.1.15</a></td></tr>
<tr><td>1.0</td><td>Beta</td><td><a href="/kb/en/mariadb-10111-release-notes/">MariaDB 10.1.11</a></td></tr>
</tbody></table>

## System Variables

### `gssapi_keytab_path`

- <strong>Description:</strong> Defines the path to the server's keytab file.
<ul start="1"><li>This system variable is only meaningful on <strong>Unix</strong>.
</li><li>See [Creating a Keytab File on Unix](#creating-a-keytab-file) and [Configuring the Path to the Keytab File on Unix](#configuring-the-path-to-the-keytab-file-on-unix) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--gssapi-keytab-path</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> ''
- <strong>Introduced:</strong> [MariaDB 10.1.11](/kb/en/mariadb-10111-release-notes/)

---

### `gssapi_principal_name`

- <strong>Description:</strong> Name of the service principal.
<ul start="1"><li>See [Configuring the Service Principal Name](#configuring-the-service-principal-name) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--gssapi-principal-name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `string`
- <strong>Default Value:</strong> ''
- <strong>Introduced:</strong> [MariaDB 10.1.11](/kb/en/mariadb-10111-release-notes/)

---

### `gssapi_mech_name`

- <strong>Description:</strong>  Name of the SSPI package used by server. Can be either 'Kerberos' or 'Negotiate'.  Set it to 'Kerberos', to prevent less secure NTLM in domain environments, but leave it as default (Negotiate) to allow non-domain environments (e.g if server does not run in a domain environment).
<ul start="1"><li>This system variable is only meaningful on <strong>Windows</strong>.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--gssapi-mech-name</code>
- <strong>Scope:</strong> Global
- <strong>Dynamic:</strong> No
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `Negotiate`
- <strong>Valid Values:</strong> `Kerberos`, `Negotiate`
- <strong>Introduced:</strong> [MariaDB 10.1.11](/kb/en/mariadb-10111-release-notes/)

---

## Options

### `gssapi`

- <strong>Description:</strong> Controls how the server should treat the plugin when the server starts up.
<ul start="1"><li>Valid values are:
<ul start="1"><li>`OFF` - Disables the plugin without removing it from the <a undefined>mysql.plugins</a> table.
</li><li>`ON` - Enables the plugin. If the plugin cannot be initialized, then the server will still continue starting up, but the plugin will be disabled.
</li><li>`FORCE` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error.
</li><li>`FORCE_PLUS_PERMANENT` - Enables the plugin. If the plugin cannot be initialized, then the server will fail to start with an error. In addition, the plugin cannot be uninstalled with [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname) or [UNINSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-plugin) while the server is running.
</li></ul>
</li><li>See [Plugin Overview: Configuring Plugin Activation at Server Startup](/kb/en/plugin-overview/#configuring-plugin-activation-at-server-startup) for more information.
</li></ul>
- <strong>Commandline:</strong> <code class="fixed" style="white-space:pre-wrap">--gssapi=value</code>
- <strong>Data Type:</strong> `enumerated`
- <strong>Default Value:</strong> `ON`
- <strong>Valid Values:</strong> `OFF`, `ON`, `FORCE`, `FORCE_PLUS_PERMANENT`
- <strong>Introduced:</strong> [MariaDB 10.1.11](/kb/en/mariadb-10111-release-notes/)

---