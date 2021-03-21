# Configuring ColumnStore Cross-Engine Joins

MariaDB ColumnStore allows columnstore tables to be joined with non-columnstore tables (e.g. [MyISAM](/kb/en/myisam/) tables) within a query. The non-columnstore table may be on the MariaDB ColumnStore system OR on an external server that supports MariaDB client connections.

To enable this process,  the &lt;CrossEngineSupport&gt; section in Columnstore.xml is configured with connection information.

The following is an example entry in the Columnstore.XML configuration file to gain access to joined tables Single Server MariaDB ColumnStore install. The Host needs to be either 127.0.0.1 or 'localhost':

```sql
<CrossEngineSupport>
       <Host>127.0.0.1</Host>
       <Port>3306</Port>
       <User>mydbuser</User>
       <Password>pwd</Password>
</CrossEngineSupport>
```

For a Multi-Node MariaDB Columnstore Installation, the Host needs to be the IP Address of the User-Module #1 or the Combination User/Performance Module #1.

If the MariaDB Client is is running on an external Server, then it would be the IP Address of that server.

For version 1.2.0 onwards the additional options in the &lt;CrossEngineSupport&gt; section are supported to add SSL/TLS encryption to the connections:

```sql
       <TLSCA></TLSCA>
       <TLSClientCert></TLSClientCert>
       <TLSClientKey></TLSClientKey>
```

This change should be made while the ColumnStore server is down. In a multi node deployment, the change should be made on the PM1 node only as this will be replicated to other instances upon restart.

Check here on how to make changes via the command line to Columnstore.xml:

[https://mariadb.com/kb/en/mariadb/columnstore-configuration-file-update-and-distribution](https://mariadb.com/kb/en/mariadb/columnstore-configuration-file-update-and-distribution)

## Troubleshooting

<strong> ERROR 1815 (HY000): Internal error: IDB-8001: CrossEngineSupport section in Columnstore.xml is not properly configured </strong> <br>

- Confirm that Columnstore.xml was correctly updated on pm1 and the server restarted.

<strong> ERROR 1815 (HY000): Internal error: fatal error in drizzle_con_connect()(23)(23) </strong>

- Confirm that the values specified for CrossEngineSupport in ColumnStore.xml are correct  for the login to be used.

<strong> ERROR 1815 (HY000): Internal error: fatal error executing query in crossengine client lib(17)(17) </strong>

- Confirm that the login used has create temporary tables permission on infinidb_vtable:

```sql
grant create temporary tables on infinidb_vtable.* to mydbuser@127.0.0.1;
```

- Confirm that the login used has grant SELECT on the table referenced in the cross engine join. Verify by attempting to connect from each UM using mcsmysql and query the table you want to reference:

```sql
mcsmysql -u mydbuser -p -h 127.0.0.1 
> use mydb;
> select * from innodb_table limit 10;
```

## Notes

- Cross engine will not work against a MyISAM/Aria table that has 0 or 1 rows in it. This is due to MariaDB's optimizer shortcut for this specific condition. We recommend using InnoDB instead of MyISAM/Aria for this case.