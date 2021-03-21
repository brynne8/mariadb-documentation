# LOOP

## Syntax

```sql
[begin_label:] LOOP
    statement_list
END LOOP [end_label]
```

## Description

`LOOP` implements a simple loop construct, enabling repeated execution
of the statement list, which consists of one or more statements, each
terminated by a semicolon (i.e., `;`) statement delimiter. The statements
within the loop are repeated until the loop is exited; usually this is
accomplished with a [LEAVE](/programming-customizing-mariadb/programmatic-compound-statements/leave/) statement.

A `LOOP` statement can be [labeled](/programming-customizing-mariadb/programmatic-compound-statements/labels/). `end_label` cannot be given unless
`begin_label` also is present. If both are present, they must be the
same.

See [Delimiters](/clients-utilities/mysql-client/delimiters/) in the [mysql](/clients-utilities/mysql-client/) client for more on delimiter usage in the client.

## See Also

- [LOOP in Oracle mode](/kb/en/sql_modeoracle-from-mariadb-103/#loop)
- [ITERATE](/programming-customizing-mariadb/programmatic-compound-statements/iterate/)
- [LEAVE](/programming-customizing-mariadb/programmatic-compound-statements/leave/)
- [FOR Loops](/programming-customizing-mariadb/programmatic-compound-statements/for/)