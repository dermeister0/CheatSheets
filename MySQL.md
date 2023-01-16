# MySQL

## Back Up

```
mysqldump.exe -u root -p mydb > mydb.sql
```

```
mysqldump -h 127.0.0.1 -P 3307 -u root -p mydb > mydb.sql
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
