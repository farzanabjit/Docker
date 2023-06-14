FROM php:8.0-apache
RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y git
RUN git clone https://github.com/Debaice06/Ansible_Projects_Ecommerce.git /var/www/html/
RUN sed -i 's/172.20.1.101/mysqldb/g' /var/www/html/index.php
