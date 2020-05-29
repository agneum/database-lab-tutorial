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
psql -U dblab_user -d pgbench_accounts
```{{execute}}

Turn off the autovacuum:
```
alter system set autovacuum = off; 
select pg_reload_conf();
```{{execute}}

Update rows:
```
update pgbench_accounts set aid = aid;
update pgbench_accounts set aid = aid; -- many times :)
```{{execute}}

Check the table size:
```
\d+ pgbench_accounts
```{{execute}}

Vacuum the table:
```
vacuum full;
```{{execute}}

Check the table size again:
```
\d+ pgbench_accounts
```{{execute}}

Quit:
```
\q
```{{execute}}
