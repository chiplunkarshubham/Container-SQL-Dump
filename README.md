Create a VM and install mysql server
====================================
```
sudo apt update
sudo apt install mysql-server
service mysql status
```

Open the mysql terminal using command
=====================================
$ mysql

mysql>> Create database,tables and insert data in tables

mysql>> exit


Come out of mysql and enter the below commands to take the sqldump
==================================================================
Generic: ```$ mysql -h IP -u root -proot shubhamdb > mysqldump.sql```

In our case: 
```$ mysql shubhamdb > mysqldump.sql```


Dockerfile
==========
```
FROM mysql/mysql-server:latest
ADD mysqlcode.sh /docker-entrypoint-initdb.d/mysqlcode.sh
ADD mysqldump.sql /home/mysqldump.sql
RUN chmod -R 775 /docker-entrypoint-initdb.d
ENV MYSQL_ROOT_PASSWORD root
```

mysqlcode.sh
============
```
#!/bin/bash
mysql -u root -proot -e "CREATE DATABASE IF NOT EXISTS shubham" && mysql -u root -proot shubham < /home/mysqldump.sql
```

Build the docker image
======================
```
docker build -t shubham .
```

Run the docker image
====================
```
docker run --name MyDB -d shubham
```
