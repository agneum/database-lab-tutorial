Check the Database Lab configuration:

```bash
cat ~/.dblab/engine/configs/server.yml
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
  --volume ~/.dblab/engine/configs:/home/dblab/configs:ro \
  --volume ~/.dblab/engine/meta:/home/dblab/meta \
  --detach \
  --env DOCKER_API_VERSION=1.39 \
  registry.gitlab.com/postgres-ai/database-lab/dblab-server:master-ad697bb2
```{{execute}}

Observe the logs:
```bash
docker logs dblab_server
```{{execute}}

Now we can check the status of the Database Lab server using a simple API call:
```bash
curl \
  --include \
  --request GET \
  --header 'Verification-Token: secret_token' \
  http://localhost:2345/status
```{{execute}}
