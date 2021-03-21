# Configuring Linux for MariaDB

## Linux kernel settings

### IO scheduler

For optimal IO performance running a database we are using the <em>noop</em> scheduler. Recommended schedulers are <em>noop</em> and <em>deadline</em>. You can check your scheduler setting with:

```sql
cat /sys/block/${DEVICE}/queue/scheduler
```

For instance, it should look like this output:

```sql
cat /sys/block/sda/queue/scheduler
[noop] deadline cfq
```

You can find detailed notes about Linux schedulers here: [Linux schedulers in TPCC like benchmark](http://www.mysqlperformanceblog.com/2009/01/30/linux-schedulers-in-tpcc-like-benchmark/).

## Resource Limits

### Configuring the Open Files Limit

By default, the system limits how many open file descriptors a process can have open at one time. It has both a soft and hard limit. On many systems, both the soft and hard limit default to 1024. On an active database server, it is very easy to exceed 1024 open file descriptors. Therefore, you may need to increase the soft and hard limits. There are a few ways to do so.

If you are using [mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe/) to start `mysqld`, then see the instructions at [mysqld_safe: Configuring the Open Files Limit](/kb/en/mysqld_safe/#configuring-the-open-files-limit).

If you are using [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) to start `mysqld`, then see the instructions at [systemd: Configuring the Open Files Limit](/kb/en/systemd/#configuring-the-open-files-limit).

Otherwise, you can set the soft and hard limits for the `mysql` user account by adding the following lines to <a undefined>/etc/security/limits.conf</a>:

```sql
mysql soft nofile 65535
mysql hard nofile 65535
```

After the system is rebooted, the `mysql` user should use the new limits, and the user's `ulimit` output should look like the following:

```sql
$ ulimit -Sn
65535
$ ulimit -Hn
65535
```

### Configuring the Core File Size

By default, the system limits the size of core files that could be created. It has both a soft and hard limit. On many systems, the soft limit defaults to 0. If you want to [enable core dumps](/kb/en/enabling-core-dumps/), then you may need to increase this. Therefore, you may need to increase the soft and hard limits. There are a few ways to do so.

If you are using [mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe/) to start `mysqld`, then see the instructions at [mysqld_safe: Configuring the Core File Size](/kb/en/mysqld_safe/#configuring-the-core-file-size).

If you are using [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) to start `mysqld`, then see the instructions at [systemd: Configuring the Core File Size](/kb/en/systemd/#configuring-the-core-file-size).

Otherwise, you can set the soft and hard limits for the `mysql` user account by adding the following lines to <a undefined>/etc/security/limits.conf</a>:

```sql
mysql soft core unlimited
mysql hard core unlimited
```

After the system is rebooted, the `mysql` user should use the new limits, and the user's `ulimit` output should look like the following:

```sql
$ ulimit -Sc
unlimited
$ ulimit -Hc
unlimited
```

## Swappiness

See [configuring swappiness](/mariadb-administration/getting-installing-and-upgrading-mariadb/mariadb-performance-advanced-configurations/configuring-swappiness/).