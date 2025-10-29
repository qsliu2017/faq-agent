# List Running PostgreSQL Sessions

Use the `pg_stat_activity` system view to inspect every active backend and its current state.

```bash
psql -d my_database
```

Inside `psql`, enable expanded output so long text columns like `query` wrap cleanly.

```sql
\x on
```

Run the query below to list process identifiers, owners, client info, wait status, start times, and the SQL text.

```sql
SELECT
  pid,
  usename,
  application_name,
  client_addr,
  state,
  wait_event_type,
  wait_event,
  backend_start,
  query_start,
  query
FROM pg_stat_activity
ORDER BY state, pid;
```

To watch the output refresh automatically, rerun the selection with `\watch` and a refresh interval (in seconds).

```sql
\watch 5
```

You need `pg_monitor` role membership (or superuser) to see all rows; otherwise you only see your own sessions.
