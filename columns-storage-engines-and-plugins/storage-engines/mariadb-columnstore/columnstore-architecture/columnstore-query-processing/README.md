# ColumnStore Query Processing

MariaDB ColumnStore processes an end user query from the user to User and Performance modules.

1 Clients issue a query to the MariaDB Server running on the User Module. The server performs a table operation for all tables needed to fulfill the request and obtains the initial query execution plan.
2 Using the MariaDB storage engine interface, ColumnStore converts the server table object into ColumnStore objects.  These objects are then sent to the User Module processes.
3 The User Module converts the MariaDB execution plan and optimizes the given objects into a ColumnStore execution plan.  It then determines the steps needed to run the query and the order in which they need to be run.
4 The User Module then consults the Extent Map to determine which Performance Modules to consult for the data it needs, it then performs Extent Elimination, eliminating any Performance Modules from the list that only contain data outside the range of what the query requires.
5 The User Module then sends commands to one or more Performance Modules to perform block I/O operations.
6 The Performance Module or Modules carry out predicate filtering, join processing, initial aggregation of data from local or external storage, then send the data back to the User Module.
7 The User Module performs the final result-set aggregation and composes the result-set for the query.
8 The User Module / ExeMgr implements any window function calculations, as well as any necessary sorting on the result-set.  It then returns the result-set to the server.
9 The MariaDB Server performs any select list functions, `ORDER BY` and `LIMIT` operations on the result-set.
10 The MariaDB Server returns the result-set to the client.