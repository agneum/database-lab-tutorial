Request a new clone:
```bash
dblab clone create \
  --username dblab_user \
  --password secret_password \
  --id qa_clone
```{{execute}}

Show tables:
```bash
export QA_CONN_STR=$(dblab clone status qa_clone | jq -r '.db.connStr')

PGPASSWORD=secret_password \
psql "${QA_CONN_STR} dbname=workshop" \
  -c '\d+'
```{{execute}}

Drop table:
```bash
PGPASSWORD=secret_password \
psql "${QA_CONN_STR} dbname=workshop" \
  -c 'drop table pgbench_accounts'
```{{execute}}

Show tables:
```bash
PGPASSWORD=secret_password psql "${QA_CONN_STR} dbname=workshop" \
  -c '\d+'
```{{execute}}


Reset clone state:
```bash
dblab clone reset qa_clone
```{{execute}}

Show tables:
```bash
PGPASSWORD=secret_password psql "${QA_CONN_STR} dbname=workshop" \
  -c '\d+'
```{{execute}}

Delete the clone:

```bash
dblab clone destroy qa_clone
```{{execute}}
