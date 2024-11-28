# Postgres

## Check blocked queries

This gives you the list of blocked queries, and the PID of the query blocking them:
```
select pid, usename, pg_blocking_pids(pid) as blocked_by, query as blocked_query
from pg_stat_activity
where cardinality(pg_blocking_pids(pid)) > 0;
```

Can then inspect the blocking query:
```
select * from pg_catalog.pg_stat_activity where pid = $PID;
```
