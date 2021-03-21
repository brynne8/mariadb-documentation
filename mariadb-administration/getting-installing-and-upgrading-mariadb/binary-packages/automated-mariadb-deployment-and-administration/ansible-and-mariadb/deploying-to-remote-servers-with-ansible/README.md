# Deploying to Remote Servers with Ansible

If we manage several remote servers, running commands on them manually can be frustrating and time consuming. Ansible allows one to run commands on a whole group of servers.

This page shows some examples of ansible-playbook invocations. We'll see how to deploy roles or parts of them to remote servers. Then we'll see how to run commands on remote hosts, and possibly to get information from them. Make sure to read [Ansible Overview](/kb/en/ansible-overview/) first, to understand Ansible general concepts.

## Pinging Remote Servers

Let's start with the simplest example: we just want our local Ansible to ping remote servers to see if they are reachable. Here's how to do it:

```sql
ansible -i production-mariadb all -m ping
```

Before proceeding with more useful examples, let's discuss this syntax.

- <strong>ansible</strong> is the executable we can call to run a command from remote servers.
- <strong>-i production-mariadb</strong> means that the servers must be read from an inventory called production-mariadb.
- <strong>all</strong> means that the command must be executed against all servers from the above inventory.
- <strong>-m ping</strong> specifies that we want to run the ping module. This is not the ping Linux command. It tells us if Ansible is able to connect a remote server and run a simple commands on them.

To run ping on a specific group or host, we can just replace "all" with a group name or host name from the inventory:

```sql
ansible -i production-mariadb main_cluster -m ping
```

## Running Commands on Remote Servers

The previous examples show how to run an Ansible module on remote servers. But it's also possible to run custom commands over SSH. Here's how:

```sql
ansible -i production-mariadb all -a 'echo $PATH'
```

This command shows the value of `$PATH` on all servers in the inventory "production-mariadb".

We can also run commands as root by adding the `-b` (or `--become`) option:

```sql
# print a MariaDB variable
ansible -i production-mariadb all -b -a 'mysql -e "SHOW GLOBAL VARIABLES LIKE \'innodb_buffer_pool_size\';"'

# reboot servers
ansible -i production-mariadb all -b -a 'reboot'
```

## Applying Roles to Remote Servers

We saw how to run commands on remote hosts. Applying roles to remote hosts is not much harder, we just need to add some information. An example:

```sql
ansible-playbook -i production-mariadb production-mariadb.yml
```

Let's see what changed:

- <strong>ansible-playbook</strong> is the executable file that we need to call to apply playbooks and roles.
- <strong>production-mariadb.yml</strong> is the play that associates the servers listed in the inventory to their roles.

If we call ansible-playbook with no additional arguments, we will apply all applicable roles to all the servers mentioned in the play.

To only apply roles to certain servers, we can use the `-l` parameter to specify a group or an individual host:

```sql
ansible-playbook -i production-mariadb -l mariadb-main production-mariadb.yml
```

We can also apply tasks from roles selectively. Tasks may optionally have tags, and each tag corresponds to an operation that we may want to run on our remote hosts. For example, a "mariadb" role could have the "timezone-update" tag, to update the contents of the [timezone tables](/kb/en/time-zones/#mysql-time-zone-tables). To only apply the tasks with the "timezone-update" tag, we can use this command:

```sql
ansible-playbook -i production-mariadb --tag timezone-update production-mariadb.yml
```

Using tags is especially useful for database servers. While most of the technologies typically managed by Ansible are stateless (web servers, load balancers, etc.) database servers are not. We must pay special attention not to run tasks that could cause a database server outage, for example destroying its data directory or restarting the service when it is not necessary.

### Check mode

We should always test our playbooks and roles on test servers before applying them to production. However, if test servers and production servers are not exactly in the same state (which means, some facts may differ) it is still possible that applying roles will fail. If it fails in the initial stage, Ansible will not touch the remote hosts at all. But there are cases where Ansible could successfully apply some tasks, and fail to apply another task. After the first failure, ansible-playbook will show errors and exit. But this could leave a host in an inconsistent state.

Ansible has a <em>check mode</em> that is meant to greatly reduce the chances of a failure. When run in check mode, ansible-playbook will read the inventory, the play and roles; it will figure out which tasks need to be applied; then it will connect to target hosts, read facts, and value all the relevant variables. If all these steps succeed, it is unlikely that running ansible-playbook without check mode will fail.

To run ansible-playbook in check mode, just add the `--check` (or `-C`) parameter.

## References

Further documentation can be found in the Ansible website:

- [ansible](https://docs.ansible.com/ansible/latest/cli/ansible.html) tool.
- [ansible-playbook](https://docs.ansible.com/ansible/latest/cli/ansible-playbook.html) tool.
- [Validating tasks: check mode and diff mode](https://docs.ansible.com/ansible/latest/user_guide/playbooks_checkmode.html).

---

Content initially contributed by [Vettabase Ltd](https://vettabase.com/).