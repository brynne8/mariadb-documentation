# Running Multiple MariaDB Server Processes

It is possible to run multiple MariaDB Server processes on the same server, but there are certain things that need to be kept in mind. This page will go over some of those things.

## Configuring Multiple MariaDB Server Processes

If multiple MariaDB Server process are running on the same server, then at minimum, you will need to ensure that the different instances do not use the same <a undefined>datadir</a>, <a undefined>port</a>, and <a undefined>socket</a>. The following example shows these options set in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/):

```sql
[client]
# TCP port to use to connect to mysqld server
port=3306
# Socket to use to connect to mysqld server
socket=/tmp/mysql.sock
[mariadb]
# TCP port to make available for clients
port=3306
#  Socket to make available for clients
socket=/tmp/mysql.sock
# Where MariaDB should store all its data
datadir=/usr/local/mysql/data
```

The above values are the defaults. If you would like to run multiple MariaDB Server instances on the same server, then you will need to set unique values for each instance.

There may be additional options that also need to be changed for each instance. Take a look at the full list of options for [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/).

To see the current values set for an instance, see [Checking Program Options](/kb/en/configuring-mariadb-with-option-files/#checking-program-options) for how to do so.

To list the default values, check the end of:

```sql
mysqld --help --verbose
```

## Starting Multiple MariaDB Server Processes

There are several different methods to start or stop the MariaDB Server process. There are two primary categories that most of these methods fall into: starting the process with the help of a service manager, and starting the process manually. See [Starting and Stopping MariaDB](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/) for more information.

### Service Managers

[sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit/) and [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) are the most common Linux service managers. [launchd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/launchd/) is used in MacOS X. [Upstart](https://en.wikipedia.org/wiki/Upstart_(software)) is a less common service manager.

#### Systemd

RHEL/CentOS 7 and above, Debian 8 Jessie and above, and Ubuntu 15.04 and above use [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) by default.

For information on how to start and stop multiple MariaDB Server processes on the same server with this service manager, see [systemd: Interacting with Multiple MariaDB Server Processes](/kb/en/systemd/#interacting-with-multiple-mariadb-server-processes).

### Starting the Server Process Manually

#### mysqld

[mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) is the actual MariaDB Server binary. It can be started manually on its own.

If you want to force each instance to read only a single [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then you can use the <a undefined>--defaults-file</a> option:

```sql
mysqld --defaults-file=/etc/my_instance1.cnf
```

#### mysqld_safe

[mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe/) is a wrapper that can be used to start the [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) server process. The script has some built-in safeguards, such as automatically restarting the server process if it dies. See [mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe/) for more information.

If you want to force each instance to read only a single [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/), then you can use the <a undefined>--defaults-file</a> option:

```sql
mysqld_safe --defaults-file=/etc/my_instance1.cnf
```

#### mysqld_multi

[mysqld_multi](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_multi/) is a wrapper that can be used to start the [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) server process if you plan to run multiple server processes on the same host. See [mysqld_multi](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_multi/) for more information.

## Other Options

In some cases, there may be easier ways to run multiple MariaDB Server instances on the same server, such as:

- Using [dbdeployer](/clients-utilities/dbdeployer/).
- Starting multiple [Docker](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/docker-and-mariadb/installing-and-using-mariadb-via-docker/) containers.