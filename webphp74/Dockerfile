FROM php:7.4-apache

RUN apt-get update && apt-get install -y \
        mariadb-client curl wget git zip iproute2 npm
RUN pecl install xdebug && docker-php-ext-enable xdebug
RUN pecl install redis && docker-php-ext-enable redis
RUN docker-php-ext-install mysqli pdo pdo_mysql gettext
RUN a2enmod expires

RUN wget https://composer.github.io/installer.sig -O - -q | tr -d '\n' > installer.sig \
&& php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
&& php -r "if (hash_file('SHA384', 'composer-setup.php') === file_get_contents('installer.sig')) { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
&& php composer-setup.php && mv composer.phar /usr/bin/composer \
&& php -r "unlink('composer-setup.php'); unlink('installer.sig');"

COPY lggr/ /var/www/html/
RUN ls -l /var/www/html
RUN cd /var/www/html && composer --no-interaction install