# Starting and Stopping MariaDB

There are several different methods to start or stop the MariaDB Server process. There are two primary categories that most of these methods fall into: starting the process with the help of a service manager, and starting the process manually.

## Service Managers

[sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit/) and [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) are the most common Linux service managers. [launchd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/launchd/) is used in MacOS X. [Upstart](https://en.wikipedia.org/wiki/Upstart_(software)) is a less common service manager.

### Systemd

RHEL/CentOS 7 and above, Debian 8 Jessie and above, and Ubuntu 15.04 and above use [systemd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/systemd/) by default.

For information on how to start and stop MariaDB with this service manager, see [systemd: Interacting with the MariaDB Server Process](/kb/en/systemd/#interacting-with-the-mariadb-server-process).

### SysVinit

RHEL/CentOS 6 and below, and Debian 7 Wheezy and below use [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit/) by default.

For information on how to start and stop MariaDB with this service manager, see [sysVinit: Interacting with the MariaDB Server Process](/kb/en/sysvinit/#interacting-with-the-mariadb-server-process).

### launchd

[launchd](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/launchd/) is used in MacOS X.

### Upstart

Ubuntu 14.10 and below use Upstart by default.

## Starting the Server Process Manually

### mysqld

[mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) is the actual MariaDB Server binary. It can be started manually on its own.

### mysqld_safe

[mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe/) is a wrapper that can be used to start the [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) server process. The script has some built-in safeguards, such as automatically restarting the server process if it dies. See [mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe/) for more information.

### mysqld_multi

[mysqld_multi](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_multi/) is a wrapper that can be used to start the [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) server process if you plan to run multiple server processes on the same host. See [mysqld_multi](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_multi/) for more information.

### mysql.server

[mysql.server](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqlserver/) is a wrapper that works as a standard [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit/) script. However, it can be used independently of [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit/) as a regular `sh` script. The script starts the [mysqld](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld-options/) server process by first changing its current working directory to the MariaDB install directory and then starting [mysqld_safe](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqld_safe/). The script requires the standard [sysVinit](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/sysvinit/) arguments, such as `start`, `stop`, and `status`. See [mysql.server](/mariadb-administration/getting-installing-and-upgrading-mariadb/starting-and-stopping-mariadb/mysqlserver/) for more information.