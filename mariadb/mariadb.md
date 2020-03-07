## Pull The Image

`$ docker image pull mariadb:10.5`

## Rename tag

`$ docker image tag mariadb:10.5 mdb:latest`

## Run

```
$ docker container run --name mdb -e MYSQL_ROOT_PASSWORD=your-new-pwd -d --rm mdb
$ docker container exec -it mdb bash
...# mysql -p -e "SHOW DATABASES;"

Enter password: 
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+

...# mysql -u root -p
Enter password:  
. . .
MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.001 sec)

MariaDB [(none)]> 
```

## Create a Database

```
MariaDB [(none)]> CREATE DATABASE Practice;
Query OK, 1 row affected (0.003 sec)

MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| Practice               |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.001 sec)

MariaDB [(none)]>
```

## Create a Table

```
MariaDB [(none)]> USE Practice;
Database changed
MariaDB [Practice]> CREATE TABLE Done(
    -> id INT NOT NULL AUTO_INCREMENT,
    -> task VARCHAR(100) NOT NULL,
    -> PRIMARY KEY (id));
Query OK, 0 rows affected (0.020 sec)

MariaDB [Practice]> SHOW TABLES;
+--------------------+
| Tables_in_Practice |
+--------------------+
| Done               |
+--------------------+
1 row in set (0.001 sec)

MariaDB [Practice]> DESCRIBE Done;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| task  | varchar(100) | NO   |     | NULL    |                |
+-------+--------------+------+-----+---------+----------------+
2 rows in set (0.002 sec)

MariaDB [Practice]> 

```
## Persist Changes


