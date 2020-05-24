Install Database Lab client CLI.

```bash
curl https://gitlab.com/postgres-ai/database-lab/-/raw/master/scripts/cli_install.sh | bash
```{{execute}}

Initialize CLI and allow HTTP
enabling `insecure` option in the config (not recommended for real-life use):

```bash
dblab init \
  --environment-id=tutorial \
  --url=http://127.0.0.1:2345 \
  --token=secret_token \
  --insecure
```{{execute}}

Check:

```bash
dblab instance status
```{{execute}}
