Run initdb container:
```bash
docker start dblab_pg_initdb
```{{execute}}

Create a new table:
```bash
docker exec -it dblab_pg_initdb \
  psql -U postgres -d workshop \
    -c 'create table multiple (id integer)'
```{{execute}}

Show tables:
```bash
docker exec -it dblab_pg_initdb \
  psql -U postgres -d workshop \
    -c '\d+'
```{{execute}}

Stop initdb container:
```bash
docker stop dblab_pg_initdb
```{{execute}}

Create a new snapshot:
```bash
export DATA_STATE_AT="$(TZ=UTC date '+%Y%m%d%H%M%S')"
zfs snapshot dblab_pool@multiple
zfs set dblab:datastateat="${DATA_STATE_AT}" dblab_pool@multiple
```{{execute}}

Create a clone using new snapshot
```bash
dblab clone create \
  --username dblab_user \
  --password secret_password \
  --id multiple1
```{{execute}}

Check tables for the clone based on the new snapshot:
```
export MULTIPLE1_CONN_STR=$(dblab clone status multiple1 | jq -r '.db.connStr')

PGPASSWORD=secret_password psql "${MULTIPLE1_CONN_STR} dbname=workshop" \
  -c '\d+'
```{{execute}}

Create a clone using the old snapshot:
```bash
dblab clone create \
  --username dblab_user \
  --password secret_password \
  --id multiple2 \
  --snapshot-id dblab_pool@initdb
```{{execute}}

Check tables for the clone based on the old snapshot:
```
export MULTIPLE2_CONN_STR=$(dblab clone status multiple2 | jq -r '.db.connStr')

PGPASSWORD=secret_password psql "${MULTIPLE2_CONN_STR} dbname=workshop" \
  -c '\d+'
```{{execute}}

Destroy clones:
```bash
dblab clone destroy multiple1
dblab clone destroy multiple2
```{{execute}}


To learn more, sign in to Database Lab Platform: https://postgres.ai/console.
