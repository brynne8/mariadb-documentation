# Orchestrator Overview

Orchestrator is a MySQL and MariaDB high availability and replication management tool. It is released by Shlomi Noach under the terms of the Apache License, version 2.0.

Orchestrator provides automation for MariaDB replication in the following ways:

- It can be used to perform certain operations, like repairing broken replication or moving a replica from one master to another. These operations can be requested using CLI commands, or via the GUI provided with Orchestrator. The actual commands sent to MariaDB are automated by Orchestrator, and the user doesn't have to worry about the details.
- Orchestrator can also automatically perform a failover in case a master crashes or is unreachable by its replicas. If that is the case, Orchestrator will promote one of the replicas to a master. The replica to promote is chosen based on several criteria, like the server versions, the [binary log](/mariadb-administration/server-monitoring-logs/binary-log/) formats in use and the datacenters locations.

To install Orchestrator, see:

- The [install.md](https://github.com/openark/orchestrator/blob/master/docs/install.md) for a manual installation;
- The links in [README.md](https://github.com/openark/orchestrator/blob/master/README.md), to install Orchestrator using automation tools.

## Supported Topologies

Currently, Orchestrator fully supports MariaDB [GTID](/replication/standard-replication/gtid/), [replication](/replication/), and [semi-synchronous replication](/replication/standard-replication/semisynchronous-replication/). While Orchestrator does not support Galera specific logic, it works with Galera clusters. For details, see [Supported Topologies and Versions](https://github.com/openark/orchestrator/blob/master/docs/supported-topologies-and-versions.md) in Orchestrator documentation.

## Architecture

Orchestrator consists of a single executable called `orchestrator`. This is a process that periodically connects to the target servers. It will run SQL queries against target servers, so it needs a user with proper permissions. When the process is running, a GUI is available via a web browser, at the URL '[https://localhost:3000'](https://localhost:3000'). It also exposes a REST API (see [Using the web API](https://github.com/openark/orchestrator/blob/master/docs/using-the-web-api.md) in the Orchestrator documentation).

Orchestrator expects to find a JSON configuration file called `orchestrator.conf.json`, in `/etc`.

A database is used to store the configuration and the state of the target servers. By default, this is done using built-in SQLite. However, it is possible to use an external MariaDB or MySQL server.

If a cluster of Orchestrator instances is running, only one central database is used. One Orchestrator node is active, while the others are passive and are only used for failover. If the active node crashes or becomes unreachable, one of the other nodes becomes the active instance. The `active_node` table shows which node is active. Nodes communicate between them using the Raft protocol.

## CLI Examples

As mentioned, Orchestrator can be used from the command-line. Here you can find some examples.

List clusters:

```sql
orchestrator -c clusters
```

Discover a specified instance and add it to the known topology:

```sql
orchestrator -c discover -i <host>:<port>
```

Forget about an instance:

```sql
orchestrator -c topology -i <host>:<port>
```

Move a replica to a different master:

```sql
orchestrator -c move-up -i <replica-host>:<replica-port> -d <master-host>:<master-port>
```

Move a replica up, so that it becomes a "sibling" of its master:

```sql
orchestrator -c move-up -i <replica-host>:<replica-port>
```

Move a replica down, so that it becomes a replica of its"sibling":

```sql
orchestrator -c move-below -i <replica-host>:<replica-port> -d <master-host>:<master-port>
```

Make a node read-only:

```sql
orchestrator -c set-read-only -i <host>:<port>
```

Make a node writeable:

```sql
orchestrator -c set-writeable -i <host>:<port>
```

The `--debug` and `--stack` options can be added to the above commands to make them more verbose.

## Orchestrator Resources and References

- [Orchestrator on GitHub](https://github.com/openark/orchestrator).
- [Documentation](https://github.com/openark/orchestrator/tree/master/docs).
- [Raft consensus protocol website](https://raft.github.io/).

The [README.md](https://github.com/openark/orchestrator/blob/master/README.md) file lists some related community projects, including modules to automate Orchestrator with [Puppet](/mariadb-administration/getting-installing-and-upgrading-mariadb/binary-packages/automated-mariadb-deployment-and-administration/automated-mariadb-deployment-and-administration-puppet-and-mariadb/) and other technologies.

On GitHub you can also find links to projects that allow the use of automation software to deploy and manage Orchestrator.

---

Content initially contributed by [Vettabase Ltd](https://vettabase.com/).