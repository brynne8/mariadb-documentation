# SHOW CREATE VIEW

## Syntax

```sql
SHOW CREATE VIEW view_name
```

## Description

This statement shows a <code class="highlight fixed" style="white-space:pre-wrap">[CREATE VIEW](/programming-customizing-mariadb/views/create-view)</code> statement that creates the given [view](/programming-customizing-mariadb/views), as well as the character set used by the connection when the view was created. This statement
also works with views.

<code class="highlight fixed" style="white-space:pre-wrap">SHOW CREATE VIEW</code> quotes table, column and stored function names according to the value of the <a undefined>sql_quote_show_create</a> server system variable.

## Examples

```sql
SHOW CREATE VIEW example\G
*************************** 1. row ***************************
                View: example
         Create View: CREATE ALGORITHM=UNDEFINED DEFINER=`root`@`localhost` SQL
SECURITY DEFINER VIEW `example` AS (select `t`.`id` AS `id`,`t`.`s` AS `s` from
`t`)
character_set_client: cp850
collation_connection: cp850_general_ci
```

With <a undefined>sql_quote_show_create</a> off:

```sql
SHOW CREATE VIEW example\G
*************************** 1. row ***************************
                View: example
         Create View: CREATE ALGORITHM=UNDEFINED DEFINER=root@localhost SQL SECU
RITY DEFINER VIEW example AS (select t.id AS id,t.s AS s from t)
character_set_client: cp850
collation_connection: cp850_general_ci
```