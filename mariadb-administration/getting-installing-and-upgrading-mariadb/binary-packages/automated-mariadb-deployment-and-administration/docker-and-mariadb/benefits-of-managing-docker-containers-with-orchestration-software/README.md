# Benefits of Managing Docker Containers with Orchestration Software

In this page we'll discuss why automating [Docker](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/docker-and-mariadb) containers with software like [Ansible](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/ansible-and-mariadb) or [Puppet](/kb/en/automated-mariadb-deployment-and-administration-puppet-and-mariadb/) may be desirable in some cases. To talk about this, we'll first need to discuss why Docker containers are defined <em>ephemeral</em>, and how this applies to containerized database servers (particularly MariaDB).

During the discussion, we should keep in mind that Docker can be used to setup production and/or development environments. These use cases are very different from a database perspective: a production database may be big, and typically contains data that we don't want to lose. Development environments usually contain small sample data that can be rebuilt relatively quickly. This page focuses on the latter case.

## Docker's Ephemeral Nature

Docker images are compiled from Dockerfiles. Containers are created from images. Normally, a container is not modified from the moment it is created. In other words, containers are usually designed to be <strong>ephemeral</strong>, meaning that they can be destroyed and replaced with new containers at any time. Provided that there is proper redundancy (for example, there are several web servers running the same services) destroying one container and starting a new one of the same type won't cause any damage.

We will discuss a bit later how this applies to MariaDB, and more generally to database servers.

When something should change, for example some software version or configuration, normally Dockerfiles are updated and containers are recreated from the latest image versions. For this reason, containers shouldn't contain anything that shouldn't be lost, and recreating them should be an extremely cheap operation. <strong>Docker Compose</strong> or the <strong>Swarm mode</strong> are used to declare which containers form a certain environment, and how they communicate with each other.

On the contrary, Ansible and Puppet are mainly built to manage the configuration of existing servers. It doesn't recreate servers, it changes their configuration. So Docker and Ansible have very different approaches. For this reason, Ansible and Puppet are not frequently used to deploy containers to production. However, using them together can bring some benefits, especially for development environments.

More on this later in the page. First, we need to understand how these concepts apply to database servers.

## Stateful Technologies

Using ephemeral containers works very well for <em>stateless</em> technologies, like web servers and proxies. These technologies virtually only need binaries, configuration and small amounts of data (web pages). If some data need to be restored after a container creation, it will be a fast operation.

In the case of a database, the problem is that data can be large and need to be written somewhere. We don't want all databases to disappear when we destroy a container. Even if we had an up-to-date backup, restoring it would take time.

However, Docker has features called <strong>volumes</strong> and <strong>volume containers</strong>. We won't discuss the difference here, let's focus on their purpose. A volume is a directory in the host system mapped to a directory in one or more containers. Volumes are not destroyed when containers are destroyed. They can be used to share data between any number of containers and the host system. Therefore, they are also a good way to persist data.

Suppose a MariaDB container called `mariadb-main-01` uses a volume that is mapped to `/var/docker/volumes/mariadb-main`. At some point we want to use a more recent MariaDB version. As explained earlier, the Docker way to do this is to destroy the container and create a new one that uses a more recent version of the MariaDB image.

So, we will destroy `mariadb-main-01`. The volume is still there. Then we create a new container with the same name, but based on a newer image. We make sure to link the volume to the new container too, so it will be able to use `/var/docker/volumes/mariadb-main` again. At this point we may want to run [mysql_upgrade](/sql-statements-structure/sql-statements/table-statements/mysql_upgrade), but apart from that, everything should <em>just work</em>.

The above described steps are simple, but running them manually is time consuming and error-prone. Automating them with some automation software like Ansible or Puppet is often desirable.

## Ways to Deploy Docker Containers

Docker containers can be deployed in the following ways:

