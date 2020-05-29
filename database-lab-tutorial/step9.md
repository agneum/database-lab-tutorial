DBA way:

```bash
dblab clone create \
  --username dblab_user \
  --password secret_password \
  --id dba_clone
```{{execute}}

Wait a few seconds and enter the running container: 
```bash
export DBA_CLONE_NAME=dblab_clone_$(dblab clone status dba_clone | jq -r '.db.port')

docker exec -it ${DBA_CLONE_NAME} bash 
```{{execute}}

Connect to the database:
```
psql -U dblab_user -d workshop
```{{execute}}

Turn off the autovacuum:
```
alter system set autovacuum = off; 
select pg_reload_conf();
```{{execute}}

Update rows:
```
update pgbench_accounts set aid = aid where aid < 1000000;
```{{execute}}

Check the table size:
```
SELECT pg_size_pretty (pg_database_size ('workshop'));
```{{execute}}

Vacuum the table:
```
vacuum full;
```{{execute}}

Check the table size again:
```
SELECT pg_size_pretty (pg_database_size ('workshop'));
```{{execute}}

Quit from psql and the container:
```
\q
exit
```{{execute}}
