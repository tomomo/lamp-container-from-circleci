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

RUN \
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

RUN \
 sudo a2dissite 000-default && \
 sudo a2enmod lbmethod_byrequests rewrite && \
 sudo mkdir -p /usr/local/etc/ssl/certificate && \
 sudo chown www-data /usr/local/etc/ssl/certificate && \
 sudo chmod 400 /usr/local/etc/ssl/certificate && \
 sudo openssl req \
 -x509 -days 720 -nodes -newkey rsa:2048 \
 -subj "/C=JP/ST=Tokyo/L=Minato-ku/O=None/OU=Org/CN=localhost" \
 #-out    /etc/ssl/certificate/localhost+1.pem \
 #-keyout /etc/ssl/certificate/localhost+1-key.pem && \
 #SSLCertificateFile    /etc/ssl/certs/localhost.crt      # CERTIFICATE
 #SSLCertificateKeyFile /etc/ssl/private/localhost.key    # PRIVATE KEY
 -out    /usr/local/etc/ssl/certificate/localhost.crt \
 -keyout /usr/local/etc/ssl/certificate/localhost.key && \
 sudo a2enmod ssl

# @REVIEW Do not use "available sites", put directly on "sites-enabled".
COPY ./container/webapp-php70/apache2/sites-available /etc/apache2/sites-enabled

CMD ["sudo", "apachectl", "-D", "FOREGROUND"]
