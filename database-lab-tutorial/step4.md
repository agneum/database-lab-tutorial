If you followed the steps described above without modification, you can use the default config with only correction: you need to put the value stored in the `$IP_OR_HOSTNAME` variable to `accessHost`. You may want to inspect all configuration options and adjust if needed. Must you need to reconfigure, edit the config file `~/.dblab/configs/config.yml`, and then re-launch the container to start using the new configuration.

```bash
# Important: environment variable $IP_OR_HOSTNAME must be specified!

mkdir -p ~/.dblab/configs

cat <<CONFIG > ~/.dblab/configs/config.yml
# Database Lab server configuration.

server:
  verificationToken: "secret_token"  # Warning! Insecure value – edit it.
  port: 2345

provision:
  # Provision mode to use.
  mode: "local"

  # Subdir where PGDATA located relative to the pool root dir.
  pgDataSubdir: "/"

  # Username that will be used for Postgres management connections.
  # The user should exist.
  pgMgmtUsername: "postgres"

  # ZFS mode related parameters.
  local:
    # Which thin-clone manager to use.
    # Available options: "zfs", "lvm".
    thinCloneManager: "zfs"

    # Name of your pool (in the case of ZFS) or volume group
    # with logic volume name (e.g. "dblab_vg/pg_lv", in the case of LVM).
    pool: "dblab_pool"

    # Pool of ports for Postgres clones.
    portPool:
      from: 6000
      to: 6100

    # Clones PGDATA mount directory.
    mountDir: "/var/lib/dblab/clones"

    # Unix sockets directory for secure connection to Postgres clones.
    unixSocketDir: "/var/lib/dblab/sockets"

    # Snapshots with the suffix will not be accessible to use for cloning.
    snapshotFilterSuffix: "_pre"

    # Database Lab provisions thin clones using Docker containers, we need
    # to specify which Postgres Docker image is to be used when cloning.
    # The default is the official Postgres image
    # (See https://hub.docker.com/_/postgres).
    # Any custom Docker image that runs Postgres with PGDATA located
    # in "/var/lib/postgresql/pgdata" directory. Our Dockerfile
    # (See https://gitlab.com/postgres-ai/database-lab/snippets/1932037)
    # is recommended in case if customization is needed.
    dockerImage: "postgres:12-alpine"

cloning:
  mode: "base"

  # Host which will be specified in clone connection info.
  accessHost: "${IP_OR_HOSTNAME}"

  # Auto-delete clones after the specified minutes of inactivity.
  # 0 - disable automatic deletion.
  maxIdleMinutes: 20

debug: true

CONFIG
```

> ⚠ Make sure the address used in `accessHost` is accessible from where you are going to connect to Database Lab clones (e.g., if you are going to install Joe Bot, `accessHost` must be accessible from the machine where you install Joe).

Launch your Database Lab instance:

```bash
sudo docker run \
  --name dblab_server \
  --label dblab_control \
  --privileged \
  --publish 2345:2345 \
  --restart on-failure \
  --volume /var/run/docker.sock:/var/run/docker.sock \
  --volume /var/lib/dblab:/var/lib/dblab:rshared \
  --volume ~/.dblab/configs/config.yml:/home/dblab/configs/config.yml \
  --detach \
  postgresai/dblab-server:latest
```

Observe the logs:

```bash
sudo docker logs dblab_server -f
```

Now we can check the status of the Database Lab server using a simple API call:
```bash
curl \
  --include \
  --request GET \
  --header 'Verification-Token: secret_token' \
  http://localhost:2345/status
```

See the full API reference [here](https://postgres.ai/swagger-ui/dblab/).
