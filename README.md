# Database Backup for Docker containers
When working with docker, the data from databases is often stored as a volume, containing the raw data directory. With this container, you can easily create and restore logical backups from MySQL/MariaDB and PostgreSQL databases:

## MySQL/MariaDB

### Backup
```
docker run --rm -e MYSQL_ROOT_PASSWORD=helloworld123 -e HOST=db --link example_database:db momar/database-backup > backup.sql
```

### Restore
Warning: you need the `-i` here so you can pipe anything to docker!
```
docker run --rm -i -e MYSQL_ROOT_PASSWORD=helloworld123 -e HOST=db --link example_database:db momar/database-backup restore < backup.sql
```

## PostgreSQL

### Backup
```
docker run --rm -e POSTGRES_PASSWORD=helloworld123 -e HOST=db --link example_database:db momar/database-backup > backup.sql
```

### Restore
Warning: you need the `-i` here so you can pipe anything to docker!  
Warning: as the --clean option of pg_dumpall is not working correctly, you should only restore into a completely empty database!
```
docker run --rm -i -e POSTGRES_PASSWORD=helloworld123 -e HOST=db --link example_database:db momar/database-backup restore < backup.sql
```

<!--
## MongoDB

### Backup
```
docker run --rm -e MONGO_URI=mongodb://admin:helloworld123@db --link example_database:db momar/database-backup > backup.bin
```

### Restore
Warning: you need the `-i` here so you can pipe anything to docker!
```
docker run --rm -i -e MONGO_URI=mongodb://admin:helloworld123@db --link example_database:db momar/database-backup restore < backup.bin
```
-->

## Script Usage
Mostly applies to both backup and restore.

```
Usage: backup [dump-command-arguments]
MySQL databases: (mysqldump will be used)
 HOST                 The hostname of the database server. [required]
 PORT                 The port of the database server.
 MYSQL_ROOT_PASSWORD  The root password for the database server. [required]
PostgreSQL databases: (pg_dumpall will be used)
 HOST                 The hostname of the database server. [required]
 PORT                 The port of the database server.
 POSTGRES_PASSWORD    The password for the superuser. [required]
 POSTGRES_USER        The username of the superuser, defaults to 'postgres'.
```
