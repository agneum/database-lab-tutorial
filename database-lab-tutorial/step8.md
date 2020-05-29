CI example:

```bash
dblab clone create \
  --username dblab_user \
  --password secret_password \
  --id ci_clone
```{{execute}}

Wait a few seconds and enter the running container: 
```bash
export CI_CLONE_NAME=dblab_clone_$(dblab clone status ci_clone | jq -r '.db.port')

docker exec -it ${CI_CLONE_NAME} bash 
```{{execute}}

Connect to the database:
```
psql -U dblab_user -d pgbench_accounts
```{{execute}}

Run observer:
```bash
dblab clone observe-summary
```{{execute}}

# Second terminal:

Run easy migration:
```sql
select 1;
```{{execute}}

Check results.

Run observer:
```bash
dblab clone observe-summary
```{{execute}}

Run hard migration with locks:
```sql
create table t1 as select i, random()::text as payload from generate_series(1, 100000000) i;
alter table t1 alter column i set not null;
```{{execute}}
