# Labels

## Syntax

```sql
label: <construct>
[label]
```

Labels are MariaDB [identifiers](/sql-statements-structure/sql-language-structure/identifier-names/) which can be used to identify a [BEGIN ... END](/programming-customizing-mariadb/programmatic-compound-statements/begin-end/) construct or a loop. They have a maximum length of 16 characters and can be quoted with backticks (i.e.., ```).

Labels have a start part and an end part. The start part must precede the portion of code it refers to, must be followed by a colon (`:`) and can be on the same or different line. The end part is optional and adds nothing, but can make the code more readable. If used, the end part must precede the construct's delimiter (`;`). Constructs identified by a label can be nested. Each construct can be identified by only one label.

Labels need not be unique in the stored program they belong to. However, a label for an inner loop cannot be identical to a label for an outer loop. In this case, the following error would be produced:

```sql
ERROR 1309 (42000): Redefining label <label_name>
```

[LEAVE](/programming-customizing-mariadb/programmatic-compound-statements/leave/) and [ITERATE](/programming-customizing-mariadb/programmatic-compound-statements/iterate/) statements can be used to exit or repeat a portion of code identified by a label. They must be in the same [Stored Routine](/kb/en/stored-programs-and-views/), [Trigger](/programming-customizing-mariadb/triggers-events/triggers/) or [Event](/programming-customizing-mariadb/triggers-events/event-scheduler/events/) which contains the target label.

Below is an example using a simple label that is used to exit a [LOOP](/programming-customizing-mariadb/programmatic-compound-statements/loop/):

```sql
CREATE PROCEDURE `test_sp`()
BEGIN
   `my_label`:
   LOOP
      SELECT 'looping';
      LEAVE `my_label`;
   END LOOP;
   SELECT 'out of loop';
END;
```

The following label is used to exit a procedure, and has an end part:

```sql
CREATE PROCEDURE `test_sp`()
`my_label`:
BEGIN
   IF @var = 1 THEN
      LEAVE `my_label`;
   END IF;
   DO something();
END `my_label`;
```