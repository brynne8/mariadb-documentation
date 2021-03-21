# systemd

##### MariaDB starting with [10.1.8](/kb/en/mariadb-1018-release-notes/)

The MariaDB `systemd` service was first released in [MariaDB 10.1.8](/kb/en/mariadb-1018-release-notes/) on supported Linux distributions.

`systemd` is a [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit/) replacement that is the default service manager on the following Linux distributions:

- RHEL 7 and above
- CentOS 7 and above
- Fedora 15 and above
- Debian 8 and above
- Ubuntu 15.04 and above
- SLES 12 and above
- OpenSUSE 12.2 and above

MariaDB's `systemd` unit file is included in the server packages for [RPMs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/) and [DEBs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-deb-files/). It is also included in certain [binary tarballs](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/installing-mariadb-binary-tarballs/).

The service name is `mariadb.service`. Aliases to `mysql.service` and `mysqld.service` are also installed for convenience.

When MariaDB is started with the `systemd` unit file, it directly starts the [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) process as the `mysql` user. Unlike with [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit/), the [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) process is not started with [mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe/). As a consequence, options will not be read from the `[mysqld_safe]` [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/).

## Locating the MariaDB Service's Unit File

By default, the unit file for the service will be installed in a directory defined at build time by the `INSTALL_SYSTEMD_UNITDIR` option provided to <a undefined>cmake</a>.

For example, on RHEL, CentOS, Fedora, and other similar Linux distributions, `INSTALL_SYSTEMD_UNITDIR` is defined as `/usr/lib/systemd/system/`, so it will be installed to:

- `/usr/lib/systemd/system/mariadb.service`

And on Debian, Ubuntu, and other similar Linux distributions, `INSTALL_SYSTEMD_UNITDIR` is defined as `/lib/systemd/system/`, so it will be installed to:

- `/lib/systemd/system/mariadb.service`

## Interacting with the MariaDB Server Process

The service can be interacted with by using the <a undefined>systemctl</a> command.

### Starting the MariaDB Server Process on Boot

MariaDB's `systemd` service can be configured to start at boot by executing the following:

```sql
sudo systemctl enable mariadb
```

### Starting the MariaDB Server Process

MariaDB's `systemd` service can be started by executing the following:

```sql
sudo systemctl start mariadb
```

MariaDB's `systemd` unit file has a default startup timeout of about 90 seconds on most systems. If certain startup tasks, such as crash recovery, take longer than this default startup timeout, then `systemd` will assume that `mysqld` has failed to startup, which causes `systemd` to kill the `mysqld` process. To work around this, you can reconfigure the MariaDB `systemd` unit to have an [infinite timeout](#configuring-the-systemd-service-timeout).