- Manually. See [Installing and Using MariaDB via Docker](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/docker-and-mariadb/installing-and-using-mariadb-via-docker). This is not recommended for production, or for complex environments. However, it can easily be done for the simplest cases. If we want to make changes to our [custom images](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/docker-and-mariadb/creating-a-custom-docker-image), we'll need to modify the Dockerfiles, destroy the containers and recreate them.
- With Docker Compose. See [Setting Up a LAMP Stack with Docker Compose](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/docker-and-mariadb/setting-up-a-lamp-stack-with-docker-compose) for a simple example. When we modify a Dockerfile, we'll need to destroy the containers and recreate them, which is usually as simple as running `docker-compose down` followed by `docker-compose-up`. After changing `docker-cmpose.yml` (maybe to add a container or a network) we'll simply need to run `docker-compose-up` again, because it is idempotent.
- Using Ansible, Puppet or other automation software, as mentioned before. We can use Ansible or Puppet to create the containers, and run them again every time we want to apply some change to the containers. This means that the containers are potentially created once and modified any number of times.

In all these cases, it is entirely possible to add [Vagrant](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/vagrant-and-mariadb) to the picture. Vagrant is a way to deploy or provision several hosts, including virtual machines (the most common case), and containers. It is agnostic in regarding the underlying technology, so it can deploy to a virtual machine, a container, or even a remote server in the same way. Docker can work with Vagrant in two ways:

- As a [provisioner](/kb/en/creating-a-vagrantfile/#provisioners). In this case Vagrant will most commonly deploy a virtual machine, and will use Docker to setup the applications that need to run in it, as containers. This guarantees a higher level of isolation, compared to running the containers in the local host. Especially if you have different environments to deploy locally, because you can have them on different virtual machines.
- As a [provider](/kb/en/creating-a-vagrantfile/#providers). Vagrant will deploy one or more Docker containers locally. Once each container is up, Vagrant can optionally use a provisioner on it, to make sure that the container runs the proper software with proper configuration. In this case, Ansible, Puppet or other automation software can be used as a provisioner. But again, this is optional: it is possible to make changes to the Dockerfiles and recreate the containers every time.

## Benefits of Managing Docker Containers with Automation Software

Docker containers can be entirely managed with Docker Compose or the Swarm mode. This is often a good idea.

However, choosing to use automation software like Ansible or Puppet has some benefits too. Benefits include:

- Docker containers allow working without modifying the host system, and their creation is very fast. Much faster than virtual machines. This makes Docker desirable for development environments.
- As explained, making all containers ephemeral and using volumes to store important data is possible. But this means adding some complexity to adapt an ephemeral philosophy to technologies that are not ephemeral by nature (databases). Also, many database professionals don't like this approach. Using automation software allows easily triggering upgrades and configuration changes in the containers, treating them as non-ephemeral systems.
- Sometimes Docker is only used in development environments. If production databases are managed via Ansible, Puppet, or other automation software, this could lead to some code duplication. Dealing with configuration changes using the same procedures will reduce the cost of maintenance.
- While recreating containers is fast, being able to apply small changes with Ansible or Puppet can be more convenient in some cases: particularly if we write files into the container itself, or if recreating a container bootstrap involves some lengthy procedure.
- Trying to do something non-standard with Dockerfiles can be tricky. For example, running two processes in a container is possible but can be problematic, as Docker is design to run a process per container. However there are situations when this is desirable. For example PMM containers run several different processes. Launching additional processes with Ansible or Puppet may be easier than doing it with a Dockerfile.

With all this in mind, let's see some examples of cases when managing Docker containers with Ansible, Puppet or other automation software is preferable, rather than destroying containers every time we want to make a change:

- We use Ansible or Puppet in production, and we try to keep development environments as similar as possible to production. By using Ansible/Puppet in development too, we can reuse part of the code.
- We make changes to the containers often, and recreating containers is not as fast as it should be (for example because a MariaDB [dump](/clients-utilities/backup-restore-and-import-clients/mysqldump) needs to be restored).
- Creating a container implies some complex logic that does not easily fit a Dockerfile or Docker Compose (including, but not limited to, running multiple processes per container).

That said, every case is different. There are environments where these advantages do not apply, or bring a very small benefit. In those cases, the cost of adding some automation with Ansible, Puppet or similar software is probably not justified.

## See Also

See these pages on how to manage Docker containers with different automation technologies:

- [Deploying Docker Containers with Ansible](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/ansible-and-mariadb/deploying-docker-containers-with-ansible).
- [Deploying Docker Containers with Puppet](/kb/en/deploying-docker-containers-with-puppet/).

---

Content initially contributed by [Vettabase Ltd](https://vettabase.com/).