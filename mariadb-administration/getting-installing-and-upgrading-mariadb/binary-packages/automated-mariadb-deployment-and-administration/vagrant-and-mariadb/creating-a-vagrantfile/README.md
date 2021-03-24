# Creating a Vagrantfile

In this page we discuss how to create a Vagrantfile, which you can use to create new boxes. This content is specifically written to address the needs of MariaDB users.

## A Basic Vagrantfile

A Vagrantfile is a Ruby file that instructs Vagrant to create one or more machines. Here is a simple example:

```sql
Vagrant.configure("2") do |config|
  config.vm.box = "hashicorp/bionic64"
  config.vm.provider "virtualbox"
  config.vm.provision :shell, path: "bootstrap.sh"
end
```

`Vagrant.configure("2")` returns the Vagrant configuration object for the new box. In the block, we'll use the `config` alias to refer this object. We are going to use version 2 of Vagrant API.

`vm.box` is the base box that we are going to use. It is Ubuntu BionicBeaver (18.04 LTS), 64-bit version, provided by HashiCorp. The schema for box names is simple: the maintainer account in [Vagrant Cloud](https://app.vagrantup.com/boxes/search) followed by the box name.

We use `vm.provision` to specify the name of the file that is going to be executed at box creation, to provision the machine. `bootstrap.sh` is the conventional name used in most cases.

## Providers

A provider allows Vagrant to create a Vagrant machine using a certain technology. Different providers may enable a virtual machine manager ([VirtualBox](https://www.vagrantup.com/docs/providers/virtualbox), [VMWare](https://www.vagrantup.com/docs/providers/vmware), [Hyper-V](https://www.vagrantup.com/docs/providers/hyperv)...), a container manager ([Docker](https://www.vagrantup.com/docs/providers/docker)), or remote cloud hosts ([AWS](https://github.com/mitchellh/vagrant-aws), [Google Compute Engine](https://github.com/mitchellh/vagrant-google)...).

Some providers are developed by third parties. [app.vagrant.com](https://app.vagrantup.com/) supports search for boxes that support the most important third parties providers. To find out how to develop a new provider, see [Plugin Development: Providers](https://www.vagrantup.com/docs/plugins/providers).

Provider options can be specified. Options affect the type of Vagrant machine that is created, like the number of virtual CPUs. Different providers support different options.

It is possible to specify multiple providers. In this case, Vagrant will try to use them in the order they appear in the Vagrantfile. It will try the first provider; if it is not available it will try the second; and so on.

Here is an example of providers usage:

```sql
Vagrant.configure("2") do |config|
    config.vm.box = "hashicorp/bionic64"
    config.vm.provider "virtualbox" do |vb|
        vb.customize ["modifyvm", :id, "--memory", 1024 * 4]
    end
    config.vm.provider "vmware_fusion"
end
```

In this example, we try to use VirtualBox to create a virtual machine. We specify that this machine must have 4G of RAM (1024M * 4). If VirtualBox is not available, Vagrant will try to use VMWare.

This mechanism is useful for at least a couple of reasons:

- Different users may use different systems, and maybe they don't have the same virtualization technologies installed.
- We can gradually move from one provider to another. For a period of time, some users will have the new virtualization technology installed, and they will use it; other users will only have the old technology installed, but they will still be able to create machines with Vagrant.

## Provisioners

We can use different methods for provisioning. The simplest provisioner is `shell`, that allows one to run a Bash file to provision a machine. Other provisioners allow setting up the machines using automation software, including Ansible, Puppet, Chef and Salt.

To find out how to develop a new provisioner, see [Plugin Development: Provisioners](https://www.vagrantup.com/docs/plugins/provisioners).

### The `shell` Provisioner

In the example above, the [shell](https://www.vagrantup.com/docs/provisioning/shell) provisioner runs boostrap.sh inside the Vagrant machine to provision it. A simple bootstrap.sh may look like the following:

```sql
#!/bin/bash

apt-get update
apt-get install -y 
```

To find out the steps to install MariaDB on your system of choice, see the [Getting, Installing, and Upgrading MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/) section.

You may also want to restore a database backup in the new box. In this way, you can have the database needed by the application you are developing. To find out how to do it, see [Backup and Restore Overview](/mariadb-administration/backing-up-and-restoring-databases/backup-and-restore-overview/). The most flexible type of backup (meaning that it works between different MariaDB versions, and in some cases even between MariaDB and different DBMSs) is a [dump](/clients-utilities/backup-restore-and-import-clients/mysqldump/).

On Linux machines, the `shell` provisioner uses the default shell. On Windows machines, it uses PowerShell.

### Uploading Files

If we use the `shell` provisioner, we need a way to upload files to the new machine when it is created. We could use the `file` provisioner, but it works by connecting the machine via ssh, and the default user doesn't have permissions for any directory except for the synced folders. We could change the target directory owner, or we could add the default user to a group with the necessary privileges, but these are not considered good practices.

Instead, we can just put the file we need to upload somewhere in the synced folder, and then copy it with a shell command:

```sql
cp ./files/my.cnf /etc/mysql/conf.d/
```

### Provisioning Vagrant with Ansible

Here is an example of how to provision a Vagrant box by running Ansible:

```sql
Vagrant.configure("2") do |config|
  ...
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "vagrant.yml"
  end
end
```

With the [Ansible provisioner](https://www.vagrantup.com/docs/provisioning/ansible), Ansible runs in the host system. In this example, it runs a playbook called `vagrant.yml`. The [Ansible Local provisioner](https://www.vagrantup.com/docs/provisioning/ansible_local) runs the playbook in the vagrant machine.

For an introduction to Ansible for MariaDB users, see [Ansible and MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/ansible-and-mariadb/).

### Provisioning Vagrant with Puppet

To provision a Vagrant box by running Puppet:

```sql
Vagrant.configure("2") do |config|
  ...
  config.vm.provision "puppet" do |puppet|
    puppet.manifests_path = "manifests"
    puppet.manifest_file = "default.pp"
  end
end
```

In this example, Puppet Apply runs in the host system and no Puppet Server is needed. Puppet expects to find a `manifests` directory in the project directory. It expects it to contain `default.pp`, which will be used as an entry point. Note that `puppet.manifests_path` and `puppet.manifest_file` are set to their default values.

Puppet needs to be installed in the guest machine.

To use a Puppet server, the `puppet_server` provisioner can be used:

```sql
Vagrant.configure("2") do |config|
  ...
  config.vm.provision "puppet_server" do |puppet|
    puppet.puppet_server = "puppet.example.com"
  end
end
```

See the [Puppet Apply provisioner](https://www.vagrantup.com/docs/provisioning/puppet_apply) and the [Puppet Agent Provisioner](https://www.vagrantup.com/docs/provisioning/puppet_agent).

For an introduction to Puppet for MariaDB users, see [Puppet and MariaDB](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/automated-mariadb-deployment-and-administration-puppet-and-mariadb/).

## Sharing Files Between the Host System and a Box

To restore a backup into MariaDB, in most cases we need to be able to copy it from the host system to the box. We may also want to occasionally copy MariaDB logs from the box to the host system, to be able to investigate problems.

The project directory (the one that contains the Vagrantfile) by default is shared with the virtual machine and mapped to the `/vagrant` directory (the synced folder). It is a good practice to put there all files that should be shared with the box when it is started. Those files should normally be versioned.

The synced folder can be changed. In the above example, we could simply add one line:

```sql
config.vm.synced_folder "/host/path", "/guest/path"
```

The synced folder can also be disabled:

```sql
config.vm.synced_folder '.', '/vagrant', disabled: true
```

## References

The [vagrant-mariadb-examples](https://github.com/Vettabase/vagrant-mariadb-examples) repository is an example of a Vagrantfile that creates a box containing MariaDB and some useful tools for developers.

Further information can be found in Vagrant documentation.

- [Vagrantfile](https://www.vagrantup.com/docs/vagrantfile).
- [Providers](https://www.vagrantup.com/docs/providers).
- [Synced Folders](https://www.vagrantup.com/docs/synced-folders).
- [Ansible Provisioner](https://www.vagrantup.com/docs/provisioning/ansible).
- [Puppet Apply Provisioner](https://www.vagrantup.com/docs/provisioning/puppet_apply).
- [Puppet Agent Provisioner](https://www.vagrantup.com/docs/provisioning/puppet_agent).

See also [Ruby documentation](http://www.ruby-lang.org/en/documentation/).

---

Content initially contributed by [Vettabase Ltd](https://vettabase.com/).