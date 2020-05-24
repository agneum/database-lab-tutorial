Create the first snapshot for our database:

```bash
DATA_STATE_AT="$(TZ=UTC date '+%Y%m%d%H%M%S')"
sudo zfs snapshot dblab_pool@initdb
sudo zfs set dblab:datastateat="${DATA_STATE_AT}" dblab_pool@initdb
```

Here, we put the current data and time to `DATA_STATE_AT`, which is stored in ZFS snapshot meta data. In a real-life scenario, this timestamp needs to correspond to a moment in time that represents the latest state of the database. In other words, it has to be the timestamp of the last modifying transaction that changed something (for example, some INSERT or UPDATE).

> In a real-life case, you may need to promote your Postgres database when preparing a snapshot if it was in a "replica" state. This will dramatically improve the timing of the creation of your thin clones. See [./scripts/create_zfs_snapshot.sh](https://gitlab.com/postgres-ai/database-lab/-/blob/master/scripts/create_zfs_snapshot.sh) in the repository as an example how this process can be automated. This script has embedded logic handling replicas, pre-promoting them to "master" state during snapshot preparation. It also allows filling `DATA_STATE_AT` automatically.

Feel free to modify this script if needed. There is also an ability to add any kind of data processing during snapshot preparation; for example, you can remove sensitive data. Finally, you can have multiple ZFS snapshots at any time, but remember to ensure that at least 20% of disk space is always free.
