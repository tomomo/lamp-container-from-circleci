FROM circleci/php:7.4-apache-node-browsers

LABEL maintainer "tomomo <eclairpark@gmail.com>"

RUN \
 sudo cp /usr/share/zoneinfo/Asia/Tokyo /etc/localtime && \
 sudo apt-get install -y sqlite3 libsqlite3-dev vim && \
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
 sudo pecl install imagick-3.4.3 redis-5.3.2 \
  && \
 sudo apt-get clean

RUN NPROC=$(grep -c ^processor /proc/cpuinfo 2>/dev/null || 1) && \
 sudo docker-php-ext-configure \
 gd --with-freetype --with-jpeg \
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
 sudo mkdir -p /etc/apache2/certs && \
 sudo chown www-data /etc/apache2/certs && \
 sudo chmod 640 /etc/apache2/certs && \
 sudo openssl req \
 -x509 -days 720 -nodes -newkey rsa:2048 \
 -subj "/C=JP/ST=Tokyo/L=Minato-ku/O=None/OU=Org/CN=localhost" \
 -out    /etc/apache2/certs/localhost.crt \
 -keyout /etc/apache2/certs/localhost.key && \
 sudo a2enmod ssl

# @REVIEW Do not use "available sites", put directly on "sites-enabled".
COPY ./apache2/sites-available /etc/apache2/sites-enabled

# ENV APACHE_RUN_USER www-data
# ENV APACHE_RUN_GROUP www-data
# ENV APACHE_PID_FILE /var/run/apache2.pid
# ENV APACHE_RUN_DIR /var/run/apache2
# ENV APACHE_LOG_DIR /var/log/apache2
# ENV APACHE_LOCK_DIR /var/lock/apache2

CMD ["sudo", "apache2-foreground"]
