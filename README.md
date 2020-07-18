# PostgreSQL 

Конфигурационные файлы: [postgresql.conf](https://github.com/awesomenmi/psql/blob/master/roles/postgres11-master/templates/postgresql.conf), [pg_hba.conf](https://github.com/awesomenmi/psql/blob/master/roles/postgres11-master/templates/pg_hba.conf), [recovery.conf](https://github.com/awesomenmi/psql/blob/master/roles/postgres11-slave/templates/recovery.conf), [barman.conf](https://github.com/awesomenmi/psql/blob/master/roles/barman/files/barman.conf)

```
vagrant ssh backup
```

```
[root@backup vagrant]# barman switch-wal --force --archive master.local
The WAL file 000000010000000000000001 has been closed on server 'master.local'
Waiting for the WAL file 000000010000000000000001 from server 'master.local' (max: 30 seconds)
Processing xlog segments from streaming for master.local
	000000010000000000000001
[root@backup vagrant]# barman check master.local
Server master.local:
	PostgreSQL: OK
	superuser or standard user with backup privileges: OK
	PostgreSQL streaming: OK
	wal_level: OK
	replication slot: OK
	directories: OK
	retention policy settings: OK
	backup maximum age: OK (no last_backup_maximum_age provided)
	compression settings: OK
	failed backups: OK (there are 0 failed backups)
	minimum redundancy requirements: OK (have 0 backups, expected at least 0)
	pg_basebackup: OK
	pg_basebackup compatible: OK
	pg_basebackup supports tablespaces mapping: OK
	systemid coherence: OK (no system Id stored on disk)
	pg_receivexlog: OK
	pg_receivexlog compatible: OK
	receive-wal running: OK
	archiver errors: OK
```
---

```
vagrant ssh master
```
```
[root@master vagrant]# sudo -iu postgres psql -x -c "select * from pg_stat_replication;"
-[ RECORD 1 ]----+------------------------------
pid              | 8347
usesysid         | 16385
usename          | streaming_barman
application_name | barman_receive_wal
client_addr      | 10.0.10.4
client_hostname  | 
client_port      | 36348
backend_start    | 2020-07-18 15:22:01.897704+00
backend_xmin     | 
state            | streaming
sent_lsn         | 0/2000108
write_lsn        | 0/2000060
flush_lsn        | 0/2000000
replay_lsn       | 
write_lag        | 00:00:06.948414
flush_lag        | 00:04:58.466727
replay_lag       | 00:35:28.802531
sync_priority    | 0
sync_state       | async


```
