FROM ubuntu:latest

ENV TERM=xterm
ENV DEBIAN_FRONTEND=noninteractive

# initialize
RUN apt-get update && apt-get -y install \
                                 zip \
                                 unzip \
                                 dialog \
                                 pkg-config \
                                 wget \
                                 git \
                                 libmagickwand-dev --no-install-recommends

# php7
RUN apt-get -y install curl php7.0-cli php7.0-dev php-pear php-xml php-mysql php-curl

# mysql
RUN apt-get -y install mysql-server-5.7 && \
    service mysql start && \
    mysqladmin -u root password ''

# xdebug
RUN wget http://xdebug.org/files/xdebug-2.4.0rc2.tgz && \
    tar -xzf xdebug-2.4.0rc2.tgz && \
    cd xdebug-2.4.0RC2/ && \
    phpize && \
    ./configure --enable-xdebug && \
    make && \
    cp modules/xdebug.so /usr/lib/. && \
    touch /etc/php/7.0/cli/conf.d/20-xdebug.ini && \
    echo 'zend_extension="/usr/lib/xdebug.so"' > /etc/php/7.0/cli/conf.d/20-xdebug.ini && \
    echo 'xdebug.remote_enable=1' >> /etc/php/7.0/cli/conf.d/20-xdebug.ini && \
    cd ../

# composer & coveralls
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    composer self-update

# coveralls    
RUN wget https://github.com/satooshi/php-coveralls/releases/download/v1.0.0/coveralls.phar && \
    chmod +x coveralls.phar && \
    mv coveralls.phar /usr/local/bin/coveralls

# phpunit
RUN curl -LO --silent https://phar.phpunit.de/phpunit.phar && \
    chmod +x phpunit.phar && \
    mv phpunit.phar /usr/local/bin/phpunit
