Create a new clone for DBA:
```bash
dblab clone create \
  --username dblab_user \
  --password secret_password \
  --id dba_clone
```{{execute}}

Wait a few seconds and enter the running container: 
```bash
export DBA_CLONE_PORT=$( \
  dblab clone status dba_clone | jq -r '.db.port' \
)
```{{execute}}

Connect to the database:
```bash
psql -U dblab_user -d workshop --port ${DBA_CLONE_PORT} \
  --host 127.0.0.1
```{{execute}}

Turn off the autovacuum:
```sql
alter system set autovacuum = off; 
select pg_reload_conf();
```{{execute}}

Update rows:
```sql
update pgbench_accounts set aid = aid where aid < 1000000;
```{{execute}}

Check the database size:
```
\l+ workshop
```{{execute}}

Vacuum the table:
```sql
vacuum full;
```{{execute}}

Check the database size again:
```
\l+ workshop
```{{execute}}

Quit from psql and the container:
```
\q
exit
```{{execute}}

Destroy the clone:
```
dblab clone destroy dba_clone
```{{execute}}
