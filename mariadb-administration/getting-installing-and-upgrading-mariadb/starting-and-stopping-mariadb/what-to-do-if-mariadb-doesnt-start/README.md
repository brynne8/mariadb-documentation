# What to Do if MariaDB Doesn't Start

There could be many reasons that MariaDB fails to start. This page will help troubleshoot some of the more common reasons and provide solutions.

If you have tried everything here, and still need help, you can ask for help on IRC or on the forums - see [Where to find other MariaDB users and developers](/kb/en/where-to-find-other-mariadb-users-and-developers/) - or ask a question at the [Starting and Stopping MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb) page.

## The Error Log and the Data Directory

The reason for the failure will almost certainly be written in the [error log](/mariadb-administration/server-monitoring-logs/error-log) and, if you are starting MariaDB manually, to the console. By default, the error log is named <em>host-name</em>.err and is written to the data directory.

Common Locations:

- /var/log/
- /var/log/mysql
- C:\Program Files\MariaDB x.y\data (x.y refers to the version number)
- C:\Program Files (x86)\MariaDB x.y\data (32bit  version on 64bit Windows)

It's also possible that the error log has been explicitly written to another location. This is often done by changing the <a undefined>datadir</a> or <a undefined>log_error</a> system variables in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). See [Option Files](#option-files) below for more information about that.

A quick way to get the values of these system variables is to execute the following commands:

```sql
mysqld --help --verbose | grep 'log-error' | tail -1
mysqld --help --verbose | grep 'datadir' | tail -1
```

## Option Files

Another kind of file to consider when troubleshooting is [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files). The default option file is called `my.cnf`. Option files contain configuration options, such as the location of the data directory mentioned above. If you're unsure where the option file is located, see [Configuring MariaDB with Option Files: Default Option File Locations](/kb/en/configuring-mariadb-with-option-files/#default-option-file-locations) for information on the default locations.

You can check which configuration options MariaDB server will use from its option files by executing the following command:

```sql
mysqld --print-defaults
```

You can also check by executing the following command:

```sql
my_print_defaults --mysqld
```

See [Configuring MariaDB with Option Files: Checking Program Options](/kb/en/configuring-mariadb-with-option-files/#checking-program-options) for more information on checking configuration options.

### Invalid Option or Option Value

Another potential reason for a startup failure is that an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) contains an invalid option or an invalid option value. In those cases, the [error log](/mariadb-administration/server-monitoring-logs/error-log) should contain an error similar to this:

```sql
140514 12:19:37 [ERROR] /usr/local/mysql/bin/mysqld: unknown variable 'option=value'
```

This is more likely to happen when you upgrade to a new version of MariaDB. In most cases the [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) from the old version of MariaDB will work just fine with the new version. However, occasionally, options are removed in new versions of MariaDB, or the valid values for options are changed in new versions of MariaDB. Therefore, it's possible for an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) to stop working after an upgrade.

Also remember that option names are case sensitive.

Examine the specifics of the error. Possible fixes are usually one of the following:

- If the option is completely invalid, then remove it from the [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files).
- If the option's name has changed, then fix the name.
- If the option's valid values have changed, then change the option's value to a valid one.
- If the problem is caused by a simple typo, then fix the typo.

## Can't Open Privilege Tables

It is possible to see errors similar to the following:

```sql
System error 1067 has occurred.
Fatal error: Can't open privilege tables: Table 'mysql.host' doesn't exist
```

If errors like this occur, then critical [system tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables) are either missing or are in the wrong location. The above error is quite common after an upgrade if the [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) set the <a undefined>basedir</a> or <a undefined>datadir</a> to a non-standard location, but the new server is using the default location. Therefore, make sure that the <a undefined>basedir</a> and <a undefined>datadir</a> variables are correctly set.

If you're unsure where the option file is located, see [Configuring MariaDB with Option Files: Default Option File Locations](/kb/en/configuring-mariadb-with-option-files/#default-option-file-locations) for information on the default locations.

If the [system tables](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables) really do not exist, then you may need to create them with [mysql_install_db](/clients-utilities/mysql_install_db). See [Installing System Tables (mysql_install_db)](/mariadb-administration/getting-installing-and-upgrading-mariadb/installing-system-tables-mysql_install_db) for more information.

## Can't Create Test File

One of the first tests on startup is to check whether MariaDB can write to the data directory. When this fails, it will log an error like this:

```sql
May 13 10:24:28 mariadb3 mysqld[19221]: 2019-05-13 10:24:28 0 [Warning] Can't create test file /usr/local/data/mariadb/mariadb3.lower-test
May 13 10:24:28 mariadb3 mysqld[19221]: 2019-05-13 10:24:28 0 [ERROR] Aborting
```

This is usually a permission error on the directory in which this file is being written. Ensure that the entire <a undefined>datadir</a> is owned by the user running `mysqld`, usually `mysql`. Ensure that directories have the "x" (execute) directory permissions for the owner. Ensure that all the parent directories of the <a undefined>datadir</a> upwards have "x" (execute) permissions for all (`user`, `group`, and `other`).

Once this is checked look at the [systemd](#systemd) and [selinux](#selinux) documentation below, or [apparmor](https://blogs.oracle.com/jsmyth/apparmor-and-mysql).

## InnoDB

[InnoDB](/kb/en/xtradb-and-innodb/) is probably the MariaDB component that most frequently causes a crash. In the error log, lines containing InnoDB messages generally start with "InnoDB:".

### Cannot Allocate Memory for the InnoDB Buffer Pool

In a typical installation on a dedicated server, at least 70% of your memory should be assigned to [InnoDB buffer pool](/kb/en/xtradbinnodb-buffer-pool/); sometimes it can even reach 85%. But be very careful: don't assign to the buffer pool more memory than it can allocate. If it cannot allocate memory, InnoDB will use the disk's swap area, which is very bad for performance. If swapping is disabled or the swap area is not big enough, InnoDB will crash. In this case, MariaDB will probably try to restart several times, and each time it will log a message like this:

```sql
140124 17:29:01 InnoDB: Fatal error: cannot allocate memory for the buffer pool
```

In that case, you will need to add more memory to your server/VM or decrease the value of the [innodb_buffer_pool_size](/kb/en/xtradbinnodb-server-system-variables/#innodb_buffer_pool_size) variables.

Remember that the buffer pool will slightly exceed that limit. Also, remember that MariaDB also needs allocate memory for other storage engines and several per-connection buffers. The operating system also needs memory.

### InnoDB Table Corruption

By default, InnoDB deliberately crashes the server when it detects table corruption. The reason for this behavior is preventing corruption propagation. However, in some situations, server availability is more important than data integrity. For this reason, we can avoid these crashes by changing the value of [innodb_corrupt_table_action](/kb/en/xtradbinnodb-server-system-variables/#innodb_corrupt_table_action) to 'warn'.

If InnoDB crashes the server after detecting data corruption, it writes a detailed message in the error log. The first lines are similar to the following:

```sql
InnoDB: Database page corruption on disk or a failed
InnoDB: file read of page 7.
InnoDB: You may have to recover from a backup.
```

Generally, it is still possible to recover most of the corrupted data. To do so, restart the server in [InnoDB recovery mode](/kb/en/xtradbinnodb-recovery-modes/) and try to extract the data that you want to backup. You can save them in a CSV file or in a non-InnoDB table. Then, restart the server in normal mode and restore the data.

## MyISAM

Most tables in the [mysql](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/the-mysql-database-tables) database are MyISAM tables. These tables are necessary for MariaDB to properly work, or even start.

A MariaDB crash could cause system tables corruption. With the default settings, MariaDB will simply not start if the system tables are corrupted. With [myisam_recover_options](/kb/en/myisam-system-variables/#myisam_recover_options), we can force MyISAM to repair damaged tables.

## systemd

If you are using [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd), then there are a few relevant notes about startup failures:

- If MariaDB is configured to access files under `/home`, `/root`, or `/run/user`, then the default systemd unit file will prevent access to these directories with a  `Permission Denied` error. This happens because the unit file set <a undefined>ProtectHome=true</a>. See [Systemd: Configuring Access to Home Directories](/kb/en/systemd/#configuring-access-to-home-directories) for information on how to work around this.

- The default systemd unit file also sets [ProtectSystem=full](https://www.freedesktop.org/software/systemd/man/systemd.exec.html#ProtectSystem=), which places restrictions on writing to a few other directories. Overwriting this with `ProtectSystem=off` in the same way as above will restore access to these directories.

- If MariaDB takes longer than 90 seconds to start, then the default systemd unit file will cause it to fail with an error. This happens because the default value for the <a undefined>TimeoutStartSec</a> option is 90 seconds. See [Systemd: Configuring the Systemd Service Timeout](/kb/en/systemd/#configuring-the-systemd-service-timeout) for information on how to work around this.

- The systemd journal may also contain useful information about startup failures. See [Systemd: Systemd Journal](/kb/en/systemd/#systemd-journal) for more information.

See [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd) documentation for further information on systemd configuration.

## SELinux

[Security-Enhanced Linux (SELinux)](https://selinuxproject.org/page/Main_Page) is a Linux kernel module that provides a framework for configuring [mandatory access control (MAC)](https://en.wikipedia.org/wiki/Mandatory_access_control) system for many resources on the system. It is enabled by default on some Linux distributions, including RHEL, CentOS, Fedora, and other similar Linux distribution. SELinux prevents programs from accessing files, directories or ports unless it is configured to access those resources.

You might need to troubleshoot SELinux-related issues in cases, such as:

- MariaDB is using a non-default port.
- MariaDB is reading from or writing to some files (datadir, log files, option files, etc.) located at non-default paths.
- MariaDB is using a plugin that requires access to resources that default installations do not use.

See [SELinux](/mariadb-administration/user-server-security/securing-mariadb/selinux) for more information.