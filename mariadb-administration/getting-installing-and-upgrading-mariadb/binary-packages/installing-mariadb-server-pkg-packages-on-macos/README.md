# Installing MariaDB Server PKG packages on macOS

##### MariaDB starting with [10.2.5](/kb/en/mariadb-1025-release-notes/)

Starting with [MariaDB 10.2.5](/kb/en/mariadb-1025-release-notes/), a .pkg installer for macOS is available from [http://mariadb.com/downloads](http://mariadb.com/downloads)

The .pkg installer installs MariaDB Server and, optionally, a launchd service definition to allow the OS to automatically start and manage MariaDB Server.

## Installer behavior

The installer installs MariaDB Server in a subdirectory of /usr/local/mariadb.

You can place /usr/local/mariadb/server/bin in your PATH to always have access to executables distributed in the MariaDB Server package. The basedir of the MariaDB Server installation will depend on the version installed; for 10.2.6, it will be /usr/local/mariadb/mariadb-10.2.6-osx10.12-x86_64, to which a symlink /usr/local/mariadb/server will be created.

The MariaDB Server binaries distributed in this package do not read from .cnf files in /etc/. Instead, put .cnf files in /usr/local/mariadb/etc/.

If there's an existing service listening on port 3306, the installer will create /usr/local/mariadb/etc/my.cnf with `skip-networking`. You can edit this file to set an alternate port for MariaDB Server to listen on, or you can remove `skip-networking` if you have disabled the daemon that had bound port 3306.

MariaDB Server will place its socket file at /usr/local/mariadb/data/mariadb.sock and the client tools included in the package will by default look for a socket file at that location.

After installation, your MariaDB Server instance will have a "root" user account and a user account named after the username of the user who ran the installer. Both of these accounts use the [UNIX_SOCKET Authentication Plugin](/kb/en/unix_socket-authentication-plugin/), which means no password is required to log in.

To log in to MariaDB as root, you will need to become root on the OS; the preferred mechanism for this is to use `sudo`, like this:

```sql
sudo /usr/local/mariadb/server/bin/mariadb
```

If you want to be able to connect to MariaDB Server using TCP, you will need to create a new user and give it a password.

The daemon's executable is `mariadbd`, not `mysqld`. You should take this into account if you have scripts, tools, or workflows that look for processes named "mysqld". You can also invoke `mysqld` to start the server; edit /Library/LaunchDaemons/com.mariadb.server.plist if you want to do that permanently.

## launchd behavior

The installer can install a launchd service configuration. This is optional, but it is enabled by default.

The service definition causes launchd to automatically start MariaDB Server after installation and when the OS boots.

The launchd service configuration file installed by the installer is located at /Library/LaunchDaemons/com.mariadb.server.plist. The service definition file uses command-line arguments to `mariadbd` to control some of its behavior. These will override changes you make in option (.cnf) files.

To stop MariaDB Server using launchd, execute `sudo launchctl stop com.mariadb.server`.

To start MariaDB Server using launchd, execute `sudo launchctl start com.mariadb.server`.

## Uninstalling MariaDB Server

To remove MariaDB Server, follow these steps:

First, stop MariaDB Server and remove the files associated with launchd support, if you installed it (you'll need to do this as root or under sudo):

- `launchctl stop com.mariadb.server`
- `launchctl unload /Library/LaunchDaemons/com.mariadb.server.plist`
- `rm  /Library/LaunchDaemons/com.mariadb.server.plist /usr/local/mariadb/server`
- `pkgutil --forget com.mariadb.mariadb-server-launchd`

Now, remove files associated with the MariaDB Server package:

- `rm -r /usr/local/mariadb/server/ /usr/local/mariadb/server`
- If you also want to remove config files, remove `/usr/local/mariadb/etc/`
- If you want to remove the MariaDB Server data directory, remove `/usr/local/mariadb/data/`

You can also consult the output of `pkgutil --files com.mariadb.mariadb-server-files` to see the specific list of files installed by the package.

After removing the files you wish to remove, tell pkgutil to forget the MariaDB Server package:

- `pkgutil --forget com.mariadb.mariadb-server-files`