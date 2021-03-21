# Existing Ansible Modules and Roles for MariaDB

This page contains links to Ansible modules and roles that can be used to automate MariaDB deployment and configuration. The list is not meant to be exhaustive. Use it as a starting point, but then please do your own research.

## Modules

At the time time of writing, there are no MariaDB-specific modules in Ansible Galaxy. MySQL modules can be used. Trying to use MySQL-specific features may result in errors or unexpected behavior. However, the same applies when trying to use a feature not supported by the MySQL version in use.

Currently, the [MySQL collection](https://galaxy.ansible.com/community/mysql?extIdCarryOver=true&amp;sc_cid=701f2000001OH7YAAW) in Ansible Galaxy contains at least the following modules:

- <strong>[mysql_db](https://docs.ansible.com/ansible/latest/collections/community/mysql/mysql_db_module.html)</strong>: manages MySQL databases.
- <strong>[mysql_info](https://docs.ansible.com/ansible/latest/collections/community/mysql/mysql_info_module.html)</strong>: gathers information about a MySQL server.
- <strong>[mysql_query](https://docs.ansible.com/ansible/latest/collections/community/mysql/mysql_query_module.html)</strong>: runs SQL queries against MySQL.
- <strong>[mysql_replication](https://docs.ansible.com/ansible/latest/collections/community/mysql/mysql_replication_module.html)</strong>: configures and operates asynchronous replication.
- <strong>[mysql_user](https://docs.ansible.com/ansible/latest/collections/community/mysql/mysql_user_module.html)</strong>: creates, modifies and deletes MySQL users.
- <strong>[mysql_variables](https://docs.ansible.com/ansible/latest/collections/community/mysql/mysql_variables_module.html)</strong>: manages MySQL configuration.

Note that some modules only exist as shortcuts, and it is possible to use mysql_query instead. However, it is important to notice that mysql_query is not idempotent. Ansible does not understand MySQL queries, therefore it cannot check whether a query needs to be run or not.

MariaDB Corporation maintains a [ColumnStore playbook](https://github.com/mariadb-corporation/columnstore-ansible) on GitHub.

### Other Useful Modules

Let's see some other modules that are useful to manage MariaDB servers.

#### shell and command

Modules like [shell](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html#ansible-collections-ansible-builtin-command-module) and [command](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/command_module.html#ansible-collections-ansible-builtin-command-module) allow one to run system commands.

To deploy on Windows, [win_shell](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_shell_module.html#ansible-collections-ansible-windows-win-shell-module) and [win_command](https://docs.ansible.com/ansible/latest/collections/ansible/windows/win_command_module.html#ansible-collections-ansible-windows-win-command-module) can be used.

Among other things, it is possible to use one of these modules to run MariaDB queries:

```sql
- name: Make the server read-only
  # become root to log into MariaDB with UNIX_SOCKET plugin
  become: yes
  shell: $( which mysql ) -e "SET GLOBAL read_only = 1;"
```

The main disadvantage with these modules is that they are not idempotent, because they're meant to run arbitrary system commands that Ansible can't understand. They are still useful in a variety of cases:

- To run queries, because mysql_query is also not idempotent.
- In cases when other modules do not allow us to use the exact arguments we need to use, we can achieve our goals by writing shell commands ourselves.
- To run custom scripts that implement non-trivial logic. Implementing complex logic in Ansible tasks is possible, but it can be tricky and inefficient.
- To call [command-line tools](/clients-utilities/). There may be specific roles for some of the most common tools, but most of the times using them is an unnecessary complication.

#### copy and template

An important part of configuration management is copying [configuration files](/mariadb-administration/getting-installing-and-upgrading-mariadb/configuring-mariadb-with-option-files/) to remote servers.

The [copy module](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/copy_module.html) allows us to copy files to target hosts. This is convenient for static files that we want to copy exactly as they are. An example task:

```sql
- name: Copy my.cnf
  copy:
    src: ./files/my.cnf.1
    dest: /etc/mysql/my.cnf
```

As you can see, the local name and the name on remote host don't need to match. This is convenient, because it makes it easy to use different configuration files with different servers. By default, files to copy are located in a `files` subdirectory in the role.

However, typically the content of a configuration file should vary based on the target host, the group and various variables. To do this, we can use the [template](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/template_module.html) module, which compiles and copies templates written in [Jinja](https://jinja.palletsprojects.com/en/2.11.x/) 2.

A simple template task:

```sql
- name: Compile and copy my.cnf
  copy:
    src: ./templates/my.cnf.j2
    dest: /etc/mysql/my.cnf
```

Again, the local and the remote names don't have to match. By default, Jinja 2 templates are located in a `templates` subdirectory in the role, and have the `.j2` extension.

A simple template example:

```sql
## WARNING: DO NOT EDIT THIS FILE MANUALLY !!
## IF YOU DO, THIS FILE WILL BE OVERWRITTEN BY ANSIBLE

[mysqld]
innodb_buffer_pool_size = {{ innodb_buffer_pool_size }}

{% if use_connect sameas true %}
connect_work_size = {{ connect_work_size }}
{% endif %}
```

#### Other Common Modules

The following modules are also often used for database servers:

- [package](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/package_module.html), [apt](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/apt_module.html) or [yum](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/yum_module.html) for package installation.
- [user](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/user_module.html), to manage system users and groups.
- [file](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/file_module.html), to handle files, directories, and permissions on them.
- [service](https://docs.ansible.com/ansible/latest/collections/ansible/builtin/service_module.html) to start, restart or stop services.

## Roles

Specific roles exist for MariaDB in Ansible Galaxy. Using them for MariaDB is generally preferable, to be sure to avoid [incompatibilities](/kb/en/mariadb-vs-mysql-compatibility/) and to probably be able to use some MariaDB specific [features](/kb/en/mariadb-vs-mysql-features/). However, using MySQL or Percona Server roles is also possible. This probably makes sense for users who also administer MySQL and Percona Server instances.

To find roles that suits you, check [Ansible Galaxy search page](https://galaxy.ansible.com/search?deprecated=false&amp;keywords=&amp;order_by=-relevance). Most roles are also available on GitHub.

## See also

- [MariaDB Deployment and Management with Ansible](https://youtu.be/CV8-56Fgjc0) (video)

---

Content initially contributed by [Vettabase Ltd](https://vettabase.com/).