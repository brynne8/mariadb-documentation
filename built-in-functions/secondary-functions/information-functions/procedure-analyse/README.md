# PROCEDURE ANALYSE

## Syntax

```sql
analyse([max_elements[,max_memory]])
```

## Description

This procedure is defined in the sql/sql_analyse.cc file. It examines
the result from a query and returns an analysis of the results that
suggests optimal data types for each column. To obtain this analysis,
append PROCEDURE ANALYSE to the end of a [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select) statement:

```sql
SELECT ... FROM ... WHERE ... PROCEDURE ANALYSE([max_elements,[max_memory]])
```

For example:

```sql
SELECT col1, col2 FROM table1 PROCEDURE ANALYSE(10, 2000);
```

The results show some statistics for the values returned by the query,
and propose an optimal data type for the columns. This can be helpful
for checking your existing tables, or after importing new data. You
may need to try different settings for the arguments so that PROCEDURE
ANALYSE() does not suggest the ENUM data type when it is not
appropriate.

The arguments are optional and are used as follows:

- max_elements (default 256) is the maximum number of distinct values that analyse notices per column. This is used by analyse to check whether the optimal data type should be of type ENUM; if there are more than max_elements distinct values, then ENUM is not a suggested type.
- max_memory (default 8192) is the maximum amount of memory that analyse should allocate per column while trying to find all distinct values.

## See Also

- [PROCEDURE](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/procedure)
- [SELECT](/sql-statements-structure/sql-statements/data-manipulation/selecting-data/select)