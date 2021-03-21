# Installing MariaDB Binary Tarballs

MariaDB Binary tarballs are named following the pattern: mariadb-VERSION-OS.tar.gz. Be sure to [download](http://downloads.mariadb.org) the correct version for your machine.

<strong>Note:</strong> Some binary tarballs are marked <em>'(GLIBC_2.14)'</em> or <em>'(requires GLIBC_2.14+)'</em>. These binaries are built the same as the others, but on a newer build host, and they require GLIBC 2.14 or higher. Use the other binaries for machines with older versions of GLIBC installed. Run `ldd --version` to see which version is running on your distribution.

Others are marked <em>'systemd'</em>, which are for systems with `systemd` and GLIBC 2.19 or higher.

To install the [binaries](http://downloads.mariadb.org),
unpack the distribution into the directory of your choice and run the <code class="highlight fixed" style="white-space:pre-wrap">[mysql_install_db](/mariadb-administration/getting-installing-and-upgrading-mariadb/installing-system-tables-mysql_install_db/)</code> script.

In the example below we install MariaDB in the <code class="highlight fixed" style="white-space:pre-wrap">/usr/local/mysql</code> directory (this is the default location for MariaDB for many platforms). However any other directory should work too.

We install the binary with a symlink to the original name. This is done so that you can easily change MariaDB versions just by moving the symlink to point to another directory.

<strong>NOTE:</strong> For [MariaDB 5.1.32](/kb/en/mariadb-5132-release-notes/) <strong><em>only</em></strong> the line
"<code class="fixed" style="white-space:pre-wrap">./scripts/mysql_install_db --user=mysql</code>" should be changed to
"<code class="fixed" style="white-space:pre-wrap">./bin/mysql_install_db --user=mysql</code>"

### Ensure You Use the Correct my.cnf Files

MariaDB searches for the configuration files '`/etc/my.cnf`' (on some
systems '`/etc/mysql/my.cnf`') and '`~/.my.cnf`'. If you have an
old `my.cnf` file (maybe from a system installation of MariaDB or MySQL) you
need to take care that you don't accidentally use the old one with your new
binary .tar installation.

The normal solution for this is to ignore the `my.cnf` file in `/etc` when
you use the programs in the tar file.

This is done by [creating your own .my.cnf file](/kb/en/mysqld-startup-options/) in
your home directory and telling <code class="fixed" style="white-space:pre-wrap">mysql_install_db</code>,
[mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe/) and possibly [mysql (the
command-line client utility)](/clients-utilities/mysql-client/) to <strong>only</strong> use this one with the option
'<code class="fixed" style="white-space:pre-wrap">--defaults-file=~/.my.cnf</code>'. Note that
this has to be first option for the above commands!

### Installing MariaDB as root in /usr/local/mysql

If you have root access to the system, you probably want to install MariaDB under the user and group 'mysql' (to keep compatibility with MySQL installations):

```sql
groupadd mysql
useradd -g mysql mysql
cd /usr/local
tar -zxvpf /path-to/mariadb-VERSION-OS.tar.gz
ln -s mariadb-VERSION-OS mysql
cd mysql
./scripts/mysql_install_db --user=mysql
chown -R root .
chown -R mysql data
```

The symlinking with <code class="fixed" style="white-space:pre-wrap">ln -s</code> is recommended as it makes it easy to install many MariaDB version at the same time (for easy testing, upgrading, downgrading etc).

If you are installing MariaDB to replace MySQL, then you can leave out the call to <code class="fixed" style="white-space:pre-wrap">mysql_install_db</code>. Instead shut down MySQL. MariaDB should find the path to the data directory from your old <code class="fixed" style="white-space:pre-wrap">/etc/my.cnf</code> file (path may vary depending on your system).

To start mysqld you should now do:

```sql
./bin/mysqld_safe --user=mysql &
or
./bin/mysqld_safe --defaults-file=~/.my.cnf --user=mysql &
```

To test connection, modify your $PATH so you can invoke client such as [mysql](/clients-utilities/mysql-client/), [mysqldump](/clients-utilities/backup-restore-and-import-clients/mysqldump/), etc.

```sql
export PATH=$PATH:/usr/local/mysql/bin/
```

You may want to modify your .bashrc or .bash_profile to make it permanent.

### Installing MariaDB as Not root in Any Directory

Below, change /usr/local to the directory of your choice.

```sql
cd /usr/local
gunzip < /path-to/mariadb-VERSION-OS.tar.gz | tar xf -
ln -s mariadb-VERSION-OS mysql
cd mysql
./scripts/mysql_install_db --defaults-file=~/.my.cnf
```

If you have problems with the above gunzip command line, you can instead, if you have gnu tar, do:

```sql
tar xfz /path-to/mariadb-VERSION-OS.tar.gz
```

To start mysqld you should now do:

```sql
./bin/mysqld_safe --defaults-file=~/.my.cnf &
```

### Auto Start of mysqld

You can get mysqld (the MariaDB server) to autostart by copying the file <code class="fixed" style="white-space:pre-wrap">mysql.server</code> file to the right place.

```sql
cp support-files/mysql.server /etc/init.d/mysql.server
```

The exact place depends on your system. The <code class="fixed" style="white-space:pre-wrap">mysql.server</code> file contains instructions of how to use and fine tune it.

For systemd installation the mariadb.service file will need to be copied from the support-files/systemd folder to the /usr/lib/systemd/system/ folder.

```sql
cp support-files/systemd/mariadb.service /usr/lib/systemd/system/mariadb.service
```

Please refer to the [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) page for further information.

### Post Installation

After this, remember to set proper passwords for all accounts accessible from
untrusted sources, to avoid exposing the host to security risks! Also consider
using the [mysql.server](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqlserver/) to
[start MariaDB automatically](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/starting-and-stopping-mariadb-automatically/)
when your system boots.

Our MariaDB binaries are similar to the Generic binaries available for the
MySQL binary distribution. So for more options on using these binaries, the
MySQL 5.5 manual entry on
[installing generic binaries](http://docs.oracle.com/cd/E17952_01/refman-5.5-en/binary-installation.html) can be consulted.

For details on the exact steps used to build the binaries, see the
[compiling MariaDB section](/mariadb-administration/getting-installing-and-upgrading-mariadb/compiling-mariadb-from-source/) of the KB.