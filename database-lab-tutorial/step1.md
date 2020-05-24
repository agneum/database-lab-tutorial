Create a regular file to mock ZFS:
```
truncate --size 10GB disk
```{{execute}}

Prepare environment variables:

```bash
export IP_OR_HOSTNAME=127.0.0.1
export DBLAB_DISK=`pwd` /disk
```{{execute}}

Next, we have to install ZFS and Docker. If needed, you can find the detailed installation guides for Docker [here](https://docs.docker.com/install/linux/docker-ce/ubuntu/).

Install dependencies:

```bash
sudo apt-get update && sudo apt-get install -y \
  apt-transport-https \
  ca-certificates \
  curl \
  gnupg-agent \
  software-properties-common \
  postgresql-client
```

Install Docker and ZFS:

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository \
  "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) \
  stable"

sudo apt-get update && sudo apt-get install -y \
  docker-ce \
  docker-ce-cli \
  containerd.io \
  zfsutils-linux
```

Now it is time ot create a ZFS pool. 

Important: environment variable `$DBLAB_DISK` must be defined!

```bash
sudo zpool create -f \
  -O compression=on \
  -O atime=off \
  -O recordsize=8k \
  -O logbias=throughput \
  -m /var/lib/dblab/data \
  dblab_pool \
  "${DBLAB_DISK}"
```

And check the result:
```bash
sudo zfs list
```{{execute}}
