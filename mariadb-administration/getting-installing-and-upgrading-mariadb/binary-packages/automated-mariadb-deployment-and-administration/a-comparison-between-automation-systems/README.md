# A Comparison Between Automation Systems

This page compares the automation systems that are covered by this section of the MariaDB Knowledge Base. More information about these systems are presented in the relevant pages, and more systems may be added in the future.

## Code Structure Differences

Different automation systems provide different ways to describe our infrastructure. Understanding how they work is the first step to evaluate them and choose one for our organization.

### Ansible Code Structure

Ansible code consists of the following components:

- An <strong>inventory</strong> determines which <strong>hosts</strong> Ansible should be able to deploy. Each host may belong to one or more <strong>groups</strong>. Groups may have <strong>children</strong>, forming a hierarchy. This is useful because it allows us to deploy on a group, or to assign variables to a group.
- A <strong>role</strong> describes the state that a host, or group of hosts, should reach after a deploy.
- A <strong>play</strong> associates hosts or groups to their roles. Each role/group can have more than one role.
- A role consists of a list of <strong>tasks</strong>. Despite its name a task is not necessarily something to do, but something that must exist in a certain state.
- Tasks can use <strong>variables</strong>. They can affect how a task is executed (for example a variable could be a file name), or even whether a task is executed or not. Variables exist at role, group or host level. Variables can also be passed by the user when a play is applied.
- <strong>Playbooks</strong> are the code that is used to define tasks and variables.
- <strong>Facts</strong> are data that Ansible retrieves from remote hosts before deploying. This is a very important step, because facts may determine which tasks are executed or how they are executed. Facts include, for example, the operating system family or its version. A playbook sees facts as pre-set variables.
- <strong>Modules</strong> implement <strong>actions</strong> that tasks can use. Action examples are <strong>file</strong> (to declare that files and directories must exist) or <strong>mysql_variables</strong> (to declare MySQL/MariaDB variables that need to be set).

