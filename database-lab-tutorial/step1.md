Create a regular file to mock ZFS:
```
truncate --size 10GB disk
```{{execute}}

Next, we have to install ZFS.

Install dependencies:

```bash
sudo apt-get update && sudo apt-get install -y \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg-agent \
  postgresql-client \
  software-properties-common \
  zfsutils-linux
```{{execute}}

Now it is time ot create a ZFS pool. 

> Important: environment variable `$DBLAB_DISK` must be defined!

```bash
zpool create -f \
  -O compression=on \
  -O atime=off \
  -O recordsize=8k \
  -O logbias=throughput \
  -m /var/lib/dblab/data \
  dblab_pool \
  "`pwd`/disk"
```{{execute}}

And check the result:
```bash
zfs list
```{{execute}}