Note that [systemd 236 added the `EXTEND_TIMEOUT_USEC` environment variable](https://lists.freedesktop.org/archives/systemd-devel/2017-December/039996.html) that allows services to extend the startup timeout during long-running processes. Starting with [MariaDB 10.1.33](/kb/en/mariadb-10133-release-notes/), [MariaDB 10.2.15](/kb/en/mariadb-10215-release-notes/), and [MariaDB 10.3.6](/kb/en/mariadb-1036-release-notes/), on systems with systemd versions that support it, MariaDB uses this feature to extend the startup timeout during certain startup processes that can run long. Therefore, if you are using `systemd` 236 or later, then you should not need to manually override `TimeoutStartSec`, even if your startup tasks, such as crash recovery, run for longer than the configured value. See [MDEV-14705](https://jira.mariadb.org/browse/MDEV-14705) for more information.

### Stopping the MariaDB Server Process

MariaDB's `systemd` service can be stopped by executing the following:

```sql
sudo systemctl stop mariadb
```

### Restarting the MariaDB Server Process

MariaDB's `systemd` service can be restarted by executing the following:

```sql
sudo systemctl restart mariadb
```

### Checking the Status of the MariaDB Server Process

The status of MariaDB's `systemd` service can be obtained by executing the following:

```sql
sudo systemctl status mariadb
```

### Interacting with Multiple MariaDB Server Processes

A `systemd` [template unit file](https://www.freedesktop.org/software/systemd/man/systemd.unit.html) with the name `mariadb@.service` is installed in `INSTALL_SYSTEMD_UNITDIR` on some systems. See [Locating the MariaDB Service's Unit File](#locating-the-mariadb-services-unit-file) to see what directory that refers to on each distribution.

This template unit file allows you to interact with multiple MariaDB instances on the same system using the same template unit file. When you interact with a MariaDB instance using this template unit file, you have to provide an instance name as a suffix. For example, the following command tries to start a MariaDB instance with the name `node1`:

```sql
sudo systemctl start mariadb@node1
```

MariaDB's build system cannot include the `mariadb@.service` template unit file in [RPM](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/rpm/) packages on platforms that have <a undefined>cmake</a> versions older than 3.3.0, because these <a undefined>cmake</a> versions have a [bug](https://public.kitware.com/Bug/view.php?id=14782) that causes it to encounter errors when packaging a file in RPMs if the file name contains the `@` character. MariaDB's RHEL 7 and CentOS 7 RPM build hosts only got a new enough <a undefined>cmake</a> version starting with [MariaDB 10.1.39](/kb/en/mariadb-10139-release-notes/), [MariaDB 10.2.23](/kb/en/mariadb-10223-release-notes/), and [MariaDB 10.3.14](/kb/en/mariadb-10314-release-notes/). To use this functionality on a MariaDB version that does not have the file, you can copy the file from a package that does have the file.

#### Default configuration of Multiple Instances in 10.4 and Later

`systemd` will also look for an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) for a specific MariaDB instance based on the instance name.

It will use the `.%I` as the [custom option group suffix](/kb/en/configuring-mariadb-with-option-files/#custom-option-group-suffixes) that is appended to any [server option group](/kb/en/configuring-mariadb-with-option-files/#server-option-groups), in any configuration file included by default.

In all distributions, the `%I` is the MariaDB instance name. In the above `node1` case, it would use the [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) at the path`/etc/mynode1.cnf`.

When using multiple instances, each instance will of course also need their own <a undefined>datadir</a>, <a undefined>socket</a> and , <a undefined>port</a> (unless  <a undefined>skip_networking</a> is specified). As <a undefined>mysql_install_db#option-groups</a> reads the same sections as the server, and ExecStartPre= run [mysql_install_db](/clients-utilities/mysql_install_db/) within the service, the instances are autocreated if there is sufficient priviledges.

To use a 10.3 configuration in 10.4 or later and the following customisation in the editor after running `sudo systemctl edit mariadb@.service`:

```sql
[Unit]
ConditionPathExists=

[Service]
Environment='MYSQLD_MULTI_INSTANCE=--defaults-file=/etc/my%I.cnf'
```

#### Custom configuration of Multiple Instances in 10.4 and Later

Because users may want to do many various things with their multiple instances, we've provided a way to let the user define how they wish their multiple instances to run. The systemd environment variable `MYSQLD_MULTI_INSTANCE` can be set to anything that [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mariadbd/) and [mysql_install_db](/clients-utilities/mysql_install_db/) will recognise.

A hosting environment where each user has their own instance may look like 
(with `sudo systemctl edit mariadb@.service`):

```sql
[Service]
ProtectHome=false
Environment='MYSQLD_MULTI_INSTANCE=--defaults-file=/home/%I/my.cnf \
                        --user=%I \
                        --socket=/home/%I.sock \ 
                        --datadir=/home/%I/mariadb_data \
                        --skip-networking'
```

Here the instance name is the unix user of the service.

#### Configuring Multiple Instances in 10.3 and Earlier

`systemd` will also look for an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) for a specific MariaDB instance based on the instance name. By default, it will look for the option file in a directory defined at build time by the `INSTALL_SYSCONF2DIR` option provided to <a undefined>cmake</a>.

For example, on RHEL, CentOS, Fedora, and other similar Linux distributions, `INSTALL_SYSCONF2DIR` is defined as `/etc/my.cnf.d/`, so it will look for an option file that matches the format:

- `/etc/my.cnf.d/my%I.cnf`

And on Debian, Ubuntu, and other similar Linux distributions, `INSTALL_SYSCONF2DIR` is defined as `/etc/mysql/conf.d//`, so it will look for an option file that matches the format:

- `/etc/mysql/conf.d/my%I.cnf`

In all distributions, the `%I` is the MariaDB instance name. In the above `node1` case, it would use the [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) at the path`/etc/my.cnf.d/mynode1.cnf` for RHEL-like distributions and `/etc/mysql/conf.d/mynode1.cnf` for Debian-like distributions.

When using multiple instances, each instance will of course also need their own <a undefined>datadir</a>. See [mysql_install_db](/clients-utilities/mysql_install_db/) for information on how to initialize the <a undefined>datadir</a> for additional MariaDB instances.

## Systemd and Galera Cluster

### Bootstrapping a New Cluster

When using [Galera Cluster](/kb/en/galera/) with systemd, the first node in a cluster has to be started with `galera_new_cluster`. See [Getting Started with MariaDB Galera Cluster: Bootstrapping a New Cluster](/kb/en/getting-started-with-mariadb-galera-cluster/#bootstrapping-a-new-cluster) for more information.

### Recovering a Node's Cluster Position

When using [Galera Cluster](/kb/en/galera/) with systemd, a node's position in the cluster can be recovered with `galera_recovery`. See [Getting Started with MariaDB Galera Cluster: Determining the Most Advanced Node](/kb/en/getting-started-with-mariadb-galera-cluster/#determining-the-most-advanced-node) for more information.

### SSTs and Systemd

MariaDB's `systemd` unit file has a default startup timeout of about 90 seconds on most systems. If an SST takes longer than this default startup timeout on a joiner node, then `systemd` will assume that `mysqld` has failed to startup, which causes `systemd` to kill the `mysqld` process on the joiner node. To work around this, you can reconfigure the MariaDB `systemd` unit to have an [infinite timeout](#configuring-the-systemd-service-timeout). See [Introduction to State Snapshot Transfers (SSTs): SSTs and Systemd](/kb/en/introduction-to-state-snapshot-transfers-ssts/#ssts-and-systemd) for more information.

Note that [systemd 236 added the `EXTEND_TIMEOUT_USEC` environment variable](https://lists.freedesktop.org/archives/systemd-devel/2017-December/039996.html) that allows services to extend the startup timeout during long-running processes. Starting with [MariaDB 10.1.35](/kb/en/mariadb-10135-release-notes/), [MariaDB 10.2.17](/kb/en/mariadb-10217-release-notes/), and [MariaDB 10.3.8](/kb/en/mariadb-1038-release-notes/), on systems with systemd versions that support it, MariaDB uses this feature to extend the startup timeout during long SSTs. Therefore, if you are using `systemd` 236 or later, then you should not need to manually override `TimeoutStartSec`, even if your SSTs run for longer than the configured value. See [MDEV-15607](https://jira.mariadb.org/browse/MDEV-15607) for more information.

## Configuring the Systemd Service

You can configure MariaDB's `systemd` service by creating a "drop-in" configuration file for the `systemd` service. On most systems, the `systemd` service's directory for "drop-in" configuration files is `/etc/systemd/system/mariadb.service.d/`. You can confirm the directory and see what "drop-in" configuration files are currently loaded by executing:

```sql
$ sudo systemctl status mariadb.service
● mariadb.service - MariaDB 10.1.37 database server
   Loaded: loaded (/usr/lib/systemd/system/mariadb.service; enabled; vendor preset: disabled)
  Drop-In: /etc/systemd/system/mariadb.service.d
           └─migrated-from-my.cnf-settings.conf, timeoutstartsec.conf
...
```

If you want to configure the `systemd` service, then you can create a file with the `.conf` extension in that directory. The configuration option(s) that you would like to change would need to be placed in an appropriate section within the file, usually `[Service]`. If a `systemd` option is a list, then you may need to set the option to empty before you set the replacement values. For example:

```sql
[Service]

ExecStart=
ExecStart=/usr/bin/numactl --interleave=all  /usr/sbin/mysqld ${MYSQLD_OPTS} ${_WSREP_NEW_CLUSTER} ${_WSREP_START_POSITION}
```

After any configuration change, you will need to execute the following for the change to go into effect:

```sql
sudo systemctl daemon-reload
```

### Useful Systemd Options

Useful `systemd` options are listed below. If an option is equivalent to a common [mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe/) option, then that is also listed.

<table><tbody><tr><th>mysqld_safe option</th><th>systemd option</th><th>Comments</th></tr>
<tr><td>no option</td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.exec.html#ProtectHome=">ProtectHome=false</a></code></td><td>If any MariaDB files are in /home/</td></tr>
<tr><td>no option</td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.exec.html#PrivateDevices=">PrivateDevices=false</a></code></td><td>If any MariaDB storage references raw block devices</td></tr>
<tr><td>no option</td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.exec.html#ProtectSystem=">ProtectSystem=</a></code></td><td>If any MariaDB write any files to anywhere under /boot, /usr or /etc</td></tr>
<tr><td>no option</td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.service.html#TimeoutStartSec=">TimeoutStartSec={time}</a></code></td><td>Service startup timeout. See <a href="#configuring-the-systemd-service-timeout">Configuring the Systemd Service Timeout</a>.</td></tr>
<tr><td>no option (see <a href="https://jira.mariadb.org/browse/MDEV-9264">MDEV-9264</a>)</td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.exec.html#OOMScoreAdjust=">OOMScoreAdjust={priority}</a></code></td><td>e.g. -600 to lower priority of OOM killer for mysqld</td></tr>
<tr><td><code><a href="/kb/en/mysqld_safe/#mysqld_safe-options">open-files-limit</a></code></td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.exec.html#LimitCPU=">LimitNOFILE={limit}</a></code></td><td>Limit on number of open files. See <a href="#configuring-the-open-files-limit">Configuring the Open Files Limit</a>.</td></tr>
<tr><td><code><a href="/kb/en/mysqld_safe/#mysqld_safe-options">core-file-size</a></code></td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.exec.html#LimitCPU=">LimitCORE={size}</a></code></td><td>Limit on core file size. Useful when <a href="/kb/en/enabling-core-dumps/">enabling core dumps</a>. See <a href="#configuring-the-core-file-size">Configuring the Core File Size</a>.</td></tr>
<tr><td></td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.exec.html#LimitCPU=">LimitMEMLOCK={size}</a></code> or <code>infinity</code></td><td>Limit on how much can be locked in memory. Useful when <a href="/kb/en/server-system-variables/#large_pages">large-pages</a> or <a href="/kb/en/mysqld-options/#-memlock">memlock</a> is used</td></tr>
<tr><td><code><a href="/kb/en/mysqld_safe/#mysqld_safe-options">nice</a></code></td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.exec.html#Nice=">Nice={nice value}</a></code></td><td></td></tr>
<tr><td><code><a href="/kb/en/mysqld_safe/#mysqld_safe-options">syslog</a></code></td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.exec.html#StandardOutput=">StandardOutput=syslog</a></code></td><td>See <a href="#configuring-mariadb-to-write-the-error-log-to-syslog">Configuring MariaDB to Write the Error Log to Syslog</a>.</td></tr>
<tr><td></td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.exec.html#StandardError=">StandardError=syslog</a></code></td><td></td></tr>
<tr><td></td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.exec.html#SyslogFacility=">SyslogFacility=daemon</a></code></td><td></td></tr>
<tr><td></td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.exec.html#SyslogLevel=">SyslogLevel=err</a></code></td><td></td></tr>
<tr><td><code><a href="/kb/en/mysqld_safe/#mysqld_safe-options">syslog-tag</a></code></td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.exec.html#SyslogIdentifier=">SyslogIdentifier</a></code></td><td></td></tr>
<tr><td><code><a href="/kb/en/mysqld_safe/#mysqld_safe-options">flush-caches</a></code></td><td>ExecStartPre=/usr/bin/sync</td><td></td></tr>
<tr><td></td><td>ExecStartPre=/usr/sbin/sysctl -q -w vm.drop_caches=3</td><td></td></tr>
<tr><td><code><a href="/kb/en/mysqld_safe/#mysqld_safe-options">numa-interleave</a></code></td><td><code><a href="https://www.freedesktop.org/software/systemd/man/systemd.exec.html#NUMAPolicy=">NUMAPolicy=interleave</a></code></td><td>from systemd v243 onwards</td></tr>
<tr><td></td><td>or: <code>ExecStart=/usr/bin/numactl --interleave=all /usr/sbin/mysqld ${MYSQLD_OPTS} ${_WSREP_NEW_CLUSTER}  ${_WSREP_START_POSITION}</code></td><td>prepending <code>ExecStart=/usr/bin/numactl --interleave=all</code> to existing <code>ExecStart</code> setting</td></tr>
<tr><td><code><a href="/kb/en/mysqld_safe/#mysqld_safe-options">no-auto-restart</a></code></td><td>Restart={exit-status}</td><td></td></tr>
</tbody></table>

Note: the <a undefined>systemd</a> manual contains the official meanings for these options. The manual also lists considerably more options than the ones listed above.

There are other options and the `mariadb-service-convert` script will attempt to convert these as accurately as possible.

### Configuring the Systemd Service Timeout

MariaDB's [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) unit file has a default startup timeout of about 90 seconds on most systems. If a service startup takes longer than this default startup timeout, then `systemd` will assume that `mysqld` has failed to startup, which causes `systemd` to kill the `mysqld` process. To work around this, it can be changed by configuring the <a undefined>TimeoutStartSec</a> option for the `systemd` service.

A similar problem can happen when stopping the MariaDB service. Therefore, it may also be a good idea to set <a undefined>TimeoutStopSec</a>.

For example, you can reconfigure the MariaDB `systemd` service to have an infinite timeout by executing one of the following commands:

If you are using `systemd` 228 or older, then you can execute the following to set an infinite timeout:

```sql
sudo tee /etc/systemd/system/mariadb.service.d/timeoutsec.conf <<EOF
[Service]

TimeoutStartSec=0
TimeoutStopSec=0
EOF
sudo systemctl daemon-reload
```

[Systemd 229 added the infinity option](https://lists.freedesktop.org/archives/systemd-devel/2016-February/035748.html), so if you are using `systemd` 229 or later, then you can execute the following to set an infinite timeout:

```sql
sudo tee /etc/systemd/system/mariadb.service.d/timeoutsec.conf <<EOF
[Service]

TimeoutStartSec=infinity
TimeoutStopSec=infinity
EOF
sudo systemctl daemon-reload
```

Note that [systemd 236 added the EXTEND_TIMEOUT_USEC environment variable](https://lists.freedesktop.org/archives/systemd-devel/2017-December/039996.html) that allows services to extend the startup timeout during long-running processes. On systems with systemd versions that support it, MariaDB uses this feature to extend the startup timeout during certain startup processes that can run long.

### Configuring the Open Files Limit

When using `systemd`, rather than setting the open files limit by setting the <a undefined>open-files-limit</a> option for `mysqld_safe` or the <a undefined>open_files_limit</a> system variable, the limit can be changed by configuring the <a undefined>LimitNOFILE</a> option for the MariaDB `systemd` service. The default is set to `LimitNOFILE=16364` in `mariadb.service`.

For example, you can reconfigure the MariaDB `systemd` service to have a larger limit for open files by executing the following commands:

```sql
sudo tee /etc/systemd/system/mariadb.service.d/limitnofile.conf <<EOF
[Service]

LimitNOFILE=infinity
EOF
sudo systemctl daemon-reload
```

An important note is that setting `LimitNOFILE=infinity` doesn't actually set the open file limit to <em>infinite</em>.

In `systemd` 234 and later, setting `LimitNOFILE=infinity` actually sets the open file limit to the value of the kernel's `fs.nr_open` parameter. Therefore, in these `systemd` versions, you may have to change this parameter's value.

The value of the `fs.nr_open` parameter can be changed permanently by setting the value in <a undefined>/etc/sysctl.conf</a> and restarting the server.

The value of the `fs.nr_open` parameter can be changed temporarily by executing the <a undefined>sysctl</a> utility. For example:

```sql
sudo sysctl -w fs.nr_open=1048576‬
```

In `systemd` 233 and before, setting `LimitNOFILE=infinity` actually sets the open file limit to `65536`. See [systemd issue #6559](https://github.com/systemd/systemd/issues/6559) for more information. Therefore, in these `systemd` versions, it is not generally recommended to set `LimitNOFILE=infinity`. Instead, it is generally better to set `LimitNOFILE` to a very large integer. For example:

```sql
sudo tee /etc/systemd/system/mariadb.service.d/limitnofile.conf <<EOF
[Service]

LimitNOFILE=1048576
EOF
sudo systemctl daemon-reload
```

### Configuring the Core File Size

When using `systemd`, if you would like to [enable core dumps](/kb/en/enabling-core-dumps/), rather than setting the core file size by setting the <a undefined>core-file-size</a> option for `mysqld_safe`, the limit can be changed by configuring the <a undefined>LimitCORE</a> option for the MariaDB `systemd` service. For example, you can reconfigure the MariaDB `systemd` service to have an infinite size for core files by executing the following commands:

```sql
sudo tee /etc/systemd/system/mariadb.service.d/limitcore.conf <<EOF
[Service]

LimitCORE=infinity
EOF
sudo systemctl daemon-reload
```

### Configuring MariaDB to Write the Error Log to Syslog

When using `systemd`, if you would like to redirect the [error log](/mariadb-administration/server-monitoring-logs/error-log/) to the [syslog](https://linux.die.net/man/8/rsyslogd), then that can easily be done by doing the following:

- Ensure that <a undefined>log_error</a> system variable is <strong>not</strong> set.
- Set <a undefined>StandardOutput=syslog</a>.
- Set <a undefined>StandardError=syslog</a>.
- Set <a undefined>SyslogFacility=daemon</a>.
- Set <a undefined>SysLogLevel=err</a>.

For example:

```sql
sudo tee /etc/systemd/system/mariadb.service.d/syslog.conf <<EOF
[Service]

StandardOutput=syslog
StandardError=syslog
SyslogFacility=daemon
SysLogLevel=err
EOF
sudo systemctl daemon-reload
```

If you have multiple instances of MariaDB, then you may also want to set <a undefined>SyslogIdentifier</a> with a different tag for each instance.

### Configuring Access to Home Directories

MariaDB's [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) unit file restricts access to `/home`, `/root`, and `/run/user` by default. This restriction can be overriden by setting the <a undefined>ProtectHome</a> option to `false` for the MariaDB `systemd` service. This is done by creating a "drop-in" directory `/etc/systemd/system/mariadb.service.d/` and in it a file with a `.conf` suffix that contains the `ProtectHome=false` directive.

You can reconfigure the MariaDB `systemd` service to allow access to `/home` by executing the following commands:

```sql
sudo mkdir /etc/systemd/system/mariadb.service.d
sudo tee /etc/systemd/system/mariadb.service.d/dontprotecthome.conf <<EOF
[Service]

ProtectHome=false
EOF
sudo systemctl daemon-reload
```

### Configuring the umask

When using `systemd`, the default file permissions of `mysqld` can be set by setting the `UMASK` and `UMASK_DIR` environment variables for the `systemd` service. For example, you can configure the MariaDB `systemd` service's umask by executing the following commands:

```sql
sudo tee /etc/systemd/system/mariadb.service.d/umask.conf <<EOF
[Service]

Environment="UMASK=0750"
Environment="UMASK_DIR=0750"
EOF
sudo systemctl daemon-reload
```

These environment variables do not set the umask. They set the default file system permissions. See [MDEV-23058](https://jira.mariadb.org/browse/MDEV-23058) for more information.

Keep in mind that configuring the umask this way will only affect the permissions of files created by the `mysqld` process that is managed by `systemd`. The permissions of files created by components that are not managed by `systemd`, such as [mysql_install_db](/clients-utilities/mysql_install_db/), will not be effected.

See [Specifying Permissions for Schema (Data) Directories and Tables](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/specifying-permissions-for-schema-data-directories-and-tables/) for more information.

## Systemd Journal

`systemd` has its own logging system called the `systemd` journal. The `systemd` journal contains information about the service startup process. It is a good place to look when a failure has occurred.

The MariaDB `systemd` service's journal can be queried by using the <a undefined>journalctl</a> command. For example:

```sql
$ sudo journalctl n 20 -u mariadb.service
-- Logs begin at Fri 2019-01-25 13:49:04 EST, end at Fri 2019-01-25 18:07:02 EST. --
Jan 25 13:49:15 ip-172-30-0-249.us-west-2.compute.internal systemd[1]: Starting MariaDB 10.1.37 database server...
Jan 25 13:49:16 ip-172-30-0-249.us-west-2.compute.internal mysqld[2364]: 2019-01-25 13:49:16 140547528317120 [Note] /usr/sbin/mysqld (mysqld 10.1.37-MariaDB) starting as process 2364 ...
Jan 25 13:49:17 ip-172-30-0-249.us-west-2.compute.internal systemd[1]: Started MariaDB 10.1.37 database server.
Jan 25 18:06:42 ip-172-30-0-249.us-west-2.compute.internal systemd[1]: Stopping MariaDB 10.1.37 database server...
Jan 25 18:06:44 ip-172-30-0-249.us-west-2.compute.internal systemd[1]: Stopped MariaDB 10.1.37 database server.
Jan 25 18:06:57 ip-172-30-0-249.us-west-2.compute.internal systemd[1]: Starting MariaDB 10.1.37 database server...
Jan 25 18:08:32 ip-172-30-0-249.us-west-2.compute.internal systemd[1]: mariadb.service start-pre operation timed out. Terminating.
Jan 25 18:08:32 ip-172-30-0-249.us-west-2.compute.internal systemd[1]: Failed to start MariaDB 10.1.37 database server.
Jan 25 18:08:32 ip-172-30-0-249.us-west-2.compute.internal systemd[1]: Unit mariadb.service entered failed state.
Jan 25 18:08:32 ip-172-30-0-249.us-west-2.compute.internal systemd[1]: mariadb.service failed.
```

## Converting mysqld_safe Options to Systemd Options

`mariadb-service-convert` is a script included in many MariaDB packages that is used by the package manager to convert <a undefined>mysqld_safe</a> options to `systemd` options. It reads any explicit settings in the `[mysqld_safe]` [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) from [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), and its output is directed to `/etc/systemd/system/mariadb.service.d/migrated-from-my.cnf-settings.conf`. This helps to keep the configuration the same when upgrading from a version of MariaDB that does not use `systemd` to one that does.

Implicitly high defaults of <a undefined>open-files-limit</a> may be missed by the conversion script and require explicit configuration. See [Configuring the Open Files Limit](#configuring-the-open-files-limit).

## Known Issues

### Memlock

Prior to [MariaDB 10.1.10](/kb/en/mariadb-10110-release-notes/), the [--memlock](/kb/en/mysqld-options/#-memlock) option could not be used with the MariaDB `systemd` service.