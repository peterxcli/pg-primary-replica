# Single-Primary PostgreSQL Replication with Docker-compose

Run primary and replica in background:
```bash
docker-compose up -d postgres_primary postgres_replica
```

Stop all services and remove volumes:
```bash
docker-compose down -v
```

## Test if is alive

```shell
$ psql postgres://user:password@localhost:5432/postgres -xc \
'select * from pg_replication_slots;'


-[ RECORD 1 ]-------+-----------------
slot_name           | replication_slot
plugin              | 
slot_type           | physical
datoid              | 
database            | 
temporary           | f
active              | t
active_pid          | 60
xmin                | 
catalog_xmin        | 
restart_lsn         | 0/3000148
confirmed_flush_lsn | 
wal_status          | reserved
safe_wal_size       | 
two_phase           | f
```

It is important to pay attention at the following printed fields (see documentation):

active —must be true indicating that replication slot is currently actively being used;
restart_lsn —must be not null, it indicates the address (LSN) of oldest WAL which still might be required by the consumer of this slot.
