Let's generate some synthetic database with data directory ("PGDATA") located at `/var/lib/dblab/data`. To do so we will use standard PostgreSQL tool called `pgbench`. With scale factor `-s 100`, the database size will be ~1.4 GiB.

Alternatively, you can take an existing PostgreSQL database and just copy it to `/var/lib/dblab/data` using any convenient method.

To generate PGDATA with `pgbench`, we are going to launch a regular Docker container with Postgres temporarily. We are going to use `POSTGRES_HOST_AUTH_METHOD=trust` to allow connection without authentication; once the generation is done, the container will be stopped, after that we can modify `pg_hba.conf` to control make the configuration secure (for the sake of simplicity, we are going to skip such configuration in this demo).

```bash
sudo docker run \
  --name dblab_pg_initdb \
  --label dblab_sync \
  --env PGDATA=/var/lib/postgresql/pgdata \
  --env POSTGRES_HOST_AUTH_METHOD=trust \
  --volume /var/lib/dblab/data:/var/lib/postgresql/pgdata \
  --detach \
  postgres:12-alpine
```

Create the `test` database:
```bash
sudo docker exec -it dblab_pg_initdb psql -U postgres -c 'create database test'
```

Generate data in the `test` database using `pgbench`, then stop and delete the container:
```bash
# 10,000,000 accounts, ~1.4 GiB of data.
sudo docker exec -it dblab_pg_initdb pgbench -U postgres -i -s 100 test
```

PGDATA is ready and now it is time to delete the temporary container with Postgres:
```bash
sudo docker stop dblab_pg_initdb
sudo docker rm dblab_pg_initdb
```