See [Ansible Overview - concepts](/kb/en/ansible-overview/#concepts) for more details and an example.

### Puppet Code Structure

Puppet code consists of the following components:

- An <strong>inventory file</strong> defines a set of <strong>groups</strong> and their <strong>targets</strong> (the members of a group). <strong>plugins</strong> can be used to retrieve groups and target dynamically, so they are equivalent to Ansible dynamic inventories.
- A <strong>manifest</strong> is a file that describes a configuration.
- A <strong>resource</strong> is a component that should run on a server. For example, "file" and "service" are existing support types.
- An <strong>attribute</strong> relates to a resource and affects the way it is applied. For example, a resource of type "file" can have attributes like "owner" and "mode".
- A <strong>class</strong> groups resources and variables, describing a logical part of server configuration. A class can be associated to several servers. A class is part of a manifest.
- A <strong>module</strong> is a set of manifests and describes an infrastructure or a part of it.
- Classes can have typed <strong>parameters</strong> that affect how they are applied.
- <strong>Properties</strong> are variables that are read from the remote server, and cannot be arbitrarily assigned.
- <strong>Facts</strong> are pre-set variables collected by Puppet before applying or compiling a manifest.

## Architectural Differences

The architecture of the various  systems is different. Their architectures determine how a deploy physically works, and what is needed to be able to deploy.

### Ansible Architecture

Ansible architecture is simple. Ansible can run from any host, and can apply its playbooks on remote hosts. To do this, it runs commands via SSH. In practice, in most cases the commands will be run as superuser via `sudo`, though this is not always necessary.

Inventories can be dynamic. In this case, when we apply a playbook Ansible connects to remote services to discover hosts.

Ansible playbooks are applied via the `ansible-playbook` binary. Changes to playbooks are only applied when we perform this operation.

To recap, Ansible does not need to be installed on the server is administers. It needs an SSH access, and normally its user needs to be able to run `sudo`. It is also possible to configure a dynamic inventory, and a remote service to be used for this purpose.

### Puppet Architecture

Puppet supports two types of architecture: agent-master or standalone. The agent-master architecture is recommended by Puppet Labs, and it is the most popular among Puppet users. For this reason, those who prefer a standalone architecture tend to prefer Ansible.

#### Agent-Master Architecture

When this architecture is chosen, manifests are sent to the <strong>Puppet master</strong>. There can be more than one master, for high availability reasons. All target hosts run a <strong>Puppet agent</strong>. Normally this is a service that automatically starts at system boot. The agent contacts a master at a given interval. It sends facts, and uses them to compile a <strong>catalog</strong> from the manifests. A catalog is a description of what exactly an individual server should run. The agent receives the catalog and checks if there are differences between its current configuration and the catalog. If differences are found, the agent applies the relevant parts of the catalog.

An optional component is <strong>PuppetDB</strong>. This is a central place where some data are stored, including manifests, retrieved facts and logs. PuppetDB is based on PostgreSQL and there are no plans to support MariaDB or other DBMSs.

If a manual change is made to a remove server, it will likely be overwritten the next time Puppet agent runs. To avoid this, the Puppet agent service can be stopped.

#### Standalone Architecture

As mentioned, this architecture is not recommended by Puppet Labs nor popular amongst Puppet users. It is similar to Ansible architecture.

Users can apply manifests from any host with Puppet installed. This could be their laptop but, in order to emulate the behavior of an agent-master architecture, normally Puppet runs on a dedicated node as a cronjob. The <strong>Puppet apply</strong> application will require facts from remote hosts, it will compile a catalog for each host, will check which parts of it need to be applied, and will apply them remotely.

If a manual change is made to a remove server, it will be overwritten the next time Puppet apply runs. To avoid this, comment out any cron job running Puppet apply, or comment out the target server in the inventory.

#### Inventory

As mentioned, Puppet supports plugins to retrieve the inventory dynamically from remote services. In an agent-master architecture, one has to make sure that each target host has access to these services. In a standalone architecture, one has to make sure that the hosts running Puppet apply have access to these services.

## Storing Secrets

Often our automation repositories need to contain secrets, like MariaDB user passwords or private keys for SSH authentication.

Both Ansible and Puppet support integration with secret stores, like Hashicorp Vault. For Puppet integration, see [Integrations with secret stores](https://puppet.com/docs/puppet/6.17/integrations_with_secret_stores.html).

In the simplest case, Ansible allows encrypting secrets in playbooks and decrypting them during execution using [ansible-vault](https://docs.ansible.com/ansible/latest/user_guide/vault.html). This implies a minimal effort to handle secrets. However, it is not the most secure way to store secrets. The passwords to disclose certain secrets need to be shared with the users who have the right to use them. Also, brute force attacks are possible.

## Ecosystems and Communities

Automation software communities are very important, because they make available a wide variety of modules to handle specific software.

### Ansible Ecosystem

Ansible is open source, released under the terms of the GNU GPL. It is produced by RedHat. RedHat has a page about [Red Hat Ansible Automation Platform Partners](https://www.ansible.com/partners), who can provide support and consulting.

[Ansible Galaxy](https://galaxy.ansible.com/) is a big repository of Ansible roles produced by both the vendor and the community. Ansible comes with `ansible-galaxy`, a tool that can be used to create roles and upload them to Ansible Galaxy.

At the time of this writing, Ansible does not have specific MariaDB official modules. MySQL official modules can be used. However, be careful not try to use features that only apply to MySQL. There are several community-maintained MariaDB roles.

### Puppet Ecosystem

Puppet is open source, released under the GNU GPL. It is produced by a homonym company. The page [Puppet Partners](https://puppet.com/partners/) lists partners that can provide support and consulting about Puppet.

[Puppet Forge](https://forge.puppet.com/) is a big repository of modules produced by the vendor and by the community, as well as how-to guides.

Currently Puppet has many MariaDB modules.

## See Also

For more information about the systems mentioned in this page, from MariaDB users perspective:

- [Ansible and MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/ansible-and-mariadb/).
- [Puppet and MariaDB](/kb/en/automated-mariadb-deployment-and-administration-puppet-and-mariadb/).

---

Content initially contributed by [Vettabase Ltd](https://vettabase.com/).