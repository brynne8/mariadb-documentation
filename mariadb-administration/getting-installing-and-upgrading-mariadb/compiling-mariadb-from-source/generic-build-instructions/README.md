# Generic Build Instructions

The instructions on this page will help you compile [MariaDB](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/) from source.
Links to more complete instructions for specific platforms can be found on the
[source](/kb/en/source/) page.

First, [get a copy of the MariaDB source](/kb/en/getting-the-mariadb-source-code/).

Next, [prepare your system to be able to compile the source](/kb/en/build-environment-setup-for-linux/).

If you don't want to run MariaDB as yourself, then you should create a
`mysql` user. The example below uses this user.

## Using cmake

[MariaDB 5.5](/kb/en/what-is-mariadb-55/) and above is compiled using <em>cmake</em>.

It is recommended to create a build directory <strong>beside</strong> your source directory

```sql
mkdir build-mariadb
cd build-mariadb
```

<strong>NOTE</strong>
If you have built MariaDB in the past and have recently updated the repository, you should perform a complete cleanup of old artifacts (such as cmake configured files). In the base repository run:

```sql
git clean -xffd && git submodule foreach --recursive git clean -xffd
```

You can configure your build simply by running <em>cmake</em> without any special options, like

```sql
cmake ../server
```

where `server` is where you installed MariaDB. If you are building in the source directory, just omit `../server`.

If you want it to be configured exactly as a normal MariaDB server release is built, use

```sql
cmake ../server -DBUILD_CONFIG=mysql_release
```

This will configure the build to generate binary tarballs similar to release tarballs from downloads.mariadb.org. Unfortunately this doesn't work on old platforms, like OpenSuse Leap 15.0, because MariaDB binary tarballs are built to minimize external dependencies, and that needs static libraries that might not be provided by the platform by default, and would need to be installed manually.

To do a build suitable for debugging use:

```sql
cmake ../server -DCMAKE_BUILD_TYPE=Debug
```

All <em>cmake</em> configuration options for MariaDB can be displayed with:

```sql
cmake ../server -LH
```

To build and install MariaDB after running <em>cmake</em> use

```sql
make
sudo make install
```

If the commands above fail, you can enable more compilation information by doing:

```sql
make VERBOSE=1
```

If you want to generate a binary tarball, run

```sql
make package
```

## Using BUILD Scripts

There are also `BUILD` scripts for the most common systems for those that doesn't want to dig into cmake options. These are optimized for in source builds.

The scripts are of type 'compile-#cpu#-how_to_build'. Some common scripts-are

<table><tbody><tr><th>Script</th><th>Description</th></tr>
<tr><td>compile-pentium64</td><td>&nbsp;Compile an optimized binary optimized for 64 bit pentium (works also for amd64)</td></tr>
<tr><td>compile-pentium-debug</td><td>Compile a debug binary optimized for 64 bit pentium</td></tr>
<tr><td>compile-pentium-valgrind-max</td><td>&nbsp;Compile a debug binary that can be used with <a href="http://www.valgrind.org/">valgrind</a> to find wrong memory accesses and memory leaks. Should be used if one want's to run the <code>mysql-test-run</code> test suite with the <code>--valgrind</code> option</td></tr>
</tbody></table>

Some common suffixes used for the scripts:

<table><tbody><tr><th>Suffix</th><th>Description</th></tr>
<tr><td><code>32</code></td><td>&nbsp;Compile for 32 bit cpu's</td></tr>
<tr><td><code>64</code></td><td>Compile for 64 bit cpu's</td></tr>
<tr><td><code>-max</code></td><td>Enable (almost) all features and plugins that MariaDB supports</td></tr>
<tr><td><code>-gprof</code></td><td>binary is compiled with profiling (gcc --pg)</td></tr>
<tr><td><code>-gcov</code></td><td>binary is compiled with code coverage (gcc -fprofile-arcs -ftest-coverage)</td></tr>
<tr><td><code>-valgrind</code></td><td>The binary is compiled for debugging and optimized to be used with <a href="http://www.valgrind.org/">valgrind</a>.</td></tr>
<tr><td><code>-debug</code></td><td>&nbsp;The binary is compiled with all symbols (gcc -g) and the <code><a href="/kb/en/creating-a-trace-file/">DBUG</a></code> log system is enabled.</td></tr>
</tbody></table>

All `BUILD` scripts support the following options:

<table><tbody><tr><th>Suffix</th><th>Description</th></tr>
<tr><td>-h, --help</td><td>Show this help message.</td></tr>
<tr><td>-n, --just-print</td><td>Don't actually run any commands; just print them.</td></tr>
<tr><td>-c, --just-configure</td><td>Stop after running configure. Combined with --just-print shows configure options.</td></tr>
<tr><td>--extra-configs=xxx</td><td>Add this to configure options</td></tr>
<tr><td>--extra-flags=xxx</td><td>Add this C and CXX flags</td></tr>
<tr><td>--extra-cflags=xxx</td><td>Add this to C flags</td></tr>
<tr><td>--extra-cxxflags=xxx</td><td>Add this to CXX flags</td></tr>
<tr><td>--verbose</td><td>Print out full compile lines</td></tr>
<tr><td>--with-debug=full</td><td>Build with full debug(no optimizations, keep call stack).</td></tr>
</tbody></table>

A typical compilation used by a developer would be:

```sql
shell> ./BUILD/compile-pentium64-debug
```

This configures the source for debugging and runs make. The server binary will be `sql/mysqld`.

## Starting MariaDB for the First Time

After installing MariaDB (using `sudo make install`), but prior to starting MariaDB for the first time, one should:

1 ensure the directory where you installed MariaDB is owned by the mysql user (if the user doesn't exist, you'll need to create it)
2 run the `mysql_install_db` script to generate the needed system tables

Here is an example:

```sql
# The following assumes that the 'mysql' user exists and that we installed MariaDB
# in /usr/local/mysql
chown -R mysql /usr/local/mysql/
cd /usr/local/mysql/
scripts/mysql_install_db --user=mysql
/usr/local/mysql/bin/mysqld_safe --user=mysql &
```

## Testing MariaDB

If you want to test your compiled MariaDB, you can do either of:

```sql
make test
```

or

```sql
mysql-test/mysql-test-run --force
```

Each of the above are run from the build directory. There is no need to '`sudo make install`' MariaDB prior to running them.

<strong>NOTE:</strong> If you are doing more extensive testing or debugging of MariaDB (like with real application data and workloads) you may want to start and run MariaDB directly from the source directory instead of installing it with '`sudo make install`'. If so, see
[Running MariaDB from the Source Directory](/kb/en/running-mariadb-from-the-source-directory/).

## Increasing Version Number or Tagging a Version

If you have made code changes and want to increase the version number or tag our version with a specific tag you can do this by editing the `VERSION` file. Tags are shown when running the '`mysqld --version`' command.

## Non-ascii Symbols

MariaDB builds with `readline`; using an alternative such as `Editline` may result in problems with non-ascii symbols.

## Post-install Tasks

- [Installing system tables (mysql_install_db)](/mariadb-administration/getting-installing-and-upgrading-mariadb/installing-system-tables-mysql_install_db/)
- [Starting and Stopping MariaDB Automatically](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/starting-and-stopping-mariadb-automatically/)