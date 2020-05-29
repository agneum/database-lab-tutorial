CI example:

```bash
dblab clone create \
  --username dblab_user \
  --password secret_password \
  --id ci_clone
```{{execute}}

Wait a few seconds and run observer in the second terminal:
```bash
export CLONE_PASSWORD=secret_password

dblab clone observe --interval-seconds 1 --max-duration-seconds 1 ci_clone &
```{{execute T2}}

Export environment variables: 
```bash
export CI_CONN_STR=dblab_clone_$(dblab clone status ci_clone | jq -r '.db.connStr')
export CLONE_PASSWORD=secret_password
exoprt PGPASSWORD=secret_password
```{{execute}}

Run easy migration:
```sql
psql "${CI_CONN_STR} dbname=workshop" -c 'select 1'
```{{execute}}

Check results:
```bash
dblab clone observe-summary
```{{execute}}

Run hard migration with locks:
```sql
psql "${CI_CONN_STR} dbname=workshop" \
-c 'create table t1 as select i, random()::text as payload from generate_series(1, 10000000) i;
alter table t1 alter column i set not null;'
```{{execute}}

Check results again:
```bash
dblab clone observe-summary
```{{execute}}
