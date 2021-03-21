# NDB in MariaDB

NDB (MySQL Cluster) has been removed completely from [MariaDB 10.1](/kb/en/what-is-mariadb-101/). From [MariaDB 10.0](/kb/en/what-is-mariadb-100/) and below, it was disabled due to it not being in the main upstream MySQL releases. Upstream has NDB available as a separate download, because Cluster is updated more frequently compared to baseline MySQL.

For an actively developed alternative, see [Galera Cluster](/kb/en/galera/).

One of the prerequisites to having NDB working in MariaDB is for us to get an active maintainer of the NDB code.

The [MariaDB community](/kb/en/community/) has not had much interest in this feature.
If there is interest in this feature, please send email to the
[maria-discuss](http://launchpad.net/~maria-discuss) mailing list, or file a
request in [Jira](http://jira.mariadb.org).

In 'theory' it should be easy for anyone to get ndb in MariaDB to work as well as in MySQL as all the ndb code is still in the tree. If you are interested in (and capable of) doing this, please join the MariaDB project!