Create the first snapshot for our database:

```bash
export DATA_STATE_AT="$(TZ=UTC date '+%Y%m%d%H%M%S')"
zfs snapshot dblab_pool@initdb
zfs set dblab:datastateat="${DATA_STATE_AT}" dblab_pool@initdb
```{{execute}}

Here, we put the current data and time to `DATA_STATE_AT`, which is stored in ZFS snapshot meta data. 

This timestamp needs to correspond to a moment in time that represents the latest state of the database. 

> In a real-life case, you may need to promote your Postgres database when preparing a snapshot if it was in a "replica" state. 
>
> See [snapshot script](https://gitlab.com/postgres-ai/database-lab/-/blob/master/scripts/create_zfs_snapshot.sh) as an example how this process can be automated.
