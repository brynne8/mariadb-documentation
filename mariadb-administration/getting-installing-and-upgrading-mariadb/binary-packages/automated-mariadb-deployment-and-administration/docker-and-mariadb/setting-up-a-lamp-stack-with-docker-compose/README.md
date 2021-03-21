# Setting Up a LAMP Stack with Docker Compose

Docker Compose is a tool that allows one to declare which Docker containers should run, and which relationships should exist between them. It follows the <strong>infrastructure as code</strong> approach, just like most automation software and Docker itself.

For information about installing Docker Compose, see [Install Docker Compose](https://docs.docker.com/compose/install/) in Docker documentation.

## The `docker-compose.yml` File

When using Docker Compose, the Docker infrastructure must be described in a YAML file called `docker-compose.yml`.

Let's see an example:

```sql
version: "3"

services:
  web:
    image: "apache:${PHP_VERSION}"
    restart: 'always'
    depends_on:
      - mariadb
    restart: 'always'
    ports:
      - '8080:80'
    links:
      - mariadb
  mariadb:
    image: "mariadb:${MARIADB_VERSION}"
    restart: 'always'
    volumes: 
      - /var/lib/mysql/data:${MARIADB_DATA_DIR
      - /var/lib/mysql/logs:${MARIADB_LOG_DIR
      - /var/docker/mariadb/conf:/etc/mysql
    environment:
      MYSQL_ROOT_PASSWORD: ${MYSQL_ROOT_PASSWORD}
      MYSQL_DATABASE: ${MYSQL_DATABASE}
      MYSQL_USER: ${MYSQL_USER}
      MYSQL_PASSWORD: ${MYSQL_PASSWORD}
```

In the first line we declare that we are using version 3 of the Docker compose language.

Then we have the list of services, namely the `web` and the `mariadb` services.

Let's see the properties of the services:

- `port` maps the 8080 container port to the 80 host system port. This is very useful for a development environment, but not in production, because it allows us to connect our browser to the containerized web server. Normally there is no need to connect to MariaDB from the host system.
- `links` declares that this container must be able to connect `mariadb`. The hostname is the container name.
- `depends_on` declares that `mariadb` needs to start before `web`. This is because we cannot do anything with our application until MariaDB is ready to accept connections.
- `restart: always` declares that the containers must restart if they crash.
- `volumes` creates volumes for the container if it is set in a service definition, or a volume that can be used by any container if it is set globally, at the same level as `services`. Volumes are directories in the host system that can be accessed by any number of containers. This allows destroying a container without losing data.
- `environment` sets environment variables inside the container. This is important because in setting these variables we set the MariaDB root credentials for the container.

### About Volumes

It is good practice to create volumes for:

- The [data directory](/kb/en/server-system-variables/#datadir), so we don't lose data when a container is created or replaced, perhaps to upgrade MariaDB.
- The directory where we put all the logs, if it is not the datadir.
- The directory containing all configuration files (for development environments), so we can edit those files with the editor installed in the host system. Normally no editor is installed in containers. In production we don't need to do this, because we can copy files from a repository located in the host system to the containers.

Note that Docker Compose variables are just placeholders for values. Compose does not support assignment, conditionals or loops.

### Using Variables

In the above example you can see several variables, like `${MARIADB_VERSION}`. Before executing the file, Docker Compose will replace this syntax with the `MARIADB_VERSION` variable.

Variables allow making Docker Compose files more re-usable: in this case, we can use any MariaDB image version without modifying the Docker Compose file.

The most common way to pass variables is to write them into a file. This has the benefit of allowing us to version the variable file along with the Docker Compose file. It uses the same syntax you would use in BASH:

```sql
PHP_VERSION=8.0
MARIADB_VERSION=10.5
...
```

For bigger setups, it could make sense to use different environment files for different services. To do so, we need to specify the file to use in the Compose file:

```sql
services:
  web:
    env_file:
      - web-variables.env
...
```

## Docker Compose Commands

Docker Compose is operated using `docker-compose`. Here we'll see the most common commands. For more commands and for more information about the commands mentioned here, see the documentation.

Docker Compose assumes that the Composer file is located in the current directory and it's called `docker-compose.yml`. To use a different file, the `-f &lt;filename&gt;`&nbsp;parameter must be specified.

To pull the necessary images:

```sql
docker-compose pull
```

Containers described in the Compose file can be created in several ways.

To create them only if they do not exist:

```sql
docker-compose up --no-recreate
```

To create them if they do not exist and recreate them if their image or configuration have changed:

```sql
docker-compose up
```

To recreate containers in all cases:

```sql
docker-compose up --force-recreate
```

Normally `docker-compose up` starts the containers. To create them without starting them, add the `--no-start` option.

To restart containers without recreating them:

```sql
docker-compose restart
```

To kill a container by sending it a `SIGKILL`:

```sql
docker-compose kill <service>
```

To instantly remove a running container:

```sql
docker-compose rm -f <serice>
```

To tear down all containers created by the current Compose file:

```sql
docker-compose down
```

## Docker Compose Resources and References

- [Overview of Docker Compose](https://docs.docker.com/compose/) in the Docker documentation.
- [Compose file](https://docs.docker.com/compose/compose-file/) in the Docker documentation.
- [Docker Compose](https://github.com/docker/compose) on GitHub.

Further information about the concepts explained in this page can be found in Docker documentation:

- [Environment variables in Compose](https://docs.docker.com/compose/environment-variables/).
- [Overview of docker-compose CLI](https://docs.docker.com/compose/reference/overview/).
- [Compose command-line reference](https://docs.docker.com/compose/reference/).

---

Content initially contributed by [Vettabase Ltd](https://vettabase.com/).