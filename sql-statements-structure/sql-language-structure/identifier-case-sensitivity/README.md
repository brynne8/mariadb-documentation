# Identifier Case-sensitivity

Whether objects are case-sensitive or not is partly determined by the underlying operating system. Unix-based systems are case-sensitive, Windows is not, while Mac OS X is usually not, but can be if UFS volumes are used.

Database, table, table aliases and [trigger](/programming-customizing-mariadb/triggers-events/triggers) names are affected by the systems case-sensitivity, while index, column, column aliases, [stored routine](/kb/en/stored-programs-and-views/) and [event](/programming-customizing-mariadb/triggers-events/event-scheduler/events) names are never case sensitive.

Log file group name are case sensitive.

The [lower_case_table_names](/kb/en/server-system-variables/#lower_case_table_names) server system variable plays a key role. It determines whether table names, aliases and database names are compared in a case-sensitive manner. If set to 0 (the default on Unix-based systems), table names and aliases and database names are compared in a case-sensitive manner. If set to 1 (the default on Windows), names are stored in lowercase and not compared in a case-sensitive manner. If set to 2 (the default on Mac OS X), names are stored as declared, but compared in lowercase.

It is thus possible to make Unix-based systems behave like Windows and ignore case-sensitivity, but the reverse is not true, as the underlying Windows filesystem can not support this.

Even on case-insensitive systems, you are required to use the same case consistently within the same statement. The following statement fails, as it refers to the table name in a different case.

```sql
SELECT * FROM a_table WHERE A_table.id>10;
```

For a full list of identifier naming rules, see [Identifier Names](/sql-statements-structure/sql-language-structure/identifier-names).