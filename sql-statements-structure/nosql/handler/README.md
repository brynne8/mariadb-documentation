# HANDLER

The `HANDLER` statements give you direct access to reading rows from the storage engine. This is much faster than normal access through [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select/) as there is less parsing involved and no optimizer involved.

You can use [prepared statements](/sql-statements-structure/sql-statements/prepared-statements/) for `HANDLER READ`, which should give you a speed comparable to [HandlerSocket](/sql-statements-structure/nosql/handlersocket/). Also see Yoshinori Matsunobu's blog post [Using MySQL as a NoSQL - A story for exceeding 750,000 qps on a commodity server](http://yoshinorimatsunobu.blogspot.com/2010/10/using-mysql-as-nosql-story-for.html).

- [HANDLER Commands](/sql-statements-structure/nosql/handler/handler-commands/) — Direct access to table storage engine interfaces for key lookups and key or table scans.
- [HANDLER for MEMORY Tables](/sql-statements-structure/nosql/handler/handler-for-memory-tables/) — Using HANDLER commands efficiently with MEMORY/HEAP tables