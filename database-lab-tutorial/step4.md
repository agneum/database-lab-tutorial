Prepare the Database Lab configuration:

```bash
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
  accessHost: "127.0.0.1"

  # Auto-delete clones after the specified minutes of inactivity.
  # 0 - disable automatic deletion.
  maxIdleMinutes: 20

debug: true

CONFIG
```{{execute}}

Launch your Database Lab instance:

```bash
docker run \
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
```{{execute}}

Observe the logs:

```bash
docker logs dblab_server -f
```{{execute}}

Now we can check the status of the Database Lab server using a simple API call:
```bash
curl \
  --include \
  --request GET \
  --header 'Verification-Token: secret_token' \
  http://localhost:2345/status
```{{execute}}
