# Out Parameters in PREPARE

##### MariaDB [10.1.1](/kb/en/mariadb-1011-release-notes/)

Out parameters in PREPARE were only available in [MariaDB 10.1.1](/kb/en/mariadb-1011-release-notes/)

One can use question mark placeholders for out-parameters in the [PREPARE](/sql-statements-structure/sql-statements/prepared-statements/prepare-statement) statement. Only [SELECT … INTO](/kb/en/select/#into) can be used this way:

```sql
prepare test from "select id into ? from t1 where val=?";
execute test using @out, @in;
```

This is particularly convenient when used with [compound statements](/programming-customizing-mariadb/programmatic-compound-statements/using-compound-statements-outside-of-stored-programs):

```sql
PREPARE stmt FROM "BEGIN NOT ATOMIC
  DECLARE v_res INT;
  SELECT COUNT(*) INTO v_res FROM t1;
  SELECT 'Hello World', v_res INTO ?,?;
END"|
```