# Kubernetes Operators for MariaDB

Operators basically instruct Kubernetes about how to manage a certain technology. Kubernetes comes with some default operators, but it is possible to create custom operators. Operators created by the community can be found on [OperatorHub.io](https://operatorhub.io/).

## Custom Operators

Kubernetes provides a declarative API. To support a specific (i.e. MariaDB) technology or implement a desired behavior (i.e. provisioning a [replica](/replication/standard-replication)), we extend Kubernetes API. This involves creating two main components:

- A custom resource.
- A custom controller.

A custom resource adds an API endpoint, so the resource can be managed via the API server. It includes functionality to get information about the resource, like a list of the existing servers.

A custom controller implements the checks that must be performed against the resource to check if its state should be corrected using the API. In the case of MariaDB, some reasonable checks would be verifying that it accepts connections, replication is running, and a server is (or is not) read only.

## MariaDB Operator

[OperatorHub.io](https://operatorhub.io/) has a [MariaDB operator](https://operatorhub.io/operator/mariadb-operator-app). At the time of this writing it is in alpha stage, so please check its maturity before using it.

MariaDB operator is open source and is released under the terms of the MIT license. The source code is [available on GitHub](https://github.com/abalki001/mariadb-operator). The `README.md` file shows usage examples.

It defines the following custom resources:

- MariaDB server.
- MariaDB Backup. [mariabackup](/mariadb-administration/backing-up-and-restoring-databases/mariabackup) is used.
- MariaDB Monitor. Prometheus metrics are exposed on port 9090.

## Other Operators

If you know about other MariaDB operators, feel free to add them to this page (see [Writing and Editing Knowledge Base Articles](/kb/en/writing-and-editing-knowledge-base-articles/)).

MySQL and Percona Server operators should work as well, though some changes may be necessary to fix [incompatibilities](https://mariadb.com/kb/en/mariadb-vs-mysql-compatibility/) or take advantage of certain [MariaDB features](https://mariadb.com/kb/en/mariadb-vs-mysql-features/).

---

Content initially contributed by [Vettabase Ltd](https://vettabase.com/).