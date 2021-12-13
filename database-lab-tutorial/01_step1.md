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

Create a regular file to mock ZFS:
```bash
truncate --size 10GB disk
```{{execute}}

Now it is time to create a ZFS pool. 
```bash
zpool create -f \
  -O compression=on \
  -O atime=off \
  -O recordsize=8k \
  -O logbias=throughput \
  -m /var/lib/dblab/data \
  dblab_pool \
  "$(pwd)/disk"
```{{execute}}

And check the result:
```bash
zfs list
df -hT
```{{execute}}
