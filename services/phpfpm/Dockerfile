FROM php:7-fpm
#RUN apt-get update -y && apt-get install -y libpng-dev curl libcurl4-openssl-dev
#RUN docker-php-ext-install pdo pdo_mysql gd curl
#RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli
RUN docker-php-ext-install mysqli
RUN apt-get update && apt-get install -y --no-install-recommends git unzip
RUN curl --silent --show-error https://getcomposer.org/installer -o composer-setup.php && php composer-setup.php --install-dir=/usr/local/bin --filename=composer
