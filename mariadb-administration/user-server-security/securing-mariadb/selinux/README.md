# Security-Enhanced Linux with MariaDB

[Security-Enhanced Linux (SELinux)](https://selinuxproject.org/page/Main_Page) is a Linux kernel module that provides a framework for configuring [mandatory access control (MAC)](https://en.wikipedia.org/wiki/Mandatory_access_control) system for many resources on the system. It is enabled by default on some Linux distributions, including RHEL, CentOS, Fedora, and other similar Linux distribution. SELinux prevents programs from accessing files, directories or ports unless it is configured to access those resources.

## Verifying Whether SELinux Is Enabled

To verify whether SELinux is enabled, execute the <a undefined>getenforce</a> command. For example:

```sql
getenforce
```

### Temporarily Putting mysqld Into Permissive Mode

When you are troubleshooting issues that you think SELinux might be causing, it can help to temporarily put `mysqld_t` into permissive mode. This can be done by executing the <a undefined>semanage</a> command. For example:

```sql
sudo semanage permissive -a mysqld_t
```

If that solved the problem, then it means that the current SELinux policy is the culprit. You need to adjust the SELinux policy or labels for MariaDB.

## Building and Loading the Server's SELinux Policy

MariaDB Server comes with an SELinux policy that is not build or loaded by default. On many systems, the policy is installed to `/usr/share/mysql/policy/selinux/mariadb-server.fc`. In order to build it and load it, execute the following:

```sql
sudo yum install selinux-policy-devel
cd /usr/share/mysql/policy/selinux/
make -f /usr/share/selinux/devel/Makefile mariadb-server.pp
sudo semodule -i mariadb-server.pp
```

## Setting File Contexts

SELinux uses [file contexts](https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/7/html/selinux_users_and_administrators_guide/sect-security-enhanced_linux-working_with_selinux-selinux_contexts_labeling_files) as a way to determine who should be able to access that file.

File contexts are managed with the <a undefined>semanage fcontext</a> and <a undefined>restorecon</a> commands.

On many systems, the <a undefined>semanage</a> utility is installed by the `policycoreutils-python` package, and the <a undefined>restorecon</a> utility is installed by the `policycoreutils` package. You can install these with the following command:

```sql
sudo yum install policycoreutils policycoreutils-python
```

A file or directory's current context can be checked by executing `ls` with the `--context` or `--scontext` options.

### Setting the File Context for the Data Directory

If you use a custom directory for <a undefined>datadir</a>, then you may need to set the file context for that directory. The SELinux file context for MariaDB data files is `mysqld_db_t`. You can determine if this file context is present on your system and which files or directories it is associated with by executing the following command:

```sql
sudo semanage fcontext --list | grep mysqld_db_t
```

If you would like to set the file context for your custom directory for your <a undefined>datadir</a>, then that can be done by executing the <a undefined>semanage fcontext</a> and <a undefined>restorecon</a> commands. For example:

```sql
sudo semanage fcontext -a -t mysqld_db_t "/mariadb/data(/.*)?"
sudo restorecon -Rv /mariadb/data
```

If you would like to check the current file context, you can do so by by executing `ls` with the `--context` or `--scontext` options. For example:

```sql
ls --directory --scontext /mariadb/data
```

### Setting the File Context for Log Files

If you use a custom directory for [log files](/mariadb-administration/server-monitoring-logs), then you may need to set the file context for that directory. The SELinux file context for MariaDB [log files](/mariadb-administration/server-monitoring-logs) is `mysqld_log_t`. You can determine if this file context is present on your system and which files or directories it is associated with by executing the following command:

```sql
sudo semanage fcontext --list | grep mysqld_log_t
```

If you would like to set the file context for your custom directory for [log files](/mariadb-administration/server-monitoring-logs), then that can be done by executing the <a undefined>semanage fcontext</a> and <a undefined>restorecon</a> commands. For example:

```sql
sudo semanage fcontext -a -t mysqld_log_t "/var/log/mysql(/.*)?"
sudo restorecon -Rv /var/log/mysql
```

If you would like to check the current file context, you can do so by by executing `ls` with the `--context` or `--scontext` options. For example:

```sql
ls --directory --scontext /var/log/mysql
```

### Setting the File Context for Option Files

If you use a custom directory for [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), then you may need to set the file context for that directory. The SELinux file context for MariaDB [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files) is `mysqld_etc_t`. You can determine if this file context is present on your system and which files or directories it is associated with by executing the following command:

```sql
sudo semanage fcontext --list | grep mysqld_etc_t
```

If you would like to set the file context for your custom directory for [option files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files), then that can be done by executing the <a undefined>semanage fcontext</a> and <a undefined>restorecon</a> commands. For example:

```sql
sudo semanage fcontext -a -t mysqld_etc_t "/etc/mariadb(/.*)?"
sudo restorecon -Rv /etc/mariadb
```

If you would like to check the current file context, you can do so by by executing `ls` with the `--context` or `--scontext` options. For example:

```sql
ls --directory --scontext /etc/mariadb
```

### Allowing Access to the Tmpfs File Context

If you wanted to mount your <a undefined>tmpdir</a> on a `tmpfs` file system or wanted to use a `tmpfs` file system on `/run/shm`, then you might need to allow `mysqld_t` to have access to a couple tmpfs-related file contexts. For example:

```sql
cd /usr/share/mysql/policy/selinux/
tee ./mysqld_tmpfs.te <<EOF
module mysqld_tmpfs 1.0;

require {
type tmpfs_t;
type mysqld_t;
class dir { write search read remove_name open getattr add_name };
class file { write getattr read lock create unlink open };
}

allow mysqld_t tmpfs_t:dir { write search read remove_name open getattr add_name };

allow mysqld_t tmpfs_t:file { write getattr read lock create unlink open }
EOF
sudo checkmodule -M -m mysqld_tmpfs.te -o mysqld_tmpfs.mod
sudo semodule_package -m mysqld_tmpfs.mod -o mysqld_tmpfs.pp 
sudo semodule -i mysqld_tmpfs.pp
```

## Troubleshooting SELinux Issues

You might need to troubleshoot SELinux-related issues in cases, such as:

- MariaDB is using a non-default port.
- MariaDB is reading from or writing to some files (datadir, log files, option files, etc.) located at non-default paths.
- MariaDB is using a plugin that requires access to resources that default installations do not use.

### File System Permission Errors

If the file system permissions for some MariaDB directory look fine, but the MariaDB [error log](/mariadb-administration/server-monitoring-logs/error-log) still has errors that look similar to the following:

```sql
130321 11:50:51 mysqld_safe Starting mysqld daemon with databases from /datadir
...
2013-03-21 11:50:52 2119 [Warning] Can't create test file /datadir/
2013-03-21 11:50:52 2119 [Warning] Can't create test file /datadir/
...
2013-03-21 11:50:52 2119 [ERROR] /usr/sbin/mysqld: Can't create/write to file 
    '/datadir/boxy.pid' (Errcode: 13 - Permission denied)
2013-03-21 11:50:52 2119 [ERROR] Can't start server: can't create PID file: 
    Permission denied
130321 11:50:52 mysqld_safe mysqld from pid file /datadir/boxy.pid ended
```

Then check SELinux's `/var/log/audit/audit.log` for log entries that look similar to the following:

```sql
type=AVC msg=audit(1363866652.030:24): avc:  denied  { write } for  pid=2119 
    comm="mysqld" name="datadir" dev=dm-0 ino=394 
    scontext=unconfined_u:system_r:mysqld_t:s0 
    tcontext=unconfined_u:object_r:default_t:s0 tclass=dir
```

If you see any entries that look similar to this, then you most likely need to adjust the file contexts for some files or directories. See [Setting File Contexts](#setting-file-contexts) for more information on how to do that.

### SELinux and MariaDB On a Different Port

TCP and UDP ports are enabled for permission to bind too. If you are using a different port, or some Galera ports, configure SELinux to be able to use those ports:

```sql
sudo semanage port -a -t mysqld_port_t -p tcp 3307 
```

### Generating SELinux Policies with audit2allow

In some cases, a MariaDB system might need non-standard policies. It is possible to create these policies from the SELinux audit log using the <a undefined>audit2allow</a> utility. The <a undefined>semanage</a> and <a undefined>semodule</a> utilities will also be needed.

On many systems, the <a undefined>audit2allow</a> and <a undefined>semanage</a> utilities are installed by the `policycoreutils-python` package, and the <a undefined>semodule</a> utility is installed by the `policycoreutils` package. You can install these with the following command:

```sql
sudo yum install policycoreutils policycoreutils-python
```

The following process can be used to generate a policy from the audit log:

- Remove dontaudits from the policy:

```sql
sudo semodule -DB
```

- Temporarily put `mysqld_t` into permissive mode. For example:

```sql
sudo semanage permissive -a mysqld_t
```

- [Start MariaDB](/kb/en/starting-and-stopping-mariadb-starting-and-stopping-mariadb/).

- Do whatever was causing SELinux errors.

- Use the generated audit log to create a policy:

```sql
sudo grep mysqld /var/log/audit/audit.log | audit2allow -M mariadb_local
sudo semodule -i mariadb_local.pp
```

- Pull `mysqld_t` out of permissive mode. For example:

```sql
sudo semanage permissive -d mysqld_t
```

- Restore dontaudits for the policy:

```sql
sudo setmodule -B
```

The same procedure can be used if MariaDB starts but SELinux prevents it from functioning correctly. For example, SELinux may prevent [PAM plugin](/kb/en/pam-authentication-plugin/) from authenticating users. The solution is the same — enable auditing, switch to permissive, do, whatever SELinux didn't allow you to, create a policy from the audit log.

When you discover any needed SELinux permissions, please report the needed permissions to your operating system bug tracking so all users can benefit from your work (e.g. Red Hat Bugzilla [https://bugzilla.redhat.com/](https://bugzilla.redhat.com/)).