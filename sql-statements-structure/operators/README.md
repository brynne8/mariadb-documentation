# Operators

Operators can be used for comparing values or for assigning values. There are several operators and they may be used in different SQL statements and clauses. Some can be used somewhat on their own, not within an SQL statement clause.

For comparing values<span>—</span>string or numeric<span>—</span>you can use symbols such as the equal-sign (i.e., `=`) or the exclamation point and the equal-sign together (i.e., `!=`).  You might use these in `WHERE` clauses or within a flow-control statement or function (e.g., [IF( )](/built-in-functions/control-flow-functions/if-function/)).  You can also use basic regular expressions with the `LIKE ` operator.

For assigning values, you can also use the equal-sign or other arithmetic symbols (e.g. plus-sign).  You might do this with the [SET](/sql-statements-structure/sql-statements/administrative-sql-statements/set-commands/set/) statement or in a `SET` clause in an [UPDATE](/sql-statements-structure/sql-statements/data-manipulation/changing-deleting-data/update/) statement.



### [Arithmetic Operators](/sql-statements-structure/operators/arithmetic-operators/)

- [Addition Operator (+)](/built-in-functions/numeric-functions/addition-operator/) — Addition.
- [DIV](/built-in-functions/numeric-functions/div/) — Integer division.
- [Division Operator (/)](/built-in-functions/numeric-functions/division-operator/) — Division.
- [MOD](/built-in-functions/numeric-functions/mod/) — Modulo operation. Remainder of N divided by M.
- [Modulo Operator (%)](/built-in-functions/numeric-functions/modulo-operator/) — Modulo operator. Returns the remainder of N divided by M.
- [Multiplication Operator (*)](/built-in-functions/numeric-functions/multiplication-operator/) — Multiplication.
- [Subtraction Operator (-)](/sql-statements-structure/operators/arithmetic-operators/subtraction-operator-/) — Subtraction and unary minus.

### [Assignment Operators](/sql-statements-structure/operators/assignment-operators/)

- [Assignment Operator (:=)](/sql-statements-structure/operators/assignment-operators/operador-de-atribuicao/) — Assignment operator for assigning a value.
- [Assignment Operator (=)](/sql-statements-structure/operators/assignment-operators/assignment-operators-assignment-operator/) — The equal sign as an assignment operator.

### [Bit Functions and Operators](/built-in-functions/secondary-functions/bit-functions-and-operators/)

- [Operator Precedence](/sql-statements-structure/operators/operator-precedence/) — Precedence of SQL operators
- [&](/built-in-functions/secondary-functions/bit-functions-and-operators/bitwise_and/) — Bitwise AND
- [<<](/built-in-functions/secondary-functions/bit-functions-and-operators/shift-left/) — Left shift
- [>>](/built-in-functions/secondary-functions/bit-functions-and-operators/shift-right/) — Shift right
- [BIT_COUNT](/built-in-functions/secondary-functions/bit-functions-and-operators/bit_count/) — Returns the number of set bits
- [^](/built-in-functions/secondary-functions/bit-functions-and-operators/bitwise-xor/) — Bitwise XOR
- [|](/built-in-functions/secondary-functions/bit-functions-and-operators/bitwise-or/) — Bitwise OR
- [~](/built-in-functions/secondary-functions/bit-functions-and-operators/bitwise-not/) — Bitwise NOT
- [Parentheses](/built-in-functions/secondary-functions/bit-functions-and-operators/parentheses/) — Parentheses modify the precedence of other operators in an expression
- [TRUE FALSE](/built-in-functions/secondary-functions/bit-functions-and-operators/true-false/) — TRUE and FALSE evaluate to 1 and 0

### [Comparison Operators](/sql-statements-structure/operators/comparison-operators/)

- [!=](/sql-statements-structure/operators/comparison-operators/not-equal/) — Not equal operator.
- [<](/sql-statements-structure/operators/comparison-operators/less-than/) — Less than operator.
- [<=](/sql-statements-structure/operators/comparison-operators/less-than-or-equal/) — Less than or equal operator.
- [<=>](/sql-statements-structure/operators/comparison-operators/null-safe-equal/) — NULL-safe equal operator.
- [=](/sql-statements-structure/operators/comparison-operators/equal/) — Equal operator.
- [>](/sql-statements-structure/operators/comparison-operators/greater-than/) — Greater than operator.
- [>=](/sql-statements-structure/operators/comparison-operators/greater-than-or-equal/) — Greater than or equal operator.
- [BETWEEN AND](/sql-statements-structure/operators/comparison-operators/between-and/) — True if expression between two values.
- [COALESCE](/sql-statements-structure/operators/comparison-operators/coalesce/) — Returns the first non-NULL parameter
- [GREATEST](/sql-statements-structure/operators/comparison-operators/greatest/) — Returns the largest argument.
- [IN](/sql-statements-structure/operators/comparison-operators/in/) — True if expression equals any of the values in the list
- [INTERVAL](/sql-statements-structure/operators/comparison-operators/interval/) — Index of the argument that is less than the first argument
- [IS](/sql-statements-structure/operators/comparison-operators/is/) — Tests whether a boolean is TRUE, FALSE, or UNKNOWN.
- [IS NOT](/sql-statements-structure/operators/comparison-operators/is-not/) — Tests whether a boolean value is not TRUE, FALSE, or UNKNOWN
- [IS NOT NULL](/sql-statements-structure/operators/comparison-operators/is-not-null/) — Tests whether a value is not NULL
- [IS NULL](/sql-statements-structure/operators/comparison-operators/is-null/) — Tests whether a value is NULL
- [ISNULL](/sql-statements-structure/operators/comparison-operators/isnull/) — Checks if an expression is NULL
- [LEAST](/sql-statements-structure/operators/comparison-operators/least/) — Returns the smallest argument.
- [NOT BETWEEN](/sql-statements-structure/operators/comparison-operators/not-between/) — Same as NOT (expr BETWEEN min AND max)
- [NOT IN](/sql-statements-structure/operators/comparison-operators/not-in/) — Same as NOT (expr IN (value,...))

### [Logical Operators](/sql-statements-structure/operators/logical-operators/)

- [!](/sql-statements-structure/operators/logical-operators/not/) — Logical NOT.
- [&&](/sql-statements-structure/operators/logical-operators/and/) — Logical AND.
- [XOR](/sql-statements-structure/operators/logical-operators/xor/) — Logical XOR.
- [||](/sql-statements-structure/operators/logical-operators/or/) — Logical OR.

### Other Operators Articles

- [Operator Precedence](/sql-statements-structure/operators/operator-precedence/) — Precedence of SQL operators