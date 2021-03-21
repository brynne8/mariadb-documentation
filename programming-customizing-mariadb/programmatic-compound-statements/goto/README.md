# GOTO

##### MariaDB starting with [10.3](/kb/en/what-is-mariadb-103/)

The GOTO statement was introduced in [MariaDB 10.3](/kb/en/what-is-mariadb-103/) for Oracle compatibility.

## Syntax

```sql
GOTO label
```

## Description

The `GOTO` statement causes the code to jump to the specified label, and continue operating from there. It is only accepted when in [Oracle mode](/kb/en/sql_modeoracle-from-mariadb-103/).

## Example

```sql
SET sql_mode=ORACLE;

DELIMITER //

CREATE OR REPLACE PROCEDURE p1 AS

BEGIN

  SELECT 1;
  GOTO label;
  SELECT 2;
  <<label>>
  SELECT 3;

END;

//

DELIMITER 

call p1();
+---+
| 1 |
+---+
| 1 |
+---+
1 row in set (0.000 sec)

+---+
| 3 |
+---+
| 3 |
+---+
1 row in set (0.000 sec)
```