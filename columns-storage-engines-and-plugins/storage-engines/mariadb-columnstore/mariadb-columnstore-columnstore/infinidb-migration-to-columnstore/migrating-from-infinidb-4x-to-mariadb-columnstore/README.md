# Migrating from InfiniDB 4.x  to MariaDB ColumnStore

## Overview

The columnar disk storage format is unchanged between InfiniDB 4.X  and MariaDB ColumnStore allowing for relatively straightforward data migration utilizing backup and restore logic. This document outlines an approach to perform the migration that can be adapted to your particular needs.

The examples in this document assume a root install with the packages installed in /usr/local.
For non-root system, just substitute /usr/local with $HOME, which is the non-root user home directory.

## Single Server Install

### Backup Data in InfiniDB

Suspend writes if this is a live system:

```sql
# cc suspendDatabaseWrites y
```

##### Backup Front-End schemas

Use mysqldump to create schema files from appropriate databases:

```sql
/usr/local/Calpont/mysql/bin/mysqldump --defaults-file=/usr/local/Calpont/mysql/my.cnf --skip-lock-tables --no-data loans > loans_schema.sql
```

Update schema file to utilize correct engine and add schema sync only comment:

```sql
# sed "s/ENGINE=InfiniDB/ENGINE=columnstore COMMENT='schema sync only'/gI" loans_schema.sql > loans_schema_columnstore.sql
```

##### Backup Back-End data

Take a backup of the columnar data files pm1 which are stored in the data&lt;N&gt; directories. The exact folder list can be confirmed by looking at the SystemConfig section of the configuration file /usr/local/Calpont/etc/Calpont.xml. Each data&lt;n&gt; directory corresponds to a specific DBRoot containing the actual columnar data in the 000.dir and system metadata under systemFiles/dbrm.  In addition you may also want to take a copy of the data directory if this contains custom scripts for bulk loading:

```sql
cp -r /usr/local/Calpont/data? .
```

### Restoring Backup into ColumnStore:

##### Restore Front-End schemas

First install a new fresh install of ColumnStore then create the schema using the mysqldump file:

```sql
# mcsmysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 5
Server version: 10.1.19-MariaDB Columnstore 1.0.5-1

Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create database loans;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> use loans
Database changed
MariaDB [loans]> source loans_schema_columnstore.sql
Query OK, 0 rows affected (0.00 sec)

...

MariaDB [loans]> exit
```

##### Restore Back-End data

Now replace the data&lt;N&gt; directories with the backup on pm1 as appropriate for each directory:

```sql
# mcsadmin shutdownSystem y
# cd /usr/local/mariadb/columnstore/
# mv data1 data1.bkp
# mv /backupdir/data1 .
# cd data1/systemFiles/dbrm/
# mv BRM_saves_current BRM_saves_current.bkp
# cp ../../../data1.bkp/systemFiles/dbrm/BRM_saves_current .
# mcsadmin startSystem
```

The system should start cleanly and the data should now be accessible in the database.

## Multi Server Install - Combined UM/PM

### Backup Data in InfiniDB

Suspend writes if this is a live system, enter on pm1:

```sql
# cc suspendDatabaseWrites y
```

##### Backup Front-End schemas

Use mysqldump to create schema files from appropriate databases from pm1

```sql
/usr/local/Calpont/mysql/bin/mysqldump --defaults-file=/usr/local/Calpont/mysql/my.cnf --skip-lock-tables --no-data loans > loans_schema.sql
```

Update schema file to utilize correct engine and add schema sync only comment:

```sql
# sed "s/ENGINE=InfiniDB/ENGINE=columnstore COMMENT='schema sync only'/gI" loans_schema.sql > loans_schema_columnstore.sql
```

##### Backup Back-End data

Take a backup of the columnar data files from each PM which are stored in the data&lt;N&gt; directories of each PM server. The exact folder list can be confirmed by looking at the SystemConfig section of the configuration file /usr/local/Calpont/etc/Calpont.xml. Each data&lt;n&gt; directory corresponds to a specific DBRoot containing the actual columnar data in the 000.dir and system metadata under systemFiles/dbrm.  In addition you may also want to take a copy of the data directory if this contains custom scripts for bulk loading:

```sql
cp -r /usr/local/Calpont/data? .
```

### Restoring Backup into ColumnStore

##### Restore Front-End schemas

First install a new fresh install of ColumnStore then create the schema using the mysqldump file:

```sql
# mcsmysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 5
Server version: 10.1.19-MariaDB Columnstore 1.0.5-1

Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create database loans;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> use loans
Database changed
MariaDB [loans]> source loans_schema_columnstore.sql
Query OK, 0 rows affected (0.00 sec)

...

MariaDB [loans]> exit
```

##### Restore Back-end Data

Now replace the data&lt;N&gt; directories with the backup on each PM as appropriate for each directory:

```sql
# mcsadmin shutdownSystem y
# cd /usr/local/mariadb/columnstore/
# mv data1 data1.bkp
# mv /backupdir/data1 .
# cd data1/systemFiles/dbrm/
# mv BRM_saves_current BRM_saves_current.bkp
# cp ../../../data1.bkp/systemFiles/dbrm/BRM_saves_current .
# mcsadmin startSystem
```

The system should start cleanly and the data should now be accessible in the database.

## Multi Server Install - Separate UM/PM

### Backup Data in InfiniDB

Suspend writes if this is a live system, enter on pm1:

```sql
# cc suspendDatabaseWrites y
```

##### Backup Front-End schemas

On um1:

Use mysqldump to create schema files from appropriate databases from pm1

```sql
/usr/local/Calpont/mysql/bin/mysqldump --defaults-file=/usr/local/Calpont/mysql/my.cnf --skip-lock-tables --no-data loans > loans_schema.sql
```

Update schema file to utilize correct engine and add schema sync only comment:

```sql
# sed "s/ENGINE=InfiniDB/ENGINE=columnstore COMMENT='schema sync only'/gI" loans_schema.sql > loans_schema_columnstore.sql
```

##### Backup Back-End data

Take a backup of the columnar data files from each PM which are stored in the data&lt;N&gt; directories of each PM server. The exact folder list can be confirmed by looking at the SystemConfig section of the configuration file /usr/local/Calpont/etc/Calpont.xml. Each data&lt;n&gt; directory corresponds to a specific DBRoot containing the actual columnar data in the 000.dir and system metadata under systemFiles/dbrm.  In addition you may also want to take a copy of the data directory if this contains custom scripts for bulk loading:

```sql
cp -r /usr/local/Calpont/data? .
```

### Restoring Backup into ColumnStore

##### Restore Front-End schemas

On um1:

First install a new fresh install of ColumnStore then create the schema using the mysqldump file:

```sql
# mcsmysql
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 5
Server version: 10.1.19-MariaDB Columnstore 1.0.5-1

Copyright (c) 2000, 2016, Oracle, MariaDB Corporation Ab and others.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

MariaDB [(none)]> create database loans;
Query OK, 1 row affected (0.00 sec)

MariaDB [(none)]> use loans
Database changed
MariaDB [loans]> source loans_schema_columnstore.sql
Query OK, 0 rows affected (0.00 sec)

...

MariaDB [loans]> exit
```

##### Restore Back-end Data

Now replace the data&lt;N&gt; directories with the backup on each PM as appropriate for each directory:

```sql
# mcsadmin shutdownSystem y
# cd /usr/local/mariadb/columnstore/
# mv data1 data1.bkp
# mv /backupdir/data1 .
# cd data1/systemFiles/dbrm/
# mv BRM_saves_current BRM_saves_current.bkp
# cp ../../../data1.bkp/systemFiles/dbrm/BRM_saves_current .
# mcsadmin startSystem
```

The system should start cleanly and the data should now be accessible in the database.