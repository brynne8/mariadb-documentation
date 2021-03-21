# ColumnStore Configuration File Update and Distribution

In the case where an entry in the MariaDB ColumnStore's configuration needs to be updated and distributed, this can be done from the command line from Performance Module #1. All changes made to MariaDB ColumnStore's configuration file need to be applied on PM1.

NOTE: 'mcsadmin distributeconfigfile' only needs to be run if the system is Active. If the system is down, then just make the change and when the system is started, the update will get distributed.

Here is an example

```sql
/usr/local/mariadb/columnstore/bin/configxml.sh setconfig SystemConfig SystemName mcs-1
mcsadmin distributeconfigfile
```

In the cases where the MariaDB ColumnStore's configuration files gets out of sync with the PM1 copy, run the following to get MariaDB ColumnStore's configuration file redistribute to the other nodes.

To do this run the following on PM1:

```sql
mscadmin distributeconfigfile
```