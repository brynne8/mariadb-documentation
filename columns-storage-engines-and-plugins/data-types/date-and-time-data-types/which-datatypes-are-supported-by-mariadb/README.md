# Which datatypes  are supported by MariaDB

I would like to know which datatypes are supported by MariaDB. I'm asking since I'm little confused. For example, the knowledge base contains a chapter temporal datatypes:
[https://kb.askmonty.org/en/temporal-data-types/](https://kb.askmonty.org/en/temporal-data-types/)

where the the datatype timestamp is defined:
"it defines a set of correctly formed values that represent any valid Gregorian calendar date between '0001-01-01' and '9999-12-31'"

If I look at the MySQL-site
[http://dev.mysql.com/doc/refman/5.1/de/datetime.html](http://dev.mysql.com/doc/refman/5.1/de/datetime.html)

The definition for timestamp is:
"The TIMESTAMP data type is used for values that contain both date and time parts. TIMESTAMP has a range of '1970-01-01 00:00:01' UTC to '2038-01-19 03:14:07' UTC."

These are different definitions.I know that the datatype defintions in the knowledge base contains the syntax of SQL-99. What makes me insecure is which definition has priority.

Also I do not know if all datatypes in Oracle-MySQL are supported by MariaDB. For example, tinyint, bigint. (Sincerly, I know that MariaDB understand these datatypes, but I wonder if they have the same definition as given by the Oracle-MySQL-site.)
Thanks for help

## Answer
            <span class="answer_comment">Answered by 
<span class="user" id="user-982">
[Federico Razzoli](/kb/user/id/982)
</span> in [this comment](#comment_705).</span>

All datatypes in MySQL 5.5 are also in all versions of MariaDB. I'm not sure if MySQL 5.6 introduced IP types; those types seem not to be in MariaDB yet, but it will be, because I see this task in JIRA: [MDEV-274](https://jira.mariadb.org/browse/MDEV-274)

The KnowledgeBase page you linked seems to be wrong. Here is the output of:
HELP 'timestamp';

MariaDB [(none)]&gt; HELP timestamp;
Name: 'TIMESTAMP'
Description:
TIMESTAMP

A timestamp. The range is '1970-01-01 00:00:01' UTC to '2038-01-19
03:14:07' UTC. TIMESTAMP values are stored as the number of seconds
since the epoch ('1970-01-01 00:00:00' UTC). A TIMESTAMP cannot
represent the value '1970-01-01 00:00:00' because that is equivalent to
0 seconds from the epoch and the value 0 is reserved for representing
'0000-00-00 00:00:00', the "zero" TIMESTAMP value.

Unless specified otherwise, the first TIMESTAMP column in a table is
defined to be automatically set to the date and time of the most recent
modification if not explicitly assigned a value. This makes TIMESTAMP
useful for recording the timestamp of an INSERT or UPDATE operation.
You can also set any TIMESTAMP column to the current date and time by
assigning it a NULL value, unless it has been defined with the NULL
attribute to permit NULL values. The automatic initialization and
updating to the current date and time can be specified using DEFAULT
CURRENT_TIMESTAMP and ON UPDATE CURRENT_TIMESTAMP clauses, as described
in
[http://dev.mysql.com/doc/refman/5.5/en/timestamp-initialization.html](http://dev.mysql.com/doc/refman/5.5/en/timestamp-initialization.html).

- Note*: The TIMESTAMP format that was used prior to MySQL 4.1 is not
supported in MySQL 5.5; see MySQL 3.23, 4.0, 4.1 Reference Manual for
information regarding the old format.