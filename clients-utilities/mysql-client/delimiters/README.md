# Delimiters

The default delimiter in the [mysql](/clients-utilities/mysql-client/) client (from [MariaDB 10.4.6](/kb/en/mariadb-1046-release-notes/), [also called mariadb](/clients-utilities/mysql-client/mariadb-command-line-client/)) is the semicolon.

When creating [stored programs](/kb/en/stored-programs-and-views/) from the command-line, it is likely you will need to differentiate between the regular delimiter and a delimiter inside a [BEGIN END](/programming-customizing-mariadb/programmatic-compound-statements/begin-end/) block.  To understand better, consider the following example:

```sql
CREATE FUNCTION FortyTwo() RETURNS TINYINT DETERMINISTIC
BEGIN
 DECLARE x TINYINT;
 SET x = 42;
 RETURN x;
END; 
```

If you enter the above line by line, the mysql client will treat the first semicolon, at the end of the <code class="fixed" style="white-space:pre-wrap">DECLARE x TINYINT</code> line, as the end of the statement. Since that's only a partial definition, it will throw a syntax error, as follows:

```sql
CREATE FUNCTION FortyTwo() RETURNS TINYINT DETERMINISTIC
BEGIN
DECLARE x TINYINT;
ERROR 1064 (42000): You have an error in your SQL syntax; 
check the manual that corresponds to your MariaDB server version
 for the right syntax to use near '' at line 3
```

The solution is to specify a distinct delimiter for the duration of the process, using the DELIMITER command. The delimiter can be any set of characters you choose, but it needs to be a distinctive set of characters that won't cause further confusion. `//` is a common choice, and used throughout the knowledgebase.

Here's how the function could be successfully entered from the mysql client with the new delimiter.

```sql
DELIMITER //

CREATE FUNCTION FortyTwo() RETURNS TINYINT DETERMINISTIC
BEGIN
  DECLARE x TINYINT;
  SET x = 42;
  RETURN x;
END 

//

DELIMITER ;
```

At the end, the delimiter is restored to the default semicolon. The `\g` and `\G` delimiters can always be used, even when a custom delimiter is specified.