# Rotating Logs on Unix and Linux

Unix and Linux distributions offer the <a undefined>logrotate</a> utility, which makes it very easy to rotate log files. This page will describe how to configure log rotation for the [error log](/mariadb-administration/server-monitoring-logs/error-log/), [general query log](/mariadb-administration/server-monitoring-logs/general-query-log/), and the [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/).

## Configuring Locations and File Names of Logs

The first step is to configure the locations and file names of logs. To make the log rotation configuration easier, it can be best to put these logs in a dedicated log directory.

We will need to configure the following:

- The [error log](/mariadb-administration/server-monitoring-logs/error-log/) location and file name is configured with the <a undefined>log_error</a> system variable.
- The [general query log](/mariadb-administration/server-monitoring-logs/general-query-log/) location and file name is configured with the <a undefined>general_log_file</a> system variable.
- The [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/) location and file name is configured with the <a undefined>slow_query_log_file</a> system variable.

If you want to enable the [general query log](/mariadb-administration/server-monitoring-logs/general-query-log/) and [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/) immediately, then you will also have to configure the following:

- The [general query log](/mariadb-administration/server-monitoring-logs/general-query-log/) is enabled with the <a undefined>general_log</a> system variable.
- The [slow query log](/mariadb-administration/server-monitoring-logs/slow-query-log/) is enabled with the <a undefined>slow_query_log</a> system variable.

