# MariaDB Audit Plugin

MariaDB and MySQL are used in a broad range of environments, but if you needed to record user access to be in compliance with auditing regulations for your organization, you would previously have had to use other database solutions. To meet this need, though, MariaDB has developed the MariaDB Audit Plugin. Although the MariaDB Audit Plugin has some unique features available only for MariaDB, it can be used also with MySQL.

Basically, the purpose of the MariaDB Audit Plugin is to log the server's activity. For each client session, it records who connected to the server (i.e., user name and host), what queries were executed, and which tables were accessed and server variables that were changed. This information is stored in a rotating log file or it may be sent to the local `syslogd`.

The MariaDB Audit Plugin works with MariaDB, MySQL (as of, version 5.5.34 and 10.0.7) and Percona Server. MariaDB started including by default the Audit Plugin from versions 10.0.10 and 5.5.37, and it can be installed in any version from [MariaDB 5.5.20](/kb/en/mariadb-5520-release-notes/).

#### Additional documentation

Below are links to additional documentation on the MariaDB Audit Plugin. They explain in detail how to install, configure and use the Audit Plugin.

- [Installation](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-installation/)
- [Configuration](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-configuration/)
- [Log Settings](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-log-settings/)
- [Log Location &amp; Rotation](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-location-and-rotation-of-logs/)
- [Log Format](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-log-format/)
- [Status Variables](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-status-variables/)
- [System Variables](/kb/en/mariadb-audit-plugin-system-variables/)
- [Release Notes](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/release-notes-mariadb-audit-plugin/)
<br>
<br>

#### Tutorials

Below are links to some tutorials on MariaDB's site and other sites.  They may help you to get more out of the MariaDB Audit Plugin.

- [Introducing the MariaDB Audit Plugin](/resources/blog/introducing-mariadb-audit-plugin) <br>
by Anatoliy Dimitrov, September 2, 2014

- [Activating MariaDB Audit Log](https://tunnelix.com/activating-mariadb-audit-log/)
by Jaykishan Mutkawoa, May 30, 2016 <br>

- [Installing MariaDB Audit Plugin on Amazon RDS](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.MySQL.Options.AuditPlugin.html) <br>
Amazon RDS supports using the MariaDB Audit Plugin on MySQL and MariaDB database instances.
<br>
<br>

#### Web Log Articles

Below are links to web log articles on the MariaDB Audit Plugin.  You may find them useful in understanding better how to use the Audit Plugin. Since some of these articles are older, they won't include changes and improvements in newer versions.  You can rely on the documentation pages listed above for the most current information.

- [Activating Auditing for MariaDB in 5 Minutes](/resources/blog/activating-auditing-mariadb-and-mysql-5-minutes) <br>
by Ralf Gebhardt, September 29, 2013

- [Query and Password Filtering with the MariaDB Audit Plugin](/resources/blog/query-and-password-filtering-mariadb-audit-plugin) <br>
by Ralf Gebhardt, May 4, 2015

- [Set Up a Remote Log File using rsyslog](/resources/blog/mariadb-audit-plugin-set-remote-log-file-using-rsyslog) <br>
by Ralf Gebhardt, December 16, 2013

- [MySQL Auditing with MariaDB Auditing Plugin](https://planet.mysql.com/entry/?id=5994184)
by Peter Zeitsev, February 15, 2016
<br>
<br>

#### Sub-Documents

- [MariaDB Audit Plugin - Installation](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-installation/) — Installing the MariaDB Audit Plugin.
- [MariaDB Audit Plugin - Configuration](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-configuration/) — Audit Plugin global variables within MariaDB
- [MariaDB Audit Plugin - Log Settings](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-log-settings/) — Log audit events to a file or syslog
- [MariaDB Audit Plugin - Location and Rotation of Logs](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-location-and-rotation-of-logs/) — Logs can be written to a separate file or to the system logs
- [MariaDB Audit Plugin - Log Format](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-log-format/) — The audit log is a set of records written as a list of fields to a file in plain‐text format
- [MariaDB Audit Plugin - Versions](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-versions/) — Releases of the MariaDB Audit Plugin, and in which versions of MariaDB each...
- [MariaDB Audit Plugin Options and System Variables](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-options-and-system-variables/) — Description of Server_Audit plugin options and system variables.
- [MariaDB Audit Plugin - Status Variables](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/mariadb-audit-plugin-status-variables/) — Server Audit plugin status variables
- [Release Notes - MariaDB Audit Plugin](/columns-storage-engines-and-plugins/plugins/mariadb-audit-plugin/release-notes-mariadb-audit-plugin/) — MariaDB Audit Plugin release notes