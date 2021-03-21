# Docker Security Concerns

When using Docker containers in production, it is important to be aware of Docker security concerns.

## Host System Security

All Docker containers are built upon the host system's kernel. If the host system's kernel has security bugs, those bugs are also present in the containers.

In particular, Docker leverages two Linux features:

- Namespaces, to isolate containers from each other and make sure that a container can't establish unauthorized connections to another container.
- cgroups, to limit the resources (CPU, memory, IO) that each container can consume.

The administrators of a system running Docker should be particularly careful to upgrade the kernel whenever security bugs to these features are fixed.

Docker, like most container technologies, uses the runC open source library. runC security bugs are likely to affect Docker.

Finally, Docker's own security bugs potentially affect all containers.

It is important to note that when we upgrade the kernel, runC or Docker itself we cause downtime for all the containers running on the system.

## Images Security

Docker containers are built from images. If security is a major concern, you should make sure that the images you use are secure.

If you want to be sure that you are pulling authentic images, you should only pull images signed with [Docker Content Trust](/kb/en/creating-a-custom-docker-image/#docker-content-trust).

The images should create and use a system user with less privileges than root. For example, the MariaDB daemon usually runs as a user called `mysql`, who belongs the `mysql` group. There is no need to run it as root (and by default it will refuse to start as root). Containers should not run it as root. Using an unprivileged user will reduce the chances that a bug in Docker or in the kernel will allow the user to gain access to the host system.

Updated images should be used. An image usually downloads packages information at build time. If the image is not recently built, a newly created container will have old packages. Updating the packages on container creation and regularly re-updating them will ensure that the container uses packages with the most recent versions. Rebuilding an image often will reduce the time necessary to update the packages the first time.

Security bugs are usually important for a database server, so you don't want your version of MariaDB to contain known security bugs. But suppose you also have a bug in Docker, in runC, or in the kernel. A bug in a user-facing application may allow an attacker to exploit a bug in those lower level technologies. So, after gaining access to the container, an attacker may gain access to the host system. This is why system administrators should keep both the host system and the software running in the containers updated.

## References

For more information, see the following links:

- [Docker security](https://docs.docker.com/engine/security/) on Docker documentation.
- [Linux namespaces](https://en.wikipedia.org/wiki/Linux_namespaces) on Wikipedia.
- [cgroups](https://en.wikipedia.org/wiki/Cgroups) on Wikipedia.
- [runC repository](https://github.com/opencontainers/runc).

---

Content initially contributed by [Vettabase Ltd](https://vettabase.com/).