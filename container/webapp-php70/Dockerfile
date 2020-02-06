FROM circleci/php:7.0-apache-node-browsers

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
 sudo pecl install imagick redis \
  && \
 sudo apt-get clean

RUN NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
 sudo docker-php-ext-configure \
  gd \
    --with-gd \
    --with-freetype-dir=/usr/include/ \
    --with-png-dir=/usr/include/ \
    --with-jpeg-dir=/usr/include/ \
  && \
 sudo docker-php-ext-enable imagick redis \
  && \
 sudo docker-php-ext-install -j${NPROC} \
 bcmath \
 ctype \
 curl \
 exif \
 json \
 xml \
 gd \
 intl \
 mbstring \
 tokenizer \
 mysqli pdo_mysql \
 pgsql pdo_pgsql

COPY apache2/sites-available /etc/apache2/sites-available

CMD ["sudo", "apachectl", "-D", "FOREGROUND"]