These options can be set in a server [option group](/kb/en/configuring-mariadb-with-option-files/#option-groups) in an [option file](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) prior to starting up the server. For example, if we wanted to put our log files in `/var/log/mysql/`, then we could configure the following:

```sql
[mariadb]
...
log_error=/var/log/mysql/mariadb.err
general_log
general_log_file=/var/log/mysql/mariadb.log
slow_query_log
slow_query_log_file=/var/log/mysql/mariadb-slow.log
long_query_time=5
```

We will also need to create the relevant directory:

```sql
sudo mkdir /var/log/mysql/
sudo chown mysql:mysql /var/log/mysql/
sudo chmod 0770 /var/log/mysql/
```

If you are using [SELinux](/mariadb-administration/user-server-security/securing-mariadb/selinux/), then you may also need to set the SELinux context for the directory. See [SELinux: Setting the File Context for Log Files](/kb/en/selinux/#setting-the-file-context-for-log-files) for more information. For example:

```sql
sudo semanage fcontext -a -t mysqld_log_t "/var/log/mysql(/.*)?"
sudo restorecon -Rv /var/log/mysql
```

After MariaDB is [restarted](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/), it will use the new log locations and file names.

## Configuring Authentication for Logrotate

The <a undefined>logrotate</a> utility needs to be able to authenticate with MariaDB in order to flush the log files.

The easiest way to allow the <a undefined>logrotate</a> utility to authenticate with MariaDB is to configure the `root@localhost` user account to use [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/) authentication.

##### MariaDB starting with [10.4](/kb/en/what-is-mariadb-104/)

In [MariaDB 10.4](/kb/en/what-is-mariadb-104/) and later, the the `root@localhost` user account is configured to use [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/) authentication by default, so this part can be skipped in those versions.

##### MariaDB until [10.3](/kb/en/what-is-mariadb-103/)

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, a user account is only able to have one authentication method at a time. In these versions, this means that once you enable [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/) authentication for the `root@localhost` user account, you will no longer be able to use a password to log in with that user account. The user account will only be able to use [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/) authentication.

In [MariaDB 10.3](/kb/en/what-is-mariadb-103/) and before, you need to [install the unix_socket plugin](/kb/en/authentication-plugin-unix-socket/#installing-the-plugin) before you can configure the `root@localhost` user account to use it. For example:

```sql
INSTALL SONAME 'auth_socket';
```

After the plugin is installed, the `root@localhost` user account can be configured to use [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/) authentication. How this is done depends on the version of MariaDB.

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

In [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and later, the `root@localhost` user account can be altered to use [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/) authentication with the [ALTER USER](/sql-statements-structure/sql-statements/account-management-sql-commands/alter-user/) statement. For example:

```sql
ALTER USER 'root'@'localhost' IDENTIFIED VIA unix_socket;
```

##### MariaDB until [10.1](/kb/en/what-is-mariadb-101/)

In [MariaDB 10.1](/kb/en/what-is-mariadb-101/) and before, the `root@localhost` user account can be altered to use [unix_socket](/columns-storage-engines-and-plugins/plugins/authentication-plugins/authentication-plugin-unix-socket/) authentication with the [GRANT](/sql-statements-structure/sql-statements/account-management-sql-commands/grant/) statement. For example:

```sql
GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED VIA unix_socket WITH GRANT OPTION;
```

## Configuring Logrotate

At this point, we can configure the <a undefined>logrotate</a> utility to rotate the log files.

On many systems, the primary <a undefined>logrotate</a> configuration file is located at the following path:

- `/etc/logrotate.conf`

And the <a undefined>logrotate</a> configuration files for individual services are located in the following directory:

- `/etc/logrotate.d/`

We can create a <a undefined>logrotate</a> configuration file for MariaDB by executing the following command in a shell:

```sql
$ sudo tee /etc/logrotate.d/mariadb <<EOF
/var/log/mysql/* {
        su mysql mysql
        missingok
        create 660 mysql mysql
        notifempty
        daily
        minsize 1M # only use with logrotate >= 3.7.4
        maxsize 100M # only use with logrotate >= 3.8.1
        rotate 30
        # dateext # only use if your logrotate version is compatible with below dateformat
        # dateformat .%Y-%m-%d-%H-%M-%S # only use with logrotate >= 3.9.2
        compress
        delaycompress
        sharedscripts 
        olddir archive/
        createolddir 770 mysql mysql # only use with logrotate >= 3.8.9
    postrotate
        # just if mysqld is really running
        if test -x /usr/bin/mysqladmin && \
           /usr/bin/mysqladmin ping &>/dev/null
        then
           /usr/bin/mysqladmin --local flush-error-log \
              flush-engine-log flush-general-log flush-slow-log
        fi
    endscript
}
EOF
```

You may have to modify this configuration file to use it on your system, depending on the specific version of the <a undefined>logrotate</a> utility that is installed. See the description of each configuration directive below to determine which <a undefined>logrotate</a> versions support that configuration directive.

Each specific configuration directive does the following:

- <strong>`missingok`</strong>: This directive configures it to ignore missing files, rather than failing with an error.
- <strong>`create 660 mysql mysql`</strong>: This directive configures it to recreate the log files after log rotation with the specified permissions and owner.
- <strong>`notifempty`</strong>: This directive configures it to skip a log file during log rotation if it is empty.
- <strong>`daily`</strong>: This directive configures it to rotate each log file once per day.
- <strong>`minsize 1M`</strong>: This directive configures it to skip a log file during log rotation if it is smaller than 1 MB. This directive is only available with <a undefined>logrotate</a> 3.7.4 and later.
- <strong>`maxsize 100M`</strong>: This directive configures it to rotate a log file more frequently than daily if it grows larger than 100 MB. This directive is only available with <a undefined>logrotate</a> 3.8.1 and later.
- <strong>`rotate 30`</strong>: This directive configures it to keep 30 old copies of each log file.
- <strong>`dateext`</strong>: This directive configures it to use the date as an extension, rather than just a number. This directive is only available with <a undefined>logrotate</a> 3.7.6 and later.
- <strong>`dateformat .%Y-%m-%d-%H-%M-%S`</strong>: This directive configures it to use this date format string (as defined by the format specification for <a undefined>strftime</a>) for the date extension configured by the `dateext` directive. This directive is only available with <a undefined>logrotate</a> 3.7.7 and later. Support for `%H` is only available with <a undefined>logrotate</a> 3.9.0  and later. Support for `%M` and `%S` is only available with <a undefined>logrotate</a> 3.9.2 and later.
- <strong>`compress`</strong>: This directive configures it to compress the log files with <a undefined>gzip</a>.
- <strong>`delaycompress`</strong>: This directive configures it to delay compression of each log file until the next log rotation. If the log file is compressed at the same time that it is rotated, then there may be cases where a log file is being compressed while the MariaDB server is still writing to the log file. Delaying compression of a log file until the next log rotation can prevent race conditions such as these that can happen between the compression operation and the MariaDB server's log flush operation.
- <strong>`olddir archive/`</strong>: This directive configures it to archive the rotated log files in `/var/log/mysql/archive/`.
- <strong>`createolddir 770 mysql mysql`</strong>: This directive configures it to create the directory specified by the `olddir` directive with the specified permissions and owner, if the directory does not already exist. This directive is only available with <a undefined>logrotate</a> 3.8.9 and later.
- <strong>`sharedscripts`</strong>: This directive configures it to run the `postrotate` script just once, rather than once for each rotated log file.
- <strong>`postrotate`</strong>: This directive configures it to execute a script after log rotation. This particular script executes the [mysqladmin](/clients-utilities/mysqladmin/) utility, which executes the [FLUSH](/sql-statements-structure/sql-statements/administrative-sql-statements/flush-commands/flush/) statement, which tells the MariaDB server to flush its various log files. When MariaDB server flushes a log file, it closes its existing file handle and reopens a new one. This ensure that MariaDB server does not continue writing to a log file after it has been rotated. This is an important component of the log rotation process.

If our system does not have <a undefined>logrotate</a> 3.8.9 or later, which is needed to support the `createolddir` directive, then we will also need to create the relevant directory specified by the `olddir` directive:

```sql
sudo mkdir /var/log/mysql/archive/
sudo chown mysql:mysql /var/log/mysql/archive/
sudo chmod 0770 /var/log/mysql/archive/
```

## Testing Log Rotation

We can test log rotation by executing the <a undefined>logrotate</a> utility with the `--force` option. For example:

```sql
sudo logrotate --force /etc/logrotate.d/mariadb
```

Keep in mind that under normal operation, the <a undefined>logrotate</a> utility may skip a log file during log rotation if the utility does not believe that the log file needs to be rotated yet. For example:

- If you set the `notifempty` directive mentioned above, then it will be configured to skip a log file during log rotation if the log file is empty.
- If you set the `daily` directive mentioned above, then it will be configured to only rotate each log file once per day.
- If you set the `minsize 1M` directive mentioned above, then it will be configured to skip a log file during log rotation if the log file size is smaller than 1 MB.

However, when running tests with the `--force` option, the <a undefined>logrotate</a> utility does not take these options into consideration.

After a few tests, we can see that the log rotation is indeed working:

```sql
$ sudo ls -l /var/log/mysql/archive/
total 48
-rw-rw---- 1 mysql mysql  440 Mar 31 15:31 mariadb.err.1
-rw-rw---- 1 mysql mysql  138 Mar 31 15:30 mariadb.err.2.gz
-rw-rw---- 1 mysql mysql  145 Mar 31 15:28 mariadb.err.3.gz
-rw-rw---- 1 mysql mysql 1007 Mar 31 15:27 mariadb.err.4.gz
-rw-rw---- 1 mysql mysql 1437 Mar 31 15:32 mariadb.log.1
-rw-rw---- 1 mysql mysql  429 Mar 31 15:31 mariadb.log.2.gz
-rw-rw---- 1 mysql mysql  439 Mar 31 15:28 mariadb.log.3.gz
-rw-rw---- 1 mysql mysql  370 Mar 31 15:27 mariadb.log.4.gz
-rw-rw---- 1 mysql mysql 3915 Mar 31 15:32 mariadb-slow.log.1
-rw-rw---- 1 mysql mysql  554 Mar 31 15:31 mariadb-slow.log.2.gz
-rw-rw---- 1 mysql mysql  569 Mar 31 15:28 mariadb-slow.log.3.gz
-rw-rw---- 1 mysql mysql  487 Mar 31 15:27 mariadb-slow.log.4.gz
```

## Logrotate in Ansible

Let's see an example of how to configure logrotate in Ansible.

First, we'll create a couple of tasks in our playbook:

```sql
- name: Create mariadb_logrotate_old_dir
  file:
    path: "{{ mariadb_logrotate_old_dir }}"
    owner: mysql
    group: mysql
    mode: '770'
    state: directory

- name: Configure logrotate
  template:
    src: "../templates/logrotate.j2"
    dest: "/etc/logrotate.d/mysql"
```

The first task creates a directory to store the old, compressed logs, and set proper permissions.

The second task uploads logrotate configuration file into the proper directory, and calls it `mysql`. As you can see the original name is different, and it ends with the `.j2` extension, because it is a Jinja 2 template.

The file will look like the following:

```sql
{{ mariadb_log_dir }}/* {
        su mysql mysql
        missingok
        create 660 mysql mysql
        notifempty
        daily
        minsize 1M {{ mariadb_logrotate_min_size }}
        maxsize 100M {{ mariadb_logrotate_max_size }}
        rotate {{ mariadb_logrotate_old_dir }}
        dateformat .%Y-%m-%d-%H-%M-%S # only use with logrotate >= 3.9.2
        compress
        delaycompress
        sharedscripts 
        olddir archive/
        createolddir 770 mysql mysql # only use with logrotate >= 3.8.9
    postrotate
        # just if mysqld is really running
        if test -x /usr/bin/mysqladmin && \
           /usr/bin/mysqladmin ping &>/dev/null
        then
           /usr/bin/mysqladmin --local flush-error-log \
              flush-engine-log flush-general-log flush-slow-log
        fi
    endscript
}
```

The file is very similar to the file shown above, which is obvious because we're still uploading a logrotate configuration file. Ansible is just a tool we've chosen to do this.

However, both in the tasks and in the template, we used some variables. This allows to use different paths and rotation parameters for different hosts, or host groups.

If we have a group host called `mariadb` that contains the default configuration for all our MariaDB servers, we can define these variables in a file called `group_vars/mariadb.yml`:

```sql
# MariaDB writes its logs here
mariadb_log_dir: /var/lib/mysql/logs

# logrotate configuration

mariadb_logrotate_min_size: 500M
mariadb_logrotate_max_size: 1G
mariadb_logrotate_old_files: 7
mariadb_logrotate_old_dir: /var/mysql/old-logs
```

After setting up logrotate in Ansible, you may want to deploy it to a non-production server and test it manually as explained above. Once you're sure that it works fine on one server, you can be confident in the new Ansible tasks and deploy them on all servers.

For more information on how to use Ansible to automate MariaDB configuration, see [Ansible and MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/ansible-and-mariadb/).