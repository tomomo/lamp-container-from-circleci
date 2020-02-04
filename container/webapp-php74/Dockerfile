FROM circleci/php:7.4-apache-node-browsers

LABEL maintainer "tomomo <eclairpark@gmail.com>"

ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_RUN_DIR /var/run/apache2
ENV APACHE_LOG_DIR /var/log/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2

RUN sudo cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
 sudo apt-get update -y && \
 sudo apt-get upgrade -y && \
 sudo apt-get clean

RUN sudo mkdir -p /etc/ssl/certificate && \
 sudo chown www-data /etc/ssl/certificate && \
 sudo chmod 400 /etc/ssl/certificate && \
 sudo openssl req \
 -x509 -days 720 -nodes -newkey rsa:2048 \
 -subj "/C=JP/ST=Tokyo/L=Minato-ku/O=None/OU=Org/CN=localhost" \
 -out    /etc/ssl/certificate/localhost+1.pem \
 -keyout /etc/ssl/certificate/localhost+1-key.pem && \
 sudo a2enmod ssl && \
 sudo a2ensite default-ssl
# ex) docker-compose.yml
# - .devcontainer/certificate:/etc/ssl/certificate:ro

RUN sudo a2enmod lbmethod_byrequests rewrite \
  && \
 sudo apt-get install -y \
 # CURL
 libcurl3-dev libonig-dev \
 # PostgreSQL
 libpq-dev \
 # GD
 libfreetype6-dev libjpeg62-turbo-dev libpng-dev \
 # ImageMagick
 imagemagick libmagickwand-dev \
  && \
 sudo pecl install imagick \
  && \
 sudo apt-get clean
RUN NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
 sudo docker-php-ext-configure \
 gd --with-freetype --with-jpeg \
  && \
 sudo docker-php-ext-enable imagick \
  && \
 sudo docker-php-ext-install -j${NPROC} \
 curl \
 exif \
 json \
 xml \
 gd \
 intl \
 mbstring \
 mysqli pdo_mysql \
 pgsql pdo_pgsql

COPY apache2/sites-available /etc/apache2/sites-available

CMD ["sudo", "apachectl", "-D", "FOREGROUND"]

# bcmath bz2 calendar ctype curl dba dom enchant exif ffi fileinfo filter ftp gd gettext gmp hash iconv imap intl json ldap mbstring mysqli oci8 odbc opcache pcntl pdo pdo_dblib pdo_firebird pdo_mysql pdo_oci pdo_odbc pdo_pgsql pdo_sqlite pgsql phar posix pspell readline reflection session shmop simplexml snmp soap sockets sodium spl standard sysvmsg sysvsem sysvshm tidy tokenizer xml xmlreader xmlrpc xmlwriter xsl zend_test zip