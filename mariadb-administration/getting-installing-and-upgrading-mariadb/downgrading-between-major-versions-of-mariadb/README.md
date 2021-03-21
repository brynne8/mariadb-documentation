# Downgrading between Major Versions of MariaDB

Downgrading MariaDB is not supported. The only reliable way to downgrade is to [restore from a full backup](/mariadb-administration/backing-up-and-restoring-databases) made before upgrading, and start the old version of MariaDB.

Some people have reported successfully downgrading, but there are many possible things that can go wrong, and downgrading is not tested in any way by the MariaDB developers.

Between major releases, there are often substantial changes, even if none of the new features are used. For example, both [MariaDB 10.2](/kb/en/what-is-mariadb-102/) and [MariaDB 10.3](/kb/en/what-is-mariadb-103/) introduce new versions of the redo log.

Even within minor releases, there may be problems, for example [MariaDB 10.1.21](/kb/en/mariadb-10121-release-notes/) fixed a file format incompatibility bug that prevents a downgrade to earlier [MariaDB 10.1](/kb/en/what-is-mariadb-101/) releases.