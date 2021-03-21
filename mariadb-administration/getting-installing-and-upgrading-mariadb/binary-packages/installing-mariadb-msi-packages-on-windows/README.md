# Installing MariaDB MSI Packages on Windows

MSI packages are available for both x86 (32 bit) and x64 (64 bit) processor
architectures. We'll use screenshots from an x64 installation below (the 32 bit
installer is very similar).

## Installation UI

This is the typical mode of installation. To start the installer, just click on
the mariadb-&lt;major&gt;.&lt;minor&gt;.&lt;patch&gt;.msi

### Welcome

<img src="/kb/en/installing-mariadb-msi-packages-on-windows/+image/WelcomeDialog" alt="Welcome dialog" title="Welcome dialog">

### License Agreement

<img src="/kb/en/installing-mariadb-msi-packages-on-windows/+image/LicenseAgreementDialog" alt="License Agreement" title="License Agreement">

Click on "I accept the terms"

### Custom Setup

<img src="/kb/en/installing-mariadb-msi-packages-on-windows/+image/CustomSetupDialog" alt="Custom Setup" title="Custom Setup">

Here, you can choose what features to install. By default, all features are
installed with the exception of the debug symbols. If the "Database instance"
feature is selected, the installer will create a database instance, by default
running as a service. In this case the installer will present additional
dialogs to control various database properties. Note that you do not
necessarily have to create an instance at this stage. For example, if you
already have MySQL or MariaDB databases running as services, you can just
upgrade them during the installation. Also, you can create additional database
instances after the installation, with the [mysql_install_db.exe](/mariadb-administration/getting-installing-and-upgrading-mariadb/mysql_install_dbexe/) utility.

<strong>NOTE</strong>: By default, if you install a database instance, the data directory
will be in the "data" folder under the installation root. To change the data
directory location, select "Database instance" in the feature tree, and use the
"Browse" button to point to another place.

### Database Authentication/Security Related Properties

<img src="/kb/en/installing-mariadb-msi-packages-on-windows/+image/DatabaseProperties_1" alt="Database security properties" title="Database security properties">

This dialog is shown if you selected the "Database instance" feature. Here, you
can set the password for the "root" database user and specify whether root can
access database from remote machines. The "Create anonymous account" setting
allows for anonymous (non-authenticated) users. It is off by default and it is
not recommended to change this setting.

### Other Database Properties

<img src="/kb/en/installing-mariadb-msi-packages-on-windows/+image/DatabaseProperties_2" alt="Other database properties" title="Other database properties">

- Install as service

- Defines whether the database should be run as a service. If it should be run as a service, then it also defines the service name. It is recommended to run your database instance as a service as it greatly
simplifies database management. In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, the default service name used by the MSI installer is "MariaDB". In 10.3 and before, the default service name used by the MSI installer is "MySQL". Note that the default service name for the <a undefined>--install</a> and <a undefined>--install-manual</a> options for `mysqld.exe` is "MySQL" in all versions of MariaDB.

- Enable Networking

- Whether to enable TCP/IP (recommended) and which port MariaDB should
listen to. If security is a concern, you can change the [bind-address](/kb/en/server-system-variables/#bind_address)
parameter post-installation to bind to only local addresses. If the "Enable
networking" checkbox is deselected, the database will use named pipes for
communication.

- Optimize for Transactions

- If this checkbox is selected, the default storage engine is set to Innodb
(or XtraDB) and the `sql_mode` parameter is set to
"`NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES`". You can also define the
Innodb/Xtradb buffer pool size. The default buffer pool size is 12.5% of RAM
and depending on your requirements you can give innodb more (up to 70-80% RAM).
32 bit versions of MariaDB have restrictions on maximum buffer pool size, which
is approximately 1GB, due to virtual address space limitations for 32bit
processes.

### Ready to Install

<img src="/kb/en/installing-mariadb-msi-packages-on-windows/+image/ReadyDialog" alt="Ready Dialog" title="Ready Dialog">

At this point, all installation settings are collected. Click on the "Install"
button.

### User Account Control (UAC) popup

<img src="/kb/en/installing-mariadb-msi-packages-on-windows/+image/UACPopup" alt="UAC popup" title="UAC popup">

If user account control is enabled (Vista or later), you will see this dialog.
Click on "Yes".

### End

<img src="/kb/en/installing-mariadb-msi-packages-on-windows/+image/FinishDialog" alt="Finish" title="Finish">

Installation is finished now. If you have upgradable instances of
MariaDB/MySQL, running as services, this dialog will present a "Do you want to
upgrade existing instances" checkbox (if selected, it launches the Upgrade
Wizard post-installation).

If you installed a database instance as service, the service will be running
already.

## New Entries in Start Menu

Installation will add some entries in the Start Menu:

<img src="/kb/en/installing-mariadb-msi-packages-on-windows/+image/StartMenu" alt="Start Menu" title="Start Menu">

- MySQL Client - Starts  command line client mysql.exe

- Command Prompt - Starts a command prompt. Environment is set such that "bin"
  directory of the installation is included into PATH environment variable, i.e
  you can use this command prompt to issue MariaDB commands (mysqldadmin, mysql
  etc...)

- Database directory - Opens the data directory in Explorer.

- Error log - Opens the database error log in Notepad.

- my.ini - Opens the database configuration file my.ini in Notepad.

- Upgrade Wizard - Starts the Wizard to upgrade an existing MariaDB/MySQL
  database instance to this MariaDB version.

## Uninstall UI

In the Explorer applet "Programs and Features" (or "Add/Remove programs" on
older Windows), find the entry for MariaDB, choose Uninstall/Change and click
on the "Remove" button in the dialog below.

<img src="/kb/en/installing-mariadb-msi-packages-on-windows/+image/UninstallChangeDialog" alt="UninstallChangeDialog" title="UninstallChangeDialog">

If you installed a database instance, you will need to decide if you want to
remove or keep the data in the database directory.

<img src="/kb/en/installing-mariadb-msi-packages-on-windows/+image/KeepOrRemoveDataDialog" alt="KeepOrRemoveDataDialog" title="KeepOrRemoveDataDialog">

## Silent Installation

The MSI installer supports silent installations as well. In its simplest form
silent installation with all defaults can be performed from an elevated command
prompt like this:

```sql
  msiexec /i path-to-package.msi /qn
```

<strong>Note:</strong> the installation is silent due to msiexe.exe's /qn switch (no user
interface), if you omit the switch, the installation will have the full UI.

### Properties

Silent installations also support installation properties (a property would
correspond for example to checked/unchecked state of a checkbox in the UI, user
password, etc). With properties the command line to install the MSI package
would look like this:

```sql
msiexec /i path-to-package.msi [PROPERTY_1=VALUE_1 ... PROPERTY_N=VALUE_N] /qn
```

The MSI installer package requires property names to be all capitals and contain
only English letters. By convention, for a boolean property, an empty value
means "false" and a non-empty is "true".

MariaDB installation supports the following properties:

<table><tbody><tr><th>Property name</th><th>Default value</th><th>Description</th></tr>
<tr><td>INSTALLDIR</td><td>%ProgramFiles%\MariaDB &lt;version&gt;\</td><td>Installation root</td></tr>
<tr><td>PORT</td><td>3306</td><td>--port parameter for the server</td></tr>
<tr><td>ALLOWREMOTEROOTACCESS</td><td></td><td>Allow remote access for root user</td></tr>
<tr><td>BUFFERPOOLSIZE</td><td>RAM/8</td><td>Bufferpoolsize for innodb</td></tr>
<tr><td>CLEANUPDATA</td><td>1</td><td>Remove the data directory (uninstall only)</td></tr>
<tr><td>DATADIR</td><td>INSTALLDIR\data</td><td>Location of the data directory</td></tr>
<tr><td>DEFAULTUSER</td><td></td><td>Allow anonymous users</td></tr>
<tr><td>PASSWORD</td><td></td><td>Password of the root user</td></tr>
<tr><td>SERVICENAME</td><td></td><td>Name of the Windows service. A service is not created if this value is empty.</td></tr>
<tr><td>SKIPNETWORKING</td><td></td><td>Skip networking</td></tr>
<tr><td>STDCONFIG</td><td>1</td><td>Corresponds to "optimize for transactions" in the GUI, default engine innodb, strict sql mode</td></tr>
<tr><td>UTF8</td><td></td><td>if set, adds character-set-server=utf8 to my.ini file (added in <a href="/kb/en/mariadb-5529-release-notes/">MariaDB 5.5.29</a>)</td></tr>
<tr><td>PAGESIZE</td><td>16K</td><td>page size for innodb</td></tr>
</tbody></table>

### Features

<em>Feature</em> is a Windows installer term for a unit of installation. Features
can be selected and deselected in the UI in the feature tree in the "Custom
Setup" dialog.

Silent installation supports adding features with the special property
`ADDLOCAL=Feature_1,..,Feature_N` and removing features with
`REMOVE=Feature_1,..., Feature_N`

Features in the MariaDB installer:

<table><tbody><tr><th>Feature id</th><th>Installed by default?</th><th>Description</th></tr>
<tr><td>DBInstance</td><td>yes</td><td>Install database instance</td></tr>
<tr><td>Client</td><td>yes</td><td>Command line client programs</td></tr>
<tr><td>MYSQLSERVER</td><td>yes</td><td>Install server</td></tr>
<tr><td>SharedLibraries</td><td>yes</td><td>Install client shared library</td></tr>
<tr><td>DEVEL</td><td>yes</td><td>install C/C++ header files and client libraries</td></tr>
<tr><td>HeidiSQL</td><td>yes</td><td>Installs HeidiSQL</td></tr>
<tr><td>DEBUGSYMBOLS</td><td>no</td><td>install debug symbols</td></tr>
</tbody></table>

### Silent Installation Examples

All examples here require running as administrator (and elevated command line
in Vista and later)

- Install default features, database instance as service, non-default datadir
  and port <pre class="fixed">msiexec /i path-to-package.msi SERVICENAME=MySQL DATADIR=C:\mariadb5.2\data PORT=3307 /qn </pre>

- Install service, add debug symbols, do not add development components (client
  libraries and headers) <pre class="fixed">msiexec /i path-to-package.msi SERVICENAME=MySQL ADDLOCAL=DEBUGSYMBOLS REMOVE=DEVEL /qn</pre>

### Silent Uninstall

To uninstall silently, use the `REMOVE=ALL` property with msiexec:

```sql
msiexec /i path-to-package.msi REMOVE=ALL /qn```

To keep the data directory during an uninstall, you will need to pass an
additional parameter:

```sql
msiexec /i path-to-package.msi REMOVE=ALL CLEANUPDATA="" /qn```

## Installation Logs

If you encounter a bug in the installer, the installer logs should be used for
diagnosis. Please attach verbose logs to the bug reports you create. To create a verbose
installer log, start the installer from the command line with the `/l*v`
switch, like so:

```sql
  msiexec.exe /i path-to-package.msi  /l*v path-to-logfile.txt
```

## Running 32 and 64 Bit Distributions on the Same Machine

It is possible to install 32 and 64 bit packages on the same Windows x64.

Apart from testing, an example where this feature can be useful is a
development scenario, where users want to run a 64 bit server and develop both
32 and 64 bit client components. In this case the full 64 bit package can be
installed, including a database instance plus development-related features
(headers and libraries) from the 32 bit package.