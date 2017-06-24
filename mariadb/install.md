### 1) Install MARIADB and start service.
```
# yum install mariadb-server -y
# systemctl start mariadb
# systemctl enable mariadb
```

### 2) Connect to DB using `mysql` command
```
# mysql

MariaDB>
```
### 3) List of DB Commands.

```
> show databases;
> use test;
## To see all tables 
> show tables;

## Select mysql DB
> use mysql;
> show tables;
> select * from user;
> select User from user;
> select Host,User from user;

## Create a new DB.
> create database student;
> drop database student;

## Create a table 
> create table students (name VARCHAR(30));
> insert into students (name) VALUES ('Rama');
```

