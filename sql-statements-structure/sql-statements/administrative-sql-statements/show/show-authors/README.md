# SHOW AUTHORS

## Syntax

```sql
SHOW AUTHORS
```

## Description

The <code class="highlight fixed" style="white-space:pre-wrap">SHOW AUTHORS</code> statement displays information about the
people who work on MariaDB. For each author, it displays Name, Location, and
Comment values. All columns are encoded as latin1.

These include:

- First the active people in MariaDB are listed.
- Then the active people in MySQL.
- Last the people that have contributed to MariaDB/MySQL in the past.

The order is somewhat related to importance of the contribution given to the MariaDB project, but this is not 100% accurate. There is still room for improvement and debate...

## Example

```sql
SHOW AUTHORS;
+--------------------------------+---------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| Name                           | Location                              | Comment                                                                                                                                 |
+--------------------------------+---------------------------------------+-----------------------------------------------------------------------------------------------------------------------------------------+
| Michael (Monty) Widenius       | Tusby, Finland                        | Lead developer and main author                                                                                                          |
| Sergei Golubchik               | Kerpen, Germany                       | Architect, Full-text search, precision math, plugin framework, merges etc                                                               |
| Igor Babaev                    | Bellevue, USA                         | Optimizer, keycache, core work                                                                                                          |
| Sergey Petrunia                | St. Petersburg, Russia                | Optimizer                                                                                                                               |
| Oleksandr Byelkin              | Lugansk, Ukraine                      | Query Cache (4.0), Subqueries (4.1), Views (5.0)                                                                                        |
| Timour Katchaounov             | Sofia , Bulgaria                      | Optimizer                                                                                                                               |
| Kristian Nielsen               | Copenhagen, Denmark                   | Replication, Async client prototocol, General buildbot stuff                                                                            |
| Alexander (Bar) Barkov         | Izhevsk, Russia                       | Unicode and character sets                                                                                                              |
| Alexey Botchkov (Holyfoot)     | Izhevsk, Russia                       | GIS extensions, embedded server, precision math                                                                                         |
| Daniel Bartholomew             | Raleigh, USA                          | MariaDB documentation                                                                                                                   |
| Colin Charles                  | Selangor, Malesia                     | MariaDB documentation, talks at a LOT of conferences                                                                                    |
| Sergey Vojtovich               | Izhevsk, Russia                       | initial implementation of plugin architecture, maintained native storage engines (MyISAM, MEMORY, ARCHIVE, etc), rewrite of table cache |
| Vladislav Vaintroub            | Mannheim, Germany                     | MariaDB Java connector, new thread pool, Windows optimizations                                                                          |
| Elena Stepanova                | Sankt Petersburg, Russia              | QA, test cases                                                                                                                          |
| Georg Richter                  | Heidelberg, Germany                   | New LGPL C connector, PHP connector                                                                                                     |
| Jan Lindström                  | Ylämylly, Finland                     | Working on InnoDB                                                                                                                       |
| Lixun Peng                     | Hangzhou, China                       | Multi Source replication                                                                                                                |
| Percona                        | CA, USA                               | XtraDB, microslow patches, extensions to slow log   
...
```

### See Also

- [SHOW CONTRIBUTORS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-contributors). This list [all members and sponsors of the MariaDB Foundation](https://mariadb.org/en/supporters) and other sponsors.