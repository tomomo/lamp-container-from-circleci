FROM circleci/php:8.0-apache-node-browsers

LABEL maintainer "tomomo <eclairpark@gmail.com>"

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_RUN_DIR /var/run/apache2
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2

RUN sudo cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
 sudo apt-get install -y sqlite3 libsqlite3-dev && \
 sudo apt-get update -y && \
 sudo apt-get upgrade -y && \
 sudo apt-get clean

RUN \
 sudo mkdir -p /etc/ssl/certificate && \
 sudo chown www-data /etc/ssl/certificate && \
 sudo chmod 400 /etc/ssl/certificate && \
 sudo openssl req \
 -x509 -days 720 -nodes -newkey rsa:2048 \
 -subj "/C=JP/ST=Tokyo/L=Minato-ku/O=None/OU=Org/CN=localhost" \
 -out    /etc/ssl/certificate/localhost+1.pem \
 -keyout /etc/ssl/certificate/localhost+1-key.pem

RUN \
 sudo a2enmod ssl && \
 sudo a2enmod lbmethod_byrequests rewrite && \
 sudo a2dissite 000-default

# @REVIEW Do not use "available sites", put directly on "sites-enabled".
COPY ./apache2/sites-available /etc/apache2/sites-enabled

CMD ["sudo", "apachectl", "-D", "FOREGROUND"]
