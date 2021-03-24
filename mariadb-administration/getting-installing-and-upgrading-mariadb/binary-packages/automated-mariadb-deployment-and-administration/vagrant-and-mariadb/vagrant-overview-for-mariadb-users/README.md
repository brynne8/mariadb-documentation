# Vagrant Overview for MariaDB Users

Vagrant is a tool to create and manage development machines (Vagrant <em>boxes</em>).  They are usually virtual machines on the localhost system, but they could also be Docker containers or remote machines. Vagrant is open source software maintained by HashiCorp and released under the MIT license.

Vagrant benefits include simplicity, and a system to create test boxes that is mostly independent from the technology used.

For information about installing Vagrant, see [Installation](https://www.vagrantup.com/docs/installation) in Vagrant documentation.

In this page we discuss basic Vagrant concepts.

## Vagrant Concepts

A <strong>Vagrant machine</strong> is compiled from a box. It can be a virtual machine, a container or a remote server from a cloud service.

A <strong>box</strong> is a package that can be used to create Vagrant machines. We can download boxes from app.vagrantup.com, or we can build a new box from a Vagrantfile. A box can be used as a base for another box. The base boxes are usually operating system boxes downloaded from app.vagrantup.com.

A <strong>provider</strong> is responsible for providing the virtualization technology that will run our machine.

A <strong>provisioner</strong> is responsible for installing and configuring the necessary software on a newly created Vagrant machine.

### Example

The above concepts are probably easier to understand with an example.

We can use an Ubuntu box as a base to build a Vagrant machine with MariaDB. So we write a Vagrantfile for this purpose. In the Vagrantfile we specify VirtualBox as a provider. And we use the Ansible provisioner to install and configure MariaDB. Once we finish this Vagrantfile, we can run a Vagrant command to start a Vagrant machine, which is actually a VirtualBox VM running MariaDB on Ubuntu.

The following diagram should make the example clear:

<img src="/kb/en/vagrant-overview-for-mariadb-users/+image/vagrant-virtalbox-ansible" alt="vagrant-virtalbox-ansible" title="vagrant-virtalbox-ansible">

### Vagrantfiles

A Vagrantfile is a file that describes how to create one or more Vagrant machines. Vagrantfiles use the Ruby language, as well as objects provided by Vagrant itself.

A Vagrantfile is often based on a box, which is usually an operating system in which we are going to install our software. For example, one can create a MariaDB Vagrantfile based on the `ubuntu/trusty64` box. A Vagrantfile can describe a box with a single server, like MariaDB, but it can also contain a whole environment, like LAMP. For most practical use cases, having the whole environment in a single box is more convenient.

Boxes can be searched in [Vagrant Cloud](https://app.vagrantup.com/boxes/search). Most of their Vagrantfiles are available on GitHub. Searches can be made, among other things, by keyword to find a specific technology, and by provider.

### Providers

A provider adds support for creating a specific type of machines. Vagrant comes with several providers, for example:

- VirtualBox allows one to create virtual machines with VirtualBox.
- Hyper-V allows one to create virtual machines with Microsoft Hyper-V.
- [Docker](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/docker-and-mariadb/) allows one to create Docker containers.

Alternative providers are maintained by third parties or sold by HashiCorp. They allow one to create different types of machines, for example using VMWare.

Some examples of useful providers, recognized by the community:

- [Vagrant AWS Provider](https://github.com/mitchellh/vagrant-aws).
- [Vagrant Google Compute Engine (GCE) Provider](https://github.com/mitchellh/vagrant-google).
- [Vagrant Azure Provider](https://github.com/Azure/vagrant-azure).
- [OpenVZ](https://app.vagrantup.com/OpenVZ).
- [vagrant-lxc](https://github.com/fgrehm/vagrant-lxc).

If you need to create machines with different technologies, or deploy them to unsupported cloud platforms, you can develop a custom provider in Ruby language. To find out how, see [Plugin Development: Providers](https://www.vagrantup.com/docs/plugins/providers) in Vagrant documentation. The [Vagrant AWS](https://github.com/mitchellh/vagrant-aws) Provider was initially written as an example provider.

### Provisioners

A provisioner is a technology used to deploy software to the newly created machines.

The simplest provisioner is `shell`, which runs a shell file inside the Vagrant machine. `powershell` is also available.

Other providers use automation software to provision the machine. There are provisioners that allow one to use [Ansible](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/ansible-and-mariadb/), [Puppet](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/automated-mariadb-deployment-and-administration-puppet-and-mariadb/), Chef or Salt. Where relevant, there are different provisioners allowing the use of these technologies in a distributed way (for example, using Puppet apply) or in a centralized way (for example, using a Puppet server).

It is interesting to note that there is both a Docker provider and a Docker provisioner. This means that a Vagrant machine can be a Docker container, thanks to the `docker` provisioner. Or it could be any virtualisation technology with Docker running in it, thanks to the `docker` provisioner. In this case, Docker pulls images and starts containers to run the software that should be running in the Vagrant machine.

If you need to use an unsupported provisioning method, you can develop a custom provisioner in Ruby language. See [Plugin Development: Provisioners](https://www.vagrantup.com/docs/plugins/provisioners) in Vagrant documentation.

## Vagrant commands

This is a list of the most common Vagrant commands. For a complete list, see [Command-Line Interface](https://www.vagrantup.com/docs/cli) in Vagrant documentation.

To list the available machines:

```sql
vagrant box list
```

To start a machine from a box:

```sql
cd /box/directory
vagrant up
```

To connect to a machine:

```sql
vagrant ssh
```

To see all machines status and their id:

```sql
vagrant global-status
```

To destroy a machine:

```sql
vagrant destroy <id>
```

## Vagrant Resources and References

Here are some valuable websites and pages for Vagrant users.

- [Vagrant Up](https://www.vagrantup.com/).
- [app.vagrantup.com](https://app.vagrantup.com/).
- [Vagrant Community](https://www.vagrantup.com/community).
- [Vagrant on Wikipedia](https://en.wikipedia.org/wiki/Vagrant_(software)).
- [Vagrant on HashiCorp Learn](https://learn.hashicorp.com/vagrant).

---

Content initially contributed by [Vettabase Ltd](https://vettabase.com/).