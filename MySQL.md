# MySQL

## Back Up and Restore

```
mysqldump.exe -u root -p mydb > mydb.sql
```

```
mysql.exe
mysql > drop database mydb;
mysql > create database mydb;
mysql > exit
```

```
mysql.exe -u root -p mydb < mydb.sql
```
