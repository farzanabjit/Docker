# Docker Assignment



We will deploy an e-commerce by using PHP, mysql and Docker.
 For this at first we will create
<b>“docker-compose.yml”</b> file in a project directory. This file is our main file. Under the same directory we
will make two more directory one is “conf” and another is “php” . We will also create an “.env”

file in this file we will initialize our variables for mysql database. In conf directory we will create
another directory “docker-entrypoint-initdb.d”. In this directory we will store all of our sql files,
they will be run/imported before the mysql service is available outside the container. Under the php
directory we will create Dockerfile. In this file we will write commands which will be the run
during the image creation. According to the command image will be created. After that we will run
our main “docker-compose.yml” file. According to the file the image and container will be created,
and container will be run automatically.

![image](https://github.com/farzanabjit/Docker/assets/131145991/23b5d7c2-dddc-4ccb-af42-c3abf48646ef)

#docker-compose.yml:
version: '3.8'
services:
mysqldb:
container_name: mysqldb
image: mysql:5.7
restart: always
volumes:
- db_data:/var/lib/mysql
- ./conf/docker-entrypoint-initdb.d:/docker-entrypoint-initdb.d environment:
MYSQL_ROOT_PASSWORD: "${DB_ROOT_PW}"
MYSQL_DATABASE: "${DB_NAME}"
MYSQL_USER: "${DB_USER}"
MYSQL_PASSWORD: "${DB_PW}"
ports:
- "3307:3306"
php-apache-environment:
container_name: php-apache
build:
context: ./php
dockerfile: Dockerfile
depends_on:
- mysqldb
ports:
- 90:80
volumes:
db_data:
#.env:
DB_NAME=ecomdb
DB_USER=ecomuser
DB_PW=ecompassword
DB_ROOT_PW=123456


# main.sql [Under “conf/docker-entrypoint-initdb.d” directory]:
USE ecomdb;
CREATE TABLE products (id mediumint(8) unsigned NOT NULL auto_increment,Name
varchar(255) default NULL,Price varchar(255) default NULL, ImageUrl varchar(255) default
NULL,PRIMARY KEY (id)) AUTO_INCREMENT=1;
INSERT INTO products (Name,Price,ImageUrl) VALUES ("Laptop","100","c-1.png"),
("Drone","200","c-2.png"),("VR","300","c-3.png"),("Tablet","50","c-5.png"),("Watch","90","c-
6.png"),("Phone Covers","20","c-7.png"),("Phone","80","c-8.png"),("Laptop","150","c-4.png");
note: This sql is for table creation and inserting the data on the table.
# Dockerfile [Under “php/” directory]:
FROM php:8.0-apache
RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y git
RUN git clone https://github.com/Debaice06/Ansible_Projects_Ecommerce.git /var/www/html/
RUN sed -i 's/172.20.1.101/mysqldb/g' /var/www/html/index.php# Now for running this we have to write the below command:
docker-compose up -d
![image](https://github.com/farzanabjit/Docker/assets/131145991/5c92f560-45d9-4e49-868d-101f0c8bb51b)
![image](https://github.com/farzanabjit/Docker/assets/131145991/ecc1dcae-4744-4e1f-aa99-fcbcd5d2bad9)
![image](https://github.com/farzanabjit/Docker/assets/131145991/4b923bbc-f2ed-47fc-88fd-e43fdc7b6af1)
