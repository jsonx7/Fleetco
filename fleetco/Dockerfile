FROM php:5.6-apache

# La base Debian de esta imagen está archivada
RUN sed -i 's|deb.debian.org|archive.debian.org|g; s|security.debian.org|archive.debian.org|g; /stretch-updates/d' /etc/apt/sources.list \
 && echo 'Acquire::Check-Valid-Until "false";' > /etc/apt/apt.conf.d/99no-check-valid

RUN apt-get update \
 && apt-get install -y --no-install-recommends --allow-unauthenticated libpng-dev libjpeg-dev libfreetype6-dev libmcrypt-dev \
 && docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr --with-freetype-dir=/usr \
 && docker-php-ext-install gd mysql mysqli mbstring mcrypt \
 && rm -rf /var/lib/apt/lists/*

COPY . /var/www/html/

# Cambia las credenciales fijas por variables de entorno
RUN sed -i \
  -e 's|"database_host_name"|getenv("DB_HOST")|' \
  -e 's|"database_user_name"|getenv("DB_USER")|' \
  -e 's|"database_user_password"|getenv("DB_PASSWORD")|' \
  -e 's|"database_port_number"|getenv("DB_PORT")|' \
  -e 's|"database_name"|getenv("DB_NAME")|' \
  /var/www/html/connections/ConnectionManager.php \
 && chown -R www-data:www-data /var/www/html

EXPOSE 80
