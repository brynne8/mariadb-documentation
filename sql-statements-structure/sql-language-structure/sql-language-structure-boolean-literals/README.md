# Boolean Literals

In MariaDB, `FALSE` is a synonym of 0 and `TRUE` is a synonym of 1. These constants are case insensitive, so `TRUE`, `True`, and `true` are equivalent.

These terms are not synonyms of 0 and 1 when used with the [IS](/sql-statements-structure/operators/comparison-operators/is) operator. So, for example, `10 IS TRUE` returns 1, while `10 = TRUE` returns 0 (because 1 != 10).

The `IS` operator accepts a third constant exists: `UNKNOWN`. It is always a synonym of [NULL](/kb/en/null-values-in-mariadb/).

`TRUE` and `FALSE` are [reserved words](/sql-statements-structure/sql-language-structure/reserved-words), while `UNKNOWN` is not.

## See Also

- [BOOLEAN](/columns-storage-engines-and-plugins/data-types/data-types-numeric-data-types/boolean) type