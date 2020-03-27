# mysql-master-slave


first, run `docker-compose up` to start the docker

## config master db

then, go into docker of master db
```shell
docker exec -it mysql-master mysql -uroot -p
```
the password is `pwd123`

and add an account for master-slave

```shell
CREATE USER 'slave'@'%' IDENTIFIED BY 'pwd123';
GRANT REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'slave'@'%';
show master status;
```
here remember the File and Position. we suppose File is mysql-bin.000003, and Position is 834

## config slave db

go into docker of master db
```shell
docker exec -it mysql-slave mysql -uroot -p
```

then run the sql below:
```shell
change master to master_host='mysql-master', master_user='slave', master_password='pwd123', master_port=3306, master_log_file='mysql-bin.000003', master_log_pos=834, master_connect_retry=30;
```
here, master_log_pos and master_log_file should be the same with master result
then 
```shell
show slave status \G;
```
if the result is not emtpy, it is a good news;
and `SlaveIORunning` and `SlaveSQLRunning` should be `No` because we have not start it. run sql below to start it
```shell
start slave;
show slave status \G;
then `SlaveIORunning` and `SlaveSQLRunning` should be `YES`. then run the sql below on master db to test it
```

```shell
  use test;
  CREATE TABLE users (
    `id` int(11) unsigned  AUTO_INCREMENT,
    `name` varchar(64) ,
    PRIMARY KEY (`id`)
  ) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci;
```

and then go to slave db to see if there is a table use in database test;

referencing to 
https://blog.csdn.net/fighterandknight/article/details/80601968
https://www.cnblogs.com/songwenjie/p/9371422.html
