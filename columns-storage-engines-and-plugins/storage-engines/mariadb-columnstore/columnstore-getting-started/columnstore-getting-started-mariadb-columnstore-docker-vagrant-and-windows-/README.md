# MariaDB ColumnStore Docker, Vagrant, and Windows 10 Linux Setup - (allows for evaluation on a PC or Mac)

# Introduction

Docker and Vagrant allow for a simple and lightweight setup of a MariaDB ColumnStore single server instance for evaluation purposes. The configuration of both is designed for simplified developer / evaluation setup rather than production use.  Both allow for evaluating ColumnStore on a PC or MAC if a Linux environment is unavailable. Both distributions use a base OS of CentOS 7 and currently require separate download of the CentOS7 RPM install bundle.

Official images are planned to be produced after ColumnStore goes GA.

# Windows Linux Subsystem

If you have Windows 10 Creators update installed, then you can install the Ubuntu 16 installation into the bash console. Please follow the Ubuntu instructions in getting started.  If you have recently upgraded and had bash installed previously, ensure you uninstall and reinstall bash first to have a clean Ubuntu16 version. Note that ColumnStore will be terminated should you terminate the bash console.

# Docker

[Docker](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/docker-and-mariadb) allows for creation of lightweight containers (running in a virtual machine on a PC or MAC) that allows for creation of lightweight and reproducible containers with a dedicated function.

Since MariaDB ColumnStore relies on a Syslog daemon, the container must start both ColumnStore and rsyslogd and the runit utility is used to achieve this.

A single node docker image can be found at [MariaDB on docker hub](https://hub.docker.com/r/mariadb/columnstore/).

```sql
docker run -d  --name mcs mariadb/columnstore
docker exec -it mcs bash
```

A ColumnStore cluster can be brought up using a compose file provided in the ColumnStore github repository:

```sql
git clone https://github.com/mariadb-corporation/mariadb-columnstore-docker.git
cd mariadb-columnstore-docker/columnstore
docker-compose up -d
```

# Vagrant

[Vagrant](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/vagrant-and-mariadb) allows for creation of simple and reproducible virtual machine images for development environments.

The basic Vagrant setup can be found at [GitHub](https://github.com/mariadb-corporation/mariadb-columnstore-vagrant). Clone this repository and follow the Readme.md instructions.