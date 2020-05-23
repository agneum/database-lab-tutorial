Request a new clone:

```bash
dblab clone create \
  --username dblab_user_1 \
  --password secret_password \
  --id my_first_clone
```

After a second or two, if everything is configured correctly, you will see that the clone is ready to be used. It should look like this:


Now you can work with this clone using any PostgreSQL client, for example `psql`:
```bash
PGPASSWORD=secret_password \
  psql "host=${IP_OR_HOSTNAME} port=6000 user=dblab_user_1 dbname=test" \
  -c '\l+'
```

To see the full information about the Database Lab instance, including the list of all currently available clones:

```bash
dblab instance status
```

Finally, let's manually delete the clone:

```bash
dblab clone destroy my_first_clone
```
