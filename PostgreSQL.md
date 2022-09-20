# PostgreSQL

## Install pgclient

```sh
apt search postgresql-client
sudo apt install postgresql-client-14
```

## Dump DB

```sh
DB_HOST=example.com
DB_USER=user
DB_NAME=database
DATE=$(date +%Y-%m-%d_%H%M%S)
BACKUP_DIR=$DB_NAME_$DATE
pg_dump -h $DB_HOST -U $DB_USER -d $DB_NAME -j 8 -Fd -f $BACKUP_DIR -v
pg_restore -h localhost -p 5432 -U $DB_USER -d $DB_NAME $BACKUP_DIR
```

## Restore DB

```sh
pg_restore -h localhost -p 5432 -U $DB_USER -d $DB_NAME $BACKUP_DIR
```
