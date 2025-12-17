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

## Index bloat

Get a list of indexes where current size is higher than expected size (using a 40 bytes per tuple estimation):
```
select
    c.relname as index_name
    ,pg_relation_size(c.oid) as actual_size_byte
    ,pg_size_pretty(pg_relation_size(c.oid)) as actual_size
    ,pg_size_pretty((c.reltuples * 40)::bigint) as expected_size
    ,round((pg_relation_size(c.oid) / nullif(c.reltuples * 40, 0))::numeric, 1) as bloat_ratio
from pg_class c
join pg_index i on c.oid = i.indexrelid
where c.relkind = 'i'
  and c.reltuples > 0
  and c.relname not like 'pg_%'
  and pg_relation_size(c.oid) > 1024 * 1024  -- only indexes > 1 mb
order by actual_size_byte desc nulls last;
```

Can then re-index them (this doesn't hold a lock on the table):
```
set statement_timeout = 0; -- Just in case you have a global statement timeout
reindex index concurrently <INDEX_NAME>;
```

Thanks to https://boringsql.com/posts/vacuum-is-lie/