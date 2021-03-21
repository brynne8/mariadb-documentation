# Ansible Overview for MariaDB Users

Ansible is a tool to automate servers configuration management. It is produced by Red Hat and it is open source software released under the terms of the GNU GPL.

It is entirely possible to use Ansible to automate MariaDB deployments and configuration. This page contains generic information for MariaDB users who want to learn, or evaluate, Ansible.

For information about how to install Ansible, see [Installing Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html) in Ansible documentation.

## Design Principles

Ansible allows us to write <strong>playbooks</strong> that describe how our servers should be configured. Playbooks are lists of <strong>tasks</strong>.

Tasks are usually <strong>declarative</strong>. You don't explain <em>how</em> to do something, you declare <em>what</em> should be done.

Playbooks are <strong>idempotent</strong>. When you apply a playbook, tasks are only run if necessary.

Here is a task example:

```sql
- name: Install Perl
  package:
    name: perl
    state: present
```

"Install Perl" is just a description that will appear on screen when the task is applied. Then we use the `package` module to declare that a package called "perl" should be installed. When we apply the playbook, if Perl is already installed nothing happens. Otherwise, Ansible installs it.

When we apply a playbook, the last information that appears on the screen is a recap like the following:

```sql
PLAY RECAP ***************************************************************************************************
mariadb-01        : ok=6    changed=2    unreachable=0    failed=0    skipped=0    rescued=0    ignored=0
```

This means that six tasks were already applied (so no action was taken), and two tasks were applied.

As the above example shows, Ansible playbooks are written in YAML.

Modules (like `package`) can be written in any language, as long as they are able to process a JSON input and produce a JSON output. However the Ansible community prefers to write them in Python, which is the language Ansible is written in.

## Concepts

A piece of Ansible code that can be applied to a server is called a <strong>playbook</strong>.

A <strong>task</strong> is the smallest brick of code in a playbook. The name is a bit misleading, though, because an Ansible task should not be seen as "something to do". Instead, it is a minimal description of a component of a server. In the example above, we can see a task.

A task uses a single <strong>module</strong>, which is an interface that Ansible uses to interact with a specific system component. In the example, the module is "package".

A task also has attributes, that describe what should be done with that module, and how. In the example above, "name" and "state" are both tasks. The `state` attribute exists for every module, by convention (though there may be exceptions). Typically, it has at least the "present" and "absent" state, to indicate if an object should exist or not.

Other important code concepts are:

- An <strong>inventory</strong> determines which <strong>hosts</strong> Ansible should be able to deploy. Each host may belong to one or more <strong>groups</strong>. Groups may have <strong>children</strong>, forming a hierarchy. This is useful because it allows us to deploy on a group, or to assign variables to a group.
- A <strong>role</strong> describes the state that a host, or group of hosts, should reach after a deploy.
- A <strong>play</strong> associates hosts or groups to their roles. Each role/group can have more than one role.
- A role is a playbook that describes how certain servers should be configured, based on the logical role they have in the infrastructure. Servers can have multiple roles, for example the same server could have both the "mariadb" and the "mydumper" role, meaning that they run MariaDB and they have mydumper installed (as shown later).
- Tasks can use <strong>variables</strong>. They can affect how a task is executed (for example a variable could be a file name), or even whether a task is executed or not. Variables exist at role, group or host level. Variables can also be passed by the user when a play is applied.
- <strong>Facts</strong> are data that Ansible retrieves from remote hosts before deploying. This is a very important step, because facts may determine which tasks are executed or how they are executed. Facts include, for example, the operating system family or its version. A playbook sees facts as pre-set variables.
- <strong>Modules</strong> implement <strong>actions</strong> that tasks can use. Action examples are <strong>file</strong> (to declare that files and directories must exist) or <strong>mysql_variables</strong> (to declare MySQL/MariaDB variables that need to be set).

#### Example

Let's describe a hypothetical infrastructure to find out how these concepts can apply to MariaDB.

The <strong>inventory</strong> could define the following groups:

- "db-main" for the cluster used by our website. All nodes belong to this group.
- "db-analytics" for our replicas used by data analysts.
- "dump" for one or more servers that take dumps from the replicas.
- "proxysql" for one or more hosts that run ProxySQL.

Then we'll need the following nodes:

- "mariadb-node" for the nodes in "db-main". This role describes how to setup nodes of a cluster using Galera.
- "mariadb-replica" for the members of "db-analytics". It describes a running replica, and it includes the tasks that are necessary to provision the node if the data directory is empty when the playbook is applied. The hostname of the primary server is defined in a variable.
- "mariadb". The aforementioned "mariadb-node" and "mariadb-replica" can be children of this group. They have many things in common (filesystem for the data directory, some basic MariaDB configuration, some installed tools...), so it could make sense to avoid duplication and describe the common traits in a super-role.
- A "mariabackup" role to take backups with [Mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup/), running jobs during the night. We can associate this role to the "db-main" group, or we could create a child group for servers that will take the backups.
- "mariadb-dump" for the server that takes dumps with [mariadb-dump](/clients-utilities/backup-restore-and-import-clients/mysqldump/). Note that we may decide to take dumps on a replica, so the same host may belong to "db-analytics" and "mariadb-dump".
- "proxysql" for the namesake group.

## Architecture

Ansible architecture is extremely simple. Ansible can run on any host. To apply playbooks, it connects to the target hosts and runs system commands. By default the connection happens via ssh, though it is possible to develop connection plugins to use different methods. Applying playbooks locally without establishing a connection is also possible.

Modules can be written in any language, though Python is the most common choice in the Ansible community. Modules receive JSON "requests" and facts from Ansible core, they are supposed to run useful commands on a target host, and then they should return information in JSON. Their output informs Ansible whether something has changed on the remote server and if the operations succeeded.

Ansible is not centralized. It can run on any host, and it is common for a team to run it from several laptops. However, to simplify things and improve security, it may be desirable to run it from a dedicated host. Users will connect to that host, and apply Ansible playbooks.

## Ansible Resources and References

- [Ansible.com](https://www.ansible.com/)
- [Ansible Galaxy](https://galaxy.ansible.com/)
- [Ansible on Wikipedia](https://en.wikipedia.org/wiki/Ansible_(software))
- [Ansible Automation Platform](https://www.youtube.com/c/AnsibleAutomation/videos) YouTube channel
- [Ansible: Getting Started](https://www.ansible.com/resources/get-started)
- [MariaDB Deployment and Management with Ansible](https://youtu.be/CV8-56Fgjc0) (video)

Further information about the concepts discussed in this page can be found in Ansible documentation:

- [Basic Concepts](https://docs.ansible.com/ansible/latest/network/getting_started/basic_concepts.html).
- [Glossary](https://docs.ansible.com/ansible/latest/reference_appendices/glossary.html).

---

Content initially contributed by [Vettabase Ltd](https://vettabase.com/).