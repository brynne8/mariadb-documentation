# Operator Precedence

The precedence is the order in which the SQL operators are evaluated.

The following list shows the SQL operator precedence. Operators that appear first in the list have a higher precedence. Operators which are listed together have the same precedence.

- [INTERVAL](/built-in-functions/date-time-functions/date-and-time-units)
- [BINARY](/built-in-functions/string-functions/binary-operator), <a undefined>COLLATE</a>
- [!](/sql-statements-structure/operators/logical-operators/not)
- [-](/sql-statements-structure/operators/arithmetic-operators/subtraction-operator-) (unary minus), [[bitwise-not|]] (unary bit inversion)
- `||` (string concatenation)
- [^](/built-in-functions/secondary-functions/bit-functions-and-operators/bitwise-xor)
- [*](/built-in-functions/numeric-functions/multiplication-operator), [/](/built-in-functions/numeric-functions/division-operator), [DIV](/built-in-functions/numeric-functions/div), [%](/built-in-functions/numeric-functions/modulo-operator), [MOD](/built-in-functions/numeric-functions/mod)
- [-](/sql-statements-structure/operators/arithmetic-operators/subtraction-operator-), [+](/built-in-functions/numeric-functions/addition-operator)
- [&lt;&lt;](/built-in-functions/secondary-functions/bit-functions-and-operators/shift-left), [&gt;&gt;](/built-in-functions/secondary-functions/bit-functions-and-operators/shift-right)
- <a undefined>&amp;</a>
- [|](/built-in-functions/secondary-functions/bit-functions-and-operators/bitwise-or)
- [=](/sql-statements-structure/operators/comparison-operators/equal) (comparison), [&lt;=&gt;](/sql-statements-structure/operators/comparison-operators/null-safe-equal), [&gt;=](/sql-statements-structure/operators/comparison-operators/greater-than-or-equal), [&gt;](/sql-statements-structure/operators/comparison-operators/greater-than), [&lt;=](/sql-statements-structure/operators/comparison-operators/less-than-or-equal), [&lt;](/sql-statements-structure/operators/comparison-operators/less-than), [&lt;&gt;](/sql-statements-structure/operators/comparison-operators/not-equal), [!=](/sql-statements-structure/operators/comparison-operators/not-equal), [IS](/sql-statements-structure/operators/comparison-operators/is), [LIKE](/built-in-functions/string-functions/like), [REGEXP](/built-in-functions/string-functions/regular-expressions-functions/regexp), [IN](/sql-statements-structure/operators/comparison-operators/in)
- [BETWEEN](/sql-statements-structure/operators/comparison-operators/between-and), [`CASE`, `WHEN`, `THEN`, `ELSE`, `END`](/built-in-functions/control-flow-functions/case-operator)
- [NOT](/sql-statements-structure/operators/logical-operators/not)
- [&amp;&amp;](/sql-statements-structure/operators/logical-operators/and), [AND](/sql-statements-structure/operators/logical-operators/and)
- [XOR](/sql-statements-structure/operators/logical-operators/xor)
- [||](/sql-statements-structure/operators/logical-operators/or) (logical or), [OR](/sql-statements-structure/operators/logical-operators/or)
- [=](/sql-statements-structure/operators/assignment-operators/assignment-operators-assignment-operator) (assignment), <a undefined>:=</a>

Functions precedence is always higher than operators precedence.

In this page `CASE` refers to the [CASE operator](/built-in-functions/control-flow-functions/case-operator), not to the [CASE statement](/programming-customizing-mariadb/programmatic-compound-statements/case-statement)`.`

If the `HIGH_NOT_PRECEDENCE` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) is set, `NOT` has the same precedence as `!`.

The `||` operator's precedence, as well as its meaning, depends on the `PIPES_AS_CONCAT` [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode) flag: if it is on, `||` can be used to concatenate strings (like the [CONCAT()](/built-in-functions/string-functions/concat) function) and has a higher precedence.

The `=` operator's precedence depends on the context - it is higher when `=` is used as a comparison operator.

[Parenthesis](/kb/en/parenthesis/) can be used to modify the operators precedence in an expression.

## Short-circuit evaluation

The `AND`, `OR`, `&amp;&amp;` and `||` operators support short-circuit evaluation. This means that, in some cases, the expression on the right of those operators is not evaluated, because its result cannot affect the result. In the following cases, short-circuit evaluation is used and `x()` is not evaluated:

- `FALSE AND x()`
- `FALSE &amp;&amp; x()`
- `TRUE OR x()`
- `TRUE || x()`
- `NULL BETWEEN x() AND x()`

Note however that the short-circuit evaluation does <em>not</em> apply to `NULL AND x()`. Also, `BETWEEN`'s right operands are not evaluated if the left operand is `NULL`, but in all other cases all the operands are evaluated.

This is a speed optimization. Also, since functions can have side-effects, this behavior can be used to choose whether execute them or not using a concise syntax:

```sql
SELECT some_function() OR log_error();
```