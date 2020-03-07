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

## Persist Changes


