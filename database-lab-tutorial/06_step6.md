Request a new clone:
```bash
dblab clone create \
  --username dblab_user_1 \
  --password secret_password \
  --id my_first_clone
```{{execute}}

A few seconds later, if everything is configured correctly, you will see that the clone is ready to be used.

Now you can work with this clone using any PostgreSQL client – say, `psql`:
```bash
PGPASSWORD=secret_password \
  psql "host=127.0.0.1 port=6000 user=dblab_user_1 dbname=workshop" \
  -c '\l+'
```{{execute}}

Exit from the view:
```
q
```{{execute}}

To see the full information about the Database Lab instance, including the list of all currently available clones:
```bash
dblab instance status
```{{execute}}

Create more clones. Many more:
```bash
dblab clone create --username dblab_user_1 --password secret_password --id clone1
dblab clone create --username dblab_user_1 --password secret_password --id clone2
dblab clone create --username dblab_user_1 --password secret_password --id clone3
dblab clone create --username dblab_user_1 --password secret_password --id clone4
dblab clone create --username dblab_user_1 --password secret_password --id clone5
dblab clone create --username dblab_user_1 --password secret_password --id clone6
dblab clone create --username dblab_user_1 --password secret_password --id clone7
dblab clone create --username dblab_user_1 --password secret_password --id clone8
dblab clone create --username dblab_user_1 --password secret_password --id clone9
```{{execute}}

We have 10 independent databases now. But thanks to copy-on-write (CoW) and thin cloning, physically, the disk space consumed only once:
```bash
df -hT
```{{execute}}

Finally, let's manually delete all the clones:
```bash
dblab clone destroy my_first_clone
dblab clone destroy clone1
dblab clone destroy clone2
dblab clone destroy clone3
dblab clone destroy clone4
dblab clone destroy clone5
dblab clone destroy clone6
dblab clone destroy clone7
dblab clone destroy clone8
dblab clone destroy clone9
```{{execute}}
