Database Lab aims to boost software development and testing processes via
enabling ultra-fast provisioning of multi-terabyte databases.

Our steps:

1. prepare a machine with ZFS,
1. generate some PostgreSQL database for testing purposes,
1. prepare at least one snapshot to be used for cloning,
1. configure and launch the Database Lab server,
1. setup client CLI,
1. and, finally, start using its API and client CLI for the fast cloning
of the Postgres database.

If you want to use any cloud platform (like AWS or GCP) or run your Database Lab
on VMWare, or on bare metal, only the first step will slightly differ.
In general, the overall procedure will be pretty much the same.
