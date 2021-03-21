# Installing OQGRAPH

The Open Query GRAPH computation engine, or OQGRAPH as the engine itself is called, allows you to handle hierarchies (tree structures) and complex graphs (nodes having many connections in several directions).

## Installation

The OQGRAPH storage engine exists as a separate package in the repositories for [MariaDB 10.0.7](/kb/en/mariadb-1007-release-notes/) and later. On Ubuntu and Debian the package is called `mariadb-oqgraph-engine-10.0` or `mariadb-plugin-oqgraph`. On Red Hat, CentOS, and Fedora the package is called `MariaDB-oqgraph-engine`. To install the plugin, first install the appropriate package and then install the plugin using the [INSTALL SONAME](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-soname) or [INSTALL PLUGIN](/sql-statements-structure/sql-statements/administrative-sql-statements/plugin-sql-statements/install-plugin) commands.

### Debian and Ubuntu

On Debian and Ubuntu, install the package as follows:

```sql
sudo apt-get install mariadb-plugin-oqgraph
```

or (for [MariaDB 10.0](/kb/en/what-is-mariadb-100/))

```sql
sudo apt-get install mariadb-oqgraph-engine-10.0 
```

### Fedora/Red Hat/CentOS

Note that OQGRAPH v3 requires libjudy, which is not in the official Red Hat/Fedora repositories. This needs to be installed first, for example:

```sql
wget http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
rpm -Uvh epel-release-6-8.noarch.rpm
```

Then install the package, as follows:

```sql
sudo yum install MariaDB-oqgraph-engine
```

### Installing the Plugin

On either system you can then launch the `mysql` command-line client and install the plugin in MariaDB as follows:

```sql
INSTALL SONAME 'ha_oqgraph';
```

## See Also

More information on this engine is found on the OpenQuery website: [https://openquery.com.au/products/graph-engine](https://openquery.com.au/products/graph-engine)