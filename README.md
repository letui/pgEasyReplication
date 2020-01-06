# pgEasyReplication

pgEasyReplicaton is a Java library to read data changes (INSERT/UPDATE/DELETE) in PostgreSQL tables via Logical Replication.

Nerd Informations
=================

pgEasyReplicaton uses PostgreSQL Replication API to create a replication slot that reserve WAL logs on the server and a stream replication with output plugin to read and decode the WAL logs.

The data changes are returned by the library in JSON format.

Requirements
============

* PostgreSQL 10+

Configuration
=============

postgresql.conf
---------------

Your database should be configured to enable logical replication.

* Property **max_wal_senders** should be at least equal to the number of replication consumers.
* Property **wal_keep_segments** should contain count wal segments that can't be removed from database.
* Property **wal_level** for logical replication should be equal to logical.
* Property **max_replication_slots** should be greater than zero for logical replication, because logical replication can't work without replication slot.

Example:

```
max_wal_senders = 4             # max number of walsender processes
wal_keep_segments = 4           # in logfile segments, 16MB each; 0 disables
wal_level = logical             # minimal, replica, or logical
max_replication_slots = 4       # max number of replication slots
```
After configurations, restart/reload PostreSQL service.

pg_hba.conf
-----------

Enable connect user with replication privileges to replication stream.

Example:
  
```
local   replication   all                   trust
host    replication   all   192.168.32.0/24 md5
host    replication   all   ::1/128         md5
```

After configurations, restart/reload PostreSQL service.

