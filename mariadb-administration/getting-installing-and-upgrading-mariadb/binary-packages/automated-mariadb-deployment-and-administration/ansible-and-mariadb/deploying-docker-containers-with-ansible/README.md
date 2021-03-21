# Deploying Docker Containers with Ansible

Ansible can be used to manage Docker container upgrades and configuration changes. Docker has native ways to do this, namely [Dockerfiles](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/docker-and-mariadb/creating-a-custom-docker-image) and [Docker Compose](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/docker-and-mariadb/setting-up-a-lamp-stack-with-docker-compose). But sometimes there are reasons to start basic containers from an image and then manage configuration with Ansible or similar software. See [Benefits of Managing Docker Containers with Automation Software](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/docker-and-mariadb/benefits-of-managing-docker-containers-with-orchestration-software).

In this page we'll discuss how to use Ansible to manage Docker containers.

## How to Deploy a Container with Ansible

Ansible has modules to manage the Docker server, Docker containers, and Docker Compose. These modules are maintained by the community.

A dynamic inventory plugin for Docker exists. It retrieves the list of existing containers from Docker.

Docker modules and the Docker inventory plugin communicate with Docker using its API. The connection to the API can use a TSL connection and supports key authenticity verification.

To communicate with Docker API, Ansible needs a proper Python module installed on the Ansible node (`docker` or `docker-py`).

Several roles exist to deploy Docker and configure it. They can be found in Ansible Galaxy.

## References

Further information can be found in Ansible documentation.

- [Docker Guide](https://docs.ansible.com/ansible/latest/scenario_guides/guide_docker.html).
- [docker_container](https://docs.ansible.com/ansible/latest/collections/community/general/docker_container_module.html) module.

---

Content initially contributed by [Vettabase Ltd](https://vettabase.com/).