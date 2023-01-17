# MySQL

## Back Up

```sh
sudo apt install mysql-client-core-8.0
```

```
mysqldump.exe -u root -p mydb > mydb.sql
```

```
mysqldump -h 127.0.0.1 -P 3306 -u root -p mydb > mydb.sql
```

### MySQL 5

```
mysqldump -h 127.0.0.1 -P 3306 -u root -p --column-statistics=0 mydb > mydb.sql
```

## Restore

```
mysql.exe
mysql > drop database mydb;
mysql > create database mydb;
mysql > exit
```

```
mysql.exe -u root -p mydb < mydb.sql
```
