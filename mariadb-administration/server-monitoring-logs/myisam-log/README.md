# MyISAM Log

The MyISAM log records all changes to [MyISAM](/kb/en/myisam/) tables. It is not enabled by default. To enable it, start the server with the [--log-isam](/kb/en/mysqld-options/#-log-isam) option, for example:

```sql
--log-isam=myisam.log
```

The <em>isam</em> instead of <em>myisam</em> above is not a typo - it's a legacy from when the predecessor to the MyISAM format, called ISAM. The option can be used without specifying a filename, in which case the default, <em>myisam.log</em> is used.

To process the contents of the log file, use the [myisamlog](/clients-utilities/myisam-clients-and-utilities/myisamlog) utility.