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

Проверка работы репликации:
