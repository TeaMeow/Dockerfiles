FROM ubuntu:latest

ENV TERM=xterm
ENV DEBIAN_FRONTEND=noninteractive

COPY ruby.sh ruby.sh
COPY sass.sh sass.sh

# initialize
RUN apt-get update && apt-get -y install    \
                                 zip        \
                                 unzip      \
                                 dialog     \
                                 pkg-config \
                                 wget       \
                                 git        \
                                 nodejs     \
                                 npm        \
                                 rubygems   \
                                 ruby-dev   \
                                 libmagickwand-dev --no-install-recommends

# ruby
RUN chmod +x ruby.sh && \ 
    ruby.sh

# coffee
RUN npm install -g coffee-script && \ 
    ln -s /usr/bin/nodejs /usr/bin/node
    
# sass
RUN chmod +x sass.sh && \ 
    sass.sh

# sassc
RUN git clone https://github.com/sass/libsass.git && \
    export SASS_LIBSASS_PATH="$PWD/libsass"       && \
    git clone https://github.com/sass/sassc.git   && \
    cd ./sassc                                    && \
    make                                          && \
    chmod +x ./bin/sassc                          && \
    mv ./bin/sassc /usr/local/bin/sassc

# php7
RUN apt-get -y install php7.0-cli php7.0-dev php7.0-mbstring php7.0-curl php7.0-gd php7.0-intl php7.0-mcrypt php7.0-json php7.0-mysql php7.0-opcache php7.0-bcmath php7.0-soap php7.0-xml curl php-pear php-xml php-mysql php-curl

# imagick
RUN printf "\n" | pecl install imagick           && \
    touch /etc/php/7.0/cli/conf.d/20-imagick.ini && \
    echo "extension=imagick.so" >> /etc/php/7.0/cli/conf.d/20-imagick.ini
    
# mysql
RUN apt-get -y install mysql-server-5.7 && \
    service mysql start                 && \
    mysqladmin -u root password ''      && \
    mysql -u root -e "use mysql; update user set authentication_string=password(''), plugin='mysql_native_password' where user='root';"

# xdebug
RUN wget http://xdebug.org/files/xdebug-2.4.0rc2.tgz && \
    tar -xzf xdebug-2.4.0rc2.tgz                     && \
    cd xdebug-2.4.0RC2/                              && \
    phpize                                           && \
    ./configure --enable-xdebug                      && \
    make                                             && \
    cp modules/xdebug.so /usr/lib/.                  && \
    touch /etc/php/7.0/cli/conf.d/20-xdebug.ini      && \
    echo 'zend_extension="/usr/lib/xdebug.so"' > /etc/php/7.0/cli/conf.d/20-xdebug.ini && \
    echo 'xdebug.remote_enable=1' >> /etc/php/7.0/cli/conf.d/20-xdebug.ini             && \
    cd ../

# composer
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
