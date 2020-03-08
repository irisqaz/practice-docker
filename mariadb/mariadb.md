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
MariaDB [(none)]> CREATE DATABASE practice;
Query OK, 1 row affected (0.003 sec)

MariaDB [(none)]> SHOW DATABASES;
+--------------------+
| Database           |
+--------------------+
| practice               |
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
4 rows in set (0.001 sec)

MariaDB [(none)]>
```

## Create a Table

```
MariaDB [(none)]> USE practice;
Database changed
MariaDB [practice]> CREATE TABLE done(
    -> id INT NOT NULL AUTO_INCREMENT,
    -> task VARCHAR(100) NOT NULL,
    -> PRIMARY KEY (id));
Query OK, 0 rows affected (0.020 sec)

MariaDB [practice]> SHOW TABLES;
+--------------------+
| Tables_in_Practice |
+--------------------+
| done               |
+--------------------+
1 row in set (0.001 sec)

MariaDB [practice]> DESCRIBE done;
+-------+--------------+------+-----+---------+----------------+
| Field | Type         | Null | Key | Default | Extra          |
+-------+--------------+------+-----+---------+----------------+
| id    | int(11)      | NO   | PRI | NULL    | auto_increment |
| task  | varchar(100) | NO   |     | NULL    |                |
+-------+--------------+------+-----+---------+----------------+
2 rows in set (0.002 sec)

MariaDB [practice]> 

```
## Insert a row

```
MariaDB [practice]> INSERT INTO done
    -> (id, task)
    -> VALUES(1, 'walked for 1 hour');
Query OK, 1 row affected (0.013 sec)

MariaDB [practice]> SELECT * from done;
+----+-------------------+
| id | task              |
+----+-------------------+
|  1 | walked for 1 hour |
+----+-------------------+
1 row in set (0.002 sec)

MariaDB [practice]>
```


