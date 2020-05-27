CI example

Run observer
```bash
dblab clone observe
```

Run easy migration
```sql
select 1;
```
Check results

Run observer
```bash
dblab clone observe
```

Run hard migration with locks
```sql
create table t1 as select i, random()::text as payload from generate_series(1, 100000000) i;
alter table t1 alter column i set not null;
```
