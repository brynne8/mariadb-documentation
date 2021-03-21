# Installing MariaDB Server on macOS Using Homebrew

MariaDB Server is available for installation on macOS (formerly Mac OS X) via the [Homebrew](http://brew.sh/) package manager.

MariaDB Server is available as a Homebrew "bottle", a pre-compiled package. This means you can install it without having to build from source yourself. This saves time.

After installing Homebrew, MariaDB Server can be installed with this command:

```sql
brew install mariadb
```

After installation, start MariaDB Server:

```sql
mysql.server start
```

To auto-start MariaDB Server, use Homebrew's services functionality, which configures auto-start with the launchctl utility from [launchd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/launchd):

```sql
brew services start mariadb
```

After MariaDB Server is started, you can log in as your user:

```sql
mysql
```

Or log in as root:

```sql
sudo mysql -u root
```

## Upgrading MariaDB

First you may need to update your brew installation:

```sql
brew update
```

Then, to upgrade MariaDB Server:

```sql
brew upgrade mariadb
```

## Building MariaDB Server from source

In addition to the "bottled" MariaDB Server package available from Homebrew, you can use Homebrew to build MariaDB from source. This is useful if you want to use a different version of the server or enable some different capabilities that are not included in the bottle package.

Two components not included in the bottle package (as of MariaDB Server 10.1.19) are the CONNECT and OQGRAPH engines, because they have non-standard dependencies. To build MariaDB Server with these engines, you must first install `boost` and `judy`. As of December 2016, judy is in the Homebrew "boneyard", but the old formula still works on macOS Sierra. Follow these steps to install the dependencies and build the server:

```sql
brew install boost homebrew/boneyard/judy
brew install mariadb --build-from-source
```

You can also use Homebrew to build and install a pre-release version of MariaDB Server (for example MariaDB Server 10.2, when the highest GA version is MariaDB Server 10.1). Use this command to build and install a "development" version of MariaDB Server:

```sql
brew install mariadb --devel
```

## Other resources

- [mariadb.rb on github](https://github.com/mxcl/homebrew/commit/debd033ad7bcd73df68d7370d7f2386e60fd24a0#Library/Formula/mariadb.rb)
- [Terin Stock (terinjokes)](https://github.com/terinjokes) who is the packager for Homebrew