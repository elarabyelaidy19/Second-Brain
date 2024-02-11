
## Performance Benchmark 
### [PG_STATS_STATEMENT](https://www.postgresql.org/docs/current/pgstatstatements.html) 
The `pg_stat_statements` module provides a means for tracking planning and execution statistics of all SQL statements executed by a server. 



## How To Enable  
- first create the pg_stat_statement extinsion for the desired database. 
- enable shared memory in postgresql.conf 
```bash   
CREATE EXTENSION pg_stat_statements; 

# postgresql.conf
shared_preload_libraries = 'pg_stat_statements'

compute_query_id = on
pg_stat_statements.max = 10000
pg_stat_statements.track = all
```
#### Query stats 
```sql 
SELECT * FROM pg_stat_statements;

# reset stats
SELECT pg_stat_statements_reset();

# top 10 queries 
SELECT * FROM pg_stat_statements ORDER BY total_time DESC LIMIT 10;
```




