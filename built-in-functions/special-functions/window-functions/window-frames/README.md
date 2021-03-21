# Window Frames

##### MariaDB starting with [10.2](/kb/en/what-is-mariadb-102/)

[Window functions](/built-in-functions/special-functions/window-functions) were first introduced in [MariaDB 10.2.0](/kb/en/mariadb-1020-release-notes/).

## Syntax

```sql
frame_clause:
  {ROWS | RANGE} {frame_border | BETWEEN frame_border AND frame_border}

frame_border:
  | UNBOUNDED PRECEDING
  | UNBOUNDED FOLLOWING
  | CURRENT ROW
  | expr PRECEDING
  | expr FOLLOWING
```

## Description

A basic overview of [window functions](/built-in-functions/special-functions/window-functions) is described in [Window Functions Overview](/built-in-functions/special-functions/window-functions/window-functions-overview). Window frames expand this functionality by allowing the function to include a specified a number of rows around the current row.

These include:

- All rows before the current row (UNBOUNDED PRECEDING), for example `RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW`
- All rows after the current row (UNBOUNDED FOLLOWING), for example `RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING`
- A set number of rows before the current row (expr PRECEDING) for example `RANGE BETWEEN 6 PRECEDING AND CURRENT ROW`
- A set number of rows after the current row (expr PRECEDING AND expr FOLLOWING)  for example `RANGE BETWEEN CURRENT ROW AND 2 FOLLOWING`
- A specified number of rows both before and after the current row, for example `RANGE BETWEEN 6 PRECEDING AND 3 FOLLOWING `

The following functions operate on window frames:

- [AVG](/built-in-functions/aggregate-functions/avg)
- [BIT_AND](/built-in-functions/aggregate-functions/bit_and)
- [BIT_OR](/built-in-functions/aggregate-functions/bit_or)
- [BIT_XOR](/built-in-functions/aggregate-functions/bit_xor)
- [COUNT](/built-in-functions/aggregate-functions/count)
- [LEAD](/built-in-functions/special-functions/window-functions/lead)
- [MAX](/built-in-functions/aggregate-functions/max)
- [MIN](/built-in-functions/aggregate-functions/min)
- [NTILE](/built-in-functions/special-functions/window-functions/ntile)
- [STD](/built-in-functions/aggregate-functions/std)
- [STDDEV](/built-in-functions/aggregate-functions/stddev)
- [STDDEV_POP](/built-in-functions/aggregate-functions/stddev_pop)
- [STDDEV_SAMP](/built-in-functions/aggregate-functions/stddev_samp)
- [SUM](/built-in-functions/aggregate-functions/sum)
- [VAR_POP](/built-in-functions/aggregate-functions/var_pop)
- [VAR_SAMP](/built-in-functions/aggregate-functions/var_samp)
- [VARIANCE](/built-in-functions/aggregate-functions/variance)

Window frames are determined by the <em>frame_clause</em> in the window function request.

Take the following example:

```sql
CREATE TABLE `student_test` (
  name char(10),
  test char(10),
  score tinyint(4)
);

INSERT INTO student_test VALUES 
    ('Chun', 'SQL', 75), ('Chun', 'Tuning', 73), 
    ('Esben', 'SQL', 43), ('Esben', 'Tuning', 31), 
    ('Kaolin', 'SQL', 56), ('Kaolin', 'Tuning', 88), 
    ('Tatiana', 'SQL', 87);

SELECT name, test, score, SUM(score) 
  OVER () AS total_score 
  FROM student_test;
+---------+--------+-------+-------------+
| name    | test   | score | total_score |
+---------+--------+-------+-------------+
| Chun    | SQL    |    75 |         453 |
| Chun    | Tuning |    73 |         453 |
| Esben   | SQL    |    43 |         453 |
| Esben   | Tuning |    31 |         453 |
| Kaolin  | SQL    |    56 |         453 |
| Kaolin  | Tuning |    88 |         453 |
| Tatiana | SQL    |    87 |         453 |
+---------+--------+-------+-------------+
```

By not specifying an OVER condition, the [SUM](/built-in-functions/aggregate-functions/sum) function is run over the entire dataset. However, if we specify an ORDER BY condition based on score (and order the entire result in the same way for clarity), the following result is returned:

```sql
SELECT name, test, score, SUM(score) 
  OVER (ORDER BY score) AS total_score 
  FROM student_test ORDER BY score;
+---------+--------+-------+-------------+
| name    | test   | score | total_score |
+---------+--------+-------+-------------+
| Esben   | Tuning |    31 |          31 |
| Esben   | SQL    |    43 |          74 |
| Kaolin  | SQL    |    56 |         130 |
| Chun    | Tuning |    73 |         203 |
| Chun    | SQL    |    75 |         278 |
| Tatiana | SQL    |    87 |         365 |
| Kaolin  | Tuning |    88 |         453 |
+---------+--------+-------+-------------+
```

The total_score column represents a running total of the current row, and all previous rows. The window frame in this example expands as the function proceeds.

The above query makes use of the default to define the window frame. It could be written explicitly as follows:

```sql
SELECT name, test, score, SUM(score) 
  OVER (ORDER BY score RANGE BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS total_score 
  FROM student_test ORDER BY score;
+---------+--------+-------+-------------+
| name    | test   | score | total_score |
+---------+--------+-------+-------------+
| Esben   | Tuning |    31 |          31 |
| Esben   | SQL    |    43 |          74 |
| Kaolin  | SQL    |    56 |         130 |
| Chun    | Tuning |    73 |         203 |
| Chun    | SQL    |    75 |         278 |
| Tatiana | SQL    |    87 |         365 |
| Kaolin  | Tuning |    88 |         453 |
+---------+--------+-------+-------------+
```

Let's look at some alternatives:

Firstly, applying the window function to the current row and all following rows can be done with the use of UNBOUNDED FOLLOWING:

```sql
SELECT name, test, score, SUM(score) 
  OVER (ORDER BY score RANGE BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS total_score 
  FROM student_test ORDER BY score;
+---------+--------+-------+-------------+
| name    | test   | score | total_score |
+---------+--------+-------+-------------+
| Esben   | Tuning |    31 |         453 |
| Esben   | SQL    |    43 |         422 |
| Kaolin  | SQL    |    56 |         379 |
| Chun    | Tuning |    73 |         323 |
| Chun    | SQL    |    75 |         250 |
| Tatiana | SQL    |    87 |         175 |
| Kaolin  | Tuning |    88 |          88 |
+---------+--------+-------+-------------+
```

It's possible to specify a number of rows, rather than the entire unbounded following or preceding set. The following example takes the current row, as well as the previous row:

```sql
SELECT name, test, score, SUM(score) 
  OVER (ORDER BY score ROWS BETWEEN 1 PRECEDING AND CURRENT ROW) AS total_score 
  FROM student_test ORDER BY score;
+---------+--------+-------+-------------+
| name    | test   | score | total_score |
+---------+--------+-------+-------------+
| Esben   | Tuning |    31 |          31 |
| Esben   | SQL    |    43 |          74 |
| Kaolin  | SQL    |    56 |          99 |
| Chun    | Tuning |    73 |         129 |
| Chun    | SQL    |    75 |         148 |
| Tatiana | SQL    |    87 |         162 |
| Kaolin  | Tuning |    88 |         175 |
+---------+--------+-------+-------------+
```

The current row and the following row:

```sql
SELECT name, test, score, SUM(score) 
  OVER (ORDER BY score ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING) AS total_score 
  FROM student_test ORDER BY score;
+---------+--------+-------+-------------+
| name    | test   | score | total_score |
+---------+--------+-------+-------------+
| Esben   | Tuning |    31 |          74 |
| Esben   | SQL    |    43 |         130 |
| Kaolin  | SQL    |    56 |         172 |
| Chun    | Tuning |    73 |         204 |
| Chun    | SQL    |    75 |         235 |
| Tatiana | SQL    |    87 |         250 |
| Kaolin  | Tuning |    88 |         175 |
+---------+--------+-------+-------------+
```