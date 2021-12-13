A example of how database migrations (DB schema changes) can be automatically checked in CI/CD:

```bash
dblab clone create \
  --username dblab_user \
  --password secret_password \
  --id ci_clone
```{{execute T1}}

Wait a few seconds and run observer in the second terminal:
```bash
export CLONE_PASSWORD=secret_password

dblab clone observe \
  --interval-seconds 1 \
  --max-lock-duration-seconds 1 \
  --max-duration-seconds 300 \
  -f ci_clone
```{{execute T2}}

Return to the first terminal and export environment variables: 
```bash
export CI_CONN_STR=$( \
  dblab clone status ci_clone \
    | jq -r '.db.connStr' \
)
export CLONE_PASSWORD=secret_password
export PGPASSWORD=secret_password
```{{execute T1}}

Run easy migration:
```bash
psql "${CI_CONN_STR} dbname=workshop" \
  -c 'select 1'
```{{execute T1}}

Check results:
```bash
dblab clone observe-summary
```{{execute T1}}

Run a "bad" migration that implies dangerous locks:
```bash
psql "${CI_CONN_STR} dbname=workshop" \
  -c 'create table t1 as
         select i, random()::text as payload
         from generate_series(1, 5000000) i;
      alter table t1 alter column i set not null;'
```{{execute T1}}

Check results again:
```bash
dblab clone observe-summary
```{{execute T1}}

Send Ctrl+C to the running observer:
`echo "Ctrl+C"`{{execute interrupt T2}}

Return to the first terminal and delete the clone:

```bash
dblab clone destroy ci_clone
```{{execute T1}}
