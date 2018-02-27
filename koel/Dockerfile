FROM php:7.0.7-apach

RUN apt-get update

RUN apt-get install git -y

ENV TERM="xterm" \
    DB_HOST=mysql \
    DB_DATABASE=koel \
    DB_USERNAME=root\
    DB_PASSWORD=""\
    ADMIN_EMAIL=""\
    ADMIN_NAME=Admin\
    ADMIN_PASSWORD=""\
    APP_DEBUG=false\
    AP_ENV=production
    
RUN git clone http://github.com/phanan/koel.git /var/www/html

RUN cd /var/www/html \ 
    git checkout v2.2.1
    
RUN apt-get install npm -y \
    php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" \
    php -r "if (hash_file('SHA384', 'composer-setup.php') === '070854512ef404f16bac87071a6db9fd9721da1684cd4589b1196c3faf71b9a2682e2311b36a5079825e155ac7ce150d') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" \
    php composer-setup.php \
    php -r "unlink('composer-setup.php');" \
    php composer.phar install
    
COPY files/.env /var/www/html/.env

RUN docker-php-ext-install \
    pdo_mysql \
    pdo_pgsql \
    gd
RUN php artisan migrate
RUN php artisan koel:init
RUN php artisan serve
