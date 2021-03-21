# Using CONNECT - Exporting Data From MariaDB

##### MariaDB starting with [10.0](/kb/en/what-is-mariadb-100/)

The CONNECT handler was introduced in [MariaDB 10.0](/kb/en/what-is-mariadb-100/).

Exporting data from MariaDB is obviously possible with CONNECT in particular for all formats not supported by the [SELECT INTO OUTFILE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select-into-outfile) statement. Let us consider the query:

```sql
select plugin_name handler, plugin_version version, plugin_author
author, plugin_description description, plugin_maturity maturity
from information_schema.plugins where plugin_type = 'STORAGE ENGINE';
```

Supposing you want to get the result of this query into a file handlers.htm in XML/HTML format, allowing displaying it on an Internet browser, this is how you can do it:

Just create the CONNECT table that will be used to make the file:

```sql
create table handout
engine=CONNECT table_type=XML file_name='handout.htm' header=yes
option_list='name=TABLE,coltype=HTML,attribute=border=1;cellpadding=5
     ,headattr=bgcolor=yellow'
select plugin_name handler, plugin_version version, plugin_author
author, plugin_description description, plugin_maturity maturity
from information_schema.plugins where plugin_type = 'STORAGE ENGINE';
```

Here the column definition is not given and will come from the Select statement following the Create. The CONNECT options are the same we have seen previously. This will do both actions, creating the matching <em>handlers</em> CONNECT table and 'filling' it with the query result.

<strong>Note 1:</strong> This could not be done in only one statement if the table type had required using explicit CONNECT column options. In this case, firstly create the table, and then populate it with an Insert statement.

<strong>Note 2:</strong> The source “plugins” table column “description” is a long text column, data type not supported for CONNECT tables. It has been silently internally replaced by varchar(256).