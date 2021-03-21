# HELP Command

## Syntax

```sql
HELP search_string
```

## Description

The `HELP` command can be used in any MariaDB client, such as the [mysql](/clients-utilities/mysql-client/mysql-command-line-client) command-line client, to get basic syntax help and a short description for most commands and functions.

If you provide an argument to the `HELP` command, the [mysql](/clients-utilities/mysql-client/mysql-command-line-client) client uses it as a search string to access server-side help. The proper operation of this command requires that the help tables in the `mysql` database be initialized with help topic information.

If there is no match for the search string, the search fails. Use `HELP contents` to see a list of the help categories:

```sql
HELP contents
You asked for help about help category: "Contents"
For more information, type 'help <item>', where <item> is one of the following
categories:
   Account Management
   Administration
   Compound Statements
   Data Definition
   Data Manipulation
   Data Types
   Functions
   Functions and Modifiers for Use with GROUP BY
   Geographic Features
   Help Metadata
   Language Structure
   Plugins
   Procedures
   Sequences
   Table Maintenance
   Transactions
   User-Defined Functions
   Utility
```

If a search string matches multiple items, MariaDB shows a list of matching topics:

```sql
HELP drop
Many help items for your request exist.
To make a more specific request, please type 'help <item>',
where <item> is one of the following
topics:
   ALTER TABLE
   DROP DATABASE
   DROP EVENT
   DROP FUNCTION
   DROP FUNCTION UDF
   DROP INDEX
   DROP PACKAGE
   DROP PACKAGE BODY
   DROP PROCEDURE
   DROP ROLE
   DROP SEQUENCE
   DROP SERVER
   DROP TABLE
   DROP TRIGGER
   DROP USER
   DROP VIEW
```

Then you can enter a topic as the search string to see the help entry for that topic.

The help is provided with the MariaDB server and makes use of four help tables found in the `mysql` database: [help_relation](/kb/en/mysqlhelp_relation-table/), [help_topic](/kb/en/mysqlhelp_topic-table/), [help_category](/kb/en/mysqlhelp_category-table/) and [help_keyword](/kb/en/mysqlhelp_keyword-table/). These tables are populated by the `mysql_install_db` or `fill_help_table.sql` scripts which, until [MariaDB 10.4.7](/kb/en/mariadb-1047-release-notes/), contain data generated from an old version of MySQL.