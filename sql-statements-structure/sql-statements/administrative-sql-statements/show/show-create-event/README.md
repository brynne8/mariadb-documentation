# SHOW CREATE EVENT

## Syntax

```sql
SHOW CREATE EVENT event_name
```

## Description

This statement displays the <code class="highlight fixed" style="white-space:pre-wrap">[CREATE EVENT](/sql-statements-structure/sql-statements/data-definition/create/create-event/)</code>
statement needed to re-create a given [event](/programming-customizing-mariadb/triggers-events/event-scheduler/events/), as well as the [SQL_MODE](/mariadb-administration/variables-and-modes/sql-mode/) that was used when the trigger has been created and the character set used by the connection. To find out which events are present, use [SHOW EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/show/show-events/).

The output of this statement is unreliably affected by the <a undefined>sql_quote_show_create</a> server system variable - see [http://bugs.mysql.com/bug.php?id=12719](http://bugs.mysql.com/bug.php?id=12719)

The [information_schema.EVENTS](/sql-statements-structure/sql-statements/administrative-sql-statements/system-tables/information-schema/information-schema-tables/information-schema-events-table/) table provides similar, but more complete, information.

## Examples

```sql
SHOW CREATE EVENT test.e_daily\G
*************************** 1. row ***************************
               Event: e_daily
            sql_mode: 
           time_zone: SYSTEM
        Create Event: CREATE EVENT `e_daily`
                        ON SCHEDULE EVERY 1 DAY
                        STARTS CURRENT_TIMESTAMP + INTERVAL 6 HOUR
                        ON COMPLETION NOT PRESERVE
                        ENABLE
                        COMMENT 'Saves total number of sessions then
                                clears the table each day'
                        DO BEGIN
                          INSERT INTO site_activity.totals (time, total)
                            SELECT CURRENT_TIMESTAMP, COUNT(*) 
                            FROM site_activity.sessions;
                          DELETE FROM site_activity.sessions;
                        END
character_set_client: latin1
collation_connection: latin1_swedish_ci
  Database Collation: latin1_swedish_ci
```

## See also

- [Events Overview](/kb/en/events-overview/)
- [CREATE EVENT](/sql-statements-structure/sql-statements/data-definition/create/create-event/)
- [ALTER EVENT](/programming-customizing-mariadb/triggers-events/event-scheduler/alter-event/)
- [DROP EVENT](/sql-statements-structure/sql-statements/data-definition/drop/drop-event/)