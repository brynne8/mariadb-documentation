# Switching Between Different Installed MariaDB Versions

This article is about managing many different installed MariaDB versions
and running them one at a time. This is useful when doing benchmarking,
testing, or for when developing different MariaDB versions.

This is most easily done using the tar files from
[downloads.askmonty.org](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/).

## Stopping a pre-installed MySQL/MariaDB from interfering with your tests

If MySQL/MariaDB is already installed and running, you have two options:

1 Use test MariaDB servers with a different port &amp; socket.
<ul start="1"><li>In this case you are probably best off creating a specific section for
   MariaDB in your <code class="highlight fixed" style="white-space:pre-wrap">~/.my.cnf</code> file.
</li></ul>
2 Stop mysqld with <code class="highlight fixed" style="white-space:pre-wrap">/etc/rc.d/mysql stop</code>
  or <code class="highlight fixed" style="white-space:pre-wrap">mysqladmin shutdown</code>.

Note that you don't have to uninstall or otherwise remove MySQL!

## How to create a binary distribution (tar file)

Here is a short description of how to generate a tar file from a source
distribution. If you have
[downloaded](/replication/optimization-and-tuning/query-optimizations/guiduuid-performance/mariadb/) a binary tar file, you
can skip this section.

The steps to create a binary tar file are:

- Decide where to put the source. A good place is under <code class="highlight fixed" style="white-space:pre-wrap">/usr/local/src/mariadb-5.#</code>.
- [Get the source](/kb/en/source-getting-the-mariadb-source-code/)
- [Compile the source](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/)
- [Create the binary tar ball](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/creating-the-mariadb-binary-tarball/).

You will then be left with a tar file named something like:
<code class="highlight fixed" style="white-space:pre-wrap">mariadb-5.3.2-MariaDB-beta-linux-x86_64.tar.gz</code>

## Creating a directory structure for the different installations

Install the binary tar files under <code class="highlight fixed" style="white-space:pre-wrap">/usr/local/</code> with
the following directory names (one for each MariaDB version you want to use):

- <code class="highlight fixed" style="white-space:pre-wrap">mariadb-5.1</code>
- <code class="highlight fixed" style="white-space:pre-wrap">mariadb-5.2</code>
- <code class="highlight fixed" style="white-space:pre-wrap">mariadb-5.3</code>
- <code class="highlight fixed" style="white-space:pre-wrap">mariadb-5.5</code>
- <code class="highlight fixed" style="white-space:pre-wrap">mariadb-10.0</code>

The above assumes you are just testing major versions of MariaDB. If you are
testing specific versions, use directory names like `mariadb-5.3.2`

With the directories in place, create a sym-link named `mariadb` which points
at the `mariadb-XXX` directory you are currently testing. When you want to
switch to testing a different version, just update the sym-link.

Example:

```sql
cd /usr/local
tar xfz /tmp/mariadb-5.3.2-MariaDB-beta-linux-x86_64.tar.gz
mv -vi mariadb-5.3.2-MariaDB-beta-linux-x86_64 mariadb-5.3
ln -vs mariadb-5.3 mariadb
```

## Setting up the data directory

When setting up the data directory, you have the option of either using a
shared database directory or creating a unique database directory for each
server version. For testing, a common directory is probably easiest. Note that
you can only have one <code class="highlight fixed" style="white-space:pre-wrap">mysqld</code> server running against one data
directory.

### Setting up a common data directory

The steps are:

1 Create the `mysql` system user if you don't have it already!
  (On Linux you do it with the `useradd` command).
2 Create the directory (we call it `mariadb-data` in the example below) or
  add a symlink to a directory which is in some other place.
3 Create the `mysql` permission tables with `mysql_install_db`

```sql
cd /usr/local/
mkdir mariadb-data
cd mariadb
./bin/mysql_install_db --no-defaults --datadir=/usr/local/mariadb-data
chown -R mysql mariadb-data mariadb-data/*
```

The reason to use <code class="highlight fixed" style="white-space:pre-wrap">--no-defaults</code> is to ensure that we don't
inherit incorrect options from some old my.cnf.

### Setting up different data directories

To create a different <code class="highlight fixed" style="white-space:pre-wrap">data</code> directories for each installation:

```sql
cd mariadb
./scripts/mysql_install_db --no-defaults
chown -R mysql mariadb-data mariadb-data/*
```

This will create a directory <code class="highlight fixed" style="white-space:pre-wrap">data</code> inside the
current directory.

If you want to use another disk you should do:

```sql
cd mariadb
ln -s path-to-empty-directory-for-data data
./scripts/mysql_install_db --no-defaults --datadir=./data
chown -R mysql mariadb-data mariadb-data/*
```

## Running a MariaDB server

The normal steps are:

```sql
rm mariadb
ln -s mariadb-5.# mariadb
cd mariadb
./bin/mysqld_safe --no-defaults --datadir=/usr/local/mariadb-data &
```

## Setting up a .my.cnf file for running multiple MariaDB main versions

If you are going to start/stop MariaDB a lot of times, you should create
a <code class="highlight fixed" style="white-space:pre-wrap">~/.my.cnf</code> file for the common options you are using.

The following example shows how to use a non-standard TCP-port and socket (to
not interfere with a main MySQL/MariaDB server) and how to setup different
options for each main server:

```sql
[client-server]
socket=/tmp/mysql.sock
port=3306
[mysqld]
datadir=/usr/local/mariadb-data

[mariadb-5.2]
# Options for MariaDB 5.2
[mariadb-5.3]
# Options for MariaDB 5.3
```

If you create an <code class="highlight fixed" style="white-space:pre-wrap">~/.my.cnf</code> file, you should start
<code class="highlight fixed" style="white-space:pre-wrap">mysqld</code> with <code class="highlight fixed" style="white-space:pre-wrap">--defaults-file=~/.my.cnf</code>
instead of <code class="highlight fixed" style="white-space:pre-wrap">--no-defaults</code> in the examples above.