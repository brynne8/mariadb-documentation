# Performance Schema setup_consumers Table

Lists the types of consumers for which event information is available.

The `setup_consumers` table contains the following columns:

<table><tbody><tr><th>Column</th><th>Description</th></tr>
<tr><td><code>NAME</code></td><td>Consumer name</td></tr>
<tr><td><code>ENABLED</code></td><td><code>YES</code> or <code>NO</code> for whether or not the consumer is enabled. You can modify this column to ensure that event information is added, or is not added.</td></tr>
</tbody></table>

The table can be modified directly, or the server started with the option enabled, for example:

```sql
performance-schema-consumer-events-waits-history=ON
```

## Example

```sql
SELECT * FROM performance_schema.setup_consumers;

+--------------------------------+---------+
| NAME                           | ENABLED |
+--------------------------------+---------+
| events_stages_current          | NO      |
| events_stages_history          | NO      |
| events_stages_history_long     | NO      |
| events_statements_current      | YES     |
| events_statements_history      | NO      |
| events_statements_history_long | NO      |
| events_waits_current           | NO      |
| events_waits_history           | NO      |
| events_waits_history_long      | NO      |
| global_instrumentation         | YES     |
| thread_instrumentation         | YES     |
| statements_digest              | YES     |
+--------------------------------+---------+
```