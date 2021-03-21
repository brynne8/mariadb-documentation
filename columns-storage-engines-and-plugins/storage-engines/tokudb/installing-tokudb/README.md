# Installing TokuDB

TokuDB has been deprecated by its upstream maintainer. It is disabled from [MariaDB 10.5](/kb/en/what-is-mariadb-105/) and has been been removed in [MariaDB 10.6](/kb/en/what-is-mariadb-106/) - [MDEV-19780](https://jira.mariadb.org/browse/MDEV-19780). We recommend [MyRocks](/columns-storage-engines-and-plugins/storage-engines/myrocks) as a long-term migration path.

Note that ha_tokudb is not included in binaries built with the "old" glibc. Binaries built with glibc 2.14+ do include it.

TokuDB is available on the following distributions:

<table><tbody><tr><th>Distribution</th><th>Introduced</th></tr>
<tr><td>CentOS 6 64-bit and newer</td><td><a href="/kb/en/mariadb-5536-release-notes/">MariaDB 5.5.36</a> and <a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td>Debian 7 "wheezy"64-bit and newer</td><td><a href="/kb/en/mariadb-5533-release-notes/">MariaDB 5.5.33</a> and <a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td>Fedora 19 64-bit and newer</td><td><a href="/kb/en/mariadb-5533-release-notes/">MariaDB 5.5.33</a> and <a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
<tr><td>openSUSE 13.1 64-bit and newer</td><td><a href="/kb/en/mariadb-5541-release-notes/">MariaDB 5.5.41</a> and <a href="/kb/en/mariadb-10015-release-notes/">MariaDB 10.0.15</a></td></tr>
<tr><td>Red Hat 6 64-bit and newer</td><td><a href="/kb/en/mariadb-5536-release-notes/">MariaDB 5.5.36</a> and <a href="/kb/en/mariadb-1009-release-notes/">MariaDB 10.0.9</a></td></tr>
<tr><td>Ubuntu 12.10 "quantal" 64-bit and newer</td><td><a href="/kb/en/mariadb-5533-release-notes/">MariaDB 5.5.33</a> and <a href="/kb/en/mariadb-1005-release-notes/">MariaDB 10.0.5</a></td></tr>
</tbody></table>

<strong>Note:</strong> The TokuDB version that comes from MariaDB.org differs slightly from the
TokuDB version from [Tokutek](http://www.tokutek.com). Please
read the [TokuDB Differences](/columns-storage-engines-and-plugins/storage-engines/tokudb/tokudb-differences) article before using TokuDB!

The following sections detail how to install and enable TokuDB.

## Installing TokuDB

Until MariaDB versions 5.5.39 and 10.0.13, before upgrading TokuDB, the server needed to be cleanly shut down. If the server was not cleanly shut down, TokuDB would fail to start. Since 5.5.40 and 10.0.14, this has no longer been necessary. See [MDEV-6173](https://jira.mariadb.org/browse/MDEV-6173).

TokuDB has been included with MariaDB since [MariaDB 5.5.34](/kb/en/mariadb-5534-release-notes/) and [MariaDB 10.0.6](/kb/en/mariadb-1006-release-notes/) and does not require separate installation. Proceed straight to [Check for Transparent HugePage Support on Linux](#check-for-transparent-hugepage-support-on-linux). For older versions, see the distro-specific instructions below.

### Installing TokuDB on Fedora, RedHat, &amp; CentOS

In [MariaDB 5.5.33](/kb/en/mariadb-5533-release-notes/), [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/), and starting from [MariaDB 10.2.5](/kb/en/mariadb-1025-release-notes/) TokuDB is in a separate RPM package
called `MariaDB-tokudb-engine` and is installed as follows:

```sql
sudo yum install MariaDB-tokudb-engine
```

### Installing TokuDB on Ubuntu &amp; Debian

On Ubuntu, TokuDB is available on the 64-bit versions of Ubuntu 12.10 and
newer. On Debian, TokuDB is available on the 64-bit versions of Debian 7
"Wheezy" and newer.

The package is installed as follows:

```sql
sudo apt-get install mariadb-plugin-tokudb
```

In some earlier versions, from [MariaDB 5.5.33](/kb/en/mariadb-5533-release-notes/) and [MariaDB 10.0.5](/kb/en/mariadb-1005-release-notes/), TokuDB is in a separate package called
`mariadb-tokudb-engine-x.x`, where `x.x` is the MariaDB series (`5.5` or
`10.0`). The package is installed, for example on `5.5`, as follows:

```sql
sudo apt-get install mariadb-tokudb-engine-5.5
```

## libjemalloc

TokuDB requires the libjemalloc library (currently version 3.3.0 or greater).

libjemalloc should automatically be installed when using a package manager, and is loaded by restarting MariaDB.

It can be enabled, if not already done, by adding the following to the my.cnf configuration file:

```sql
[mysqld_safe]

malloc-lib= /path/to/jemalloc
```

If you don't do the above, you will get an error similar to the following one in your error file

```sql
2018-11-19 18:46:26 0 [ERROR] mysqld: Can't open shared library '/home/my/maria-10.3/mysql-test/var/plugins/ha_tokudb.so' (errno: 2, /usr/lib64/libjemalloc.so.2: cannot allocate memory in static TLS block)
```

## Check for Transparent HugePage Support on Linux

Transparent hugepages is a feature in newer linux kernel versions that causes
problems for the memory usage tracking calculations in TokuKV and can lead to
memory overcommit. If you have this feature enabled, TokuKV will not start, and
you should turn it off.

You can check the status of Transparent Hugepages as follows:

```sql
cat /sys/kernel/mm/transparent_hugepage/enabled
```

If the path does not exist, Transparent Hugepages are not enabled and you may continue.

Alternatively, the following will be returned:

```sql
always madvise [never]
```

indicating Transparent Hugepages are not enabled and you may continue. If the following is returned:

```sql
[always] madvise never
```

Transparent Hugepages are enabled, and you will need to disable them.

To disable them, pass "transparent_hugepage=never" to the kernel in your bootloader (grub, lilo, etc.). For example, for SUSE, add `transparent_hugepage=never` to Optional Kernel Command Line Parameter at the end, such as after "showopts", and press OK. The setting will take effect on the next reboot.

You can also disable with:

```sql
echo never > /sys/kernel/mm/transparent_hugepage/enabled
echo never > /sys/kernel/mm/transparent_hugepage/defrag
```

On Centos or RedHat you can do:

Add line <code class="fixed" style="white-space:pre-wrap">GRUB_CMDLINE_LINUX_DEFAULT="transparent_hugepage=never"</code> to file /etc/default/grub

Update grub (boot loader):

```sql
grub2-mkconfig -o /boot/grub2/grub.cfg "$@"
```

For more information, see [http://unix.stackexchange.com/questions/99154/disable-transparent-hugepages](http://unix.stackexchange.com/questions/99154/disable-transparent-hugepages)

## Enabling TokuDB

Attempting to enable TokuDB while Linux Transparent HugePages are enabled will fail with an error such as:

```sql
ERROR 1123 (HY000): Can't initialize function 'TokuDB'; Plugin initialization function failed
```

See the section above; [Check for Transparent HugePage Support on Linux](#check-for-transparent-hugepage-support-on-linux).

The [binary log also needs to be enabled](/mariadb-administration/server-monitoring-logs/binary-log/activating-the-binary-log) before attempting to enable TokuDB. Strictly speaking, the XA code requires two XA-capable storage engines, and this is checked at startup. In practice, this requires InnoDB and the binary log to be active. If it isn't, the following warning will be returned and XA features will be disabled:

```sql
Cannot enable tc-log at run-time. XA features of TokuDB are disabled
```

MariaDB's default <code class="highlight fixed" style="white-space:pre-wrap">my.cnf</code> files come with a section for
TokuDB. To enable TokuDB just remove the '#' comment markers from the options
in the TokuDB section.

A typical TokuDB section looks like the following:

```sql
# See https://mariadb.com/kb/en/how-to-enable-tokudb-in-mariadb/
# for instructions how to enable TokuDB
#
# See https://mariadb.com/kb/en/tokudb-differences/ for differences
# between TokuDB in MariaDB and TokuDB from http://www.tokutek.com/

plugin-load=ha_tokudb
```

By default, the `plugin-load` option is commented out. Simply un-comment it
as in the example above.

Don't forget to also enable jemalloc in the config file.

```sql
[mysqld_safe]
malloc-lib= /path/to/jemalloc
```

With these changes done, you can restart MariaDB to activate TokuDB.

### Enabling TokuDB on Fedora

Instead of putting the TokuDB section in the main `my.cnf` file, it is
placed in a separate file located at: `/etc/my.cnf.d/tokudb.cnf`

### Enabling TokuDB on Ubuntu &amp; Debian

Instead of putting the TokuDB section in the main `my.cnf` file, it is
placed in a separate file located at: `/etc/mysql/conf.d/tokudb.cnf`

### Enabling TokuDB Manually From the mysql Command Line

Generally, it is recommended to use one of the above methods to enable the
TokuDB storage engine, but it is also possible to enable it manually as with
other plugins. To do so, launch the mysql command-line client and connect to
MariaDB as a user with the `SUPER` privilege and execute the following
command:

```sql
INSTALL SONAME 'ha_tokudb';
```

TokuDB will be installed until someone executes [UNINSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/uninstall-soname).

### Temporarily Enabling TokuDB When Starting MariaDB

If you just want to test TokuDB, you can start the `mysqld` server with TokuDB with the following command:

```sql
mysqld --plugin-load=ha_tokudb --plugin-dir=/usr/local/mysql/lib/mysql/plugin
```

## See Also

- [Differences between TokuDB from Tokutek.com and the TokuDB version in MariaDB from MariaDB.org](/columns-storage-engines-and-plugins/storage-engines/tokudb/tokudb-differences).
- [TokuDB System and Status Variables](/kb/en/tokudb-system-and-status-variables/)