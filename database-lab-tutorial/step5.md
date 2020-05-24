Install Database Lab client CLI. This can be done on any machine, you just need to be able to reach your Database Lab API (`https://IP_OR_HOSTNAME` in this example) from that machine:

```bash
curl https://gitlab.com/postgres-ai/database-lab/-/raw/master/scripts/cli_install.sh | bash
sudo mv ~/.dblab/dblab /usr/local/bin/dblab
```

Initialize CLI (`$IP_OR_HOSTNAME` should be defined in Step 1) and allow HTTP
enabling `insecure` option in the config (not recommended for real-life use):

```bash
dblab init \
  --environment-id=tutorial \
  --url=http://$IP_OR_HOSTNAME:2345 \
  --token=secret_token \
  --insecure
```

Check:

```bash
dblab instance status
```
