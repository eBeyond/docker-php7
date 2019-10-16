FROM alpine:3.8
MAINTAINER Marc Ende <me@e-beyond.de>

# Add repos
#RUN echo "http://dl-cdn.alpinelinux.org/alpine/v3.8/testing" >> /etc/apk/repositories

# Add basics first
RUN apk update && apk upgrade && apk add \
	bash apache2 php5-apache2 curl ca-certificates openssl openssh git php5 php5-phar php5-json php5-iconv php5-openssl tzdata openntpd nano

# Add Composer
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer

# Setup apache and php
RUN apk add \
	php5-ftp \
	php5-xdebug \
	php5-mcrypt \
	php5-mbstring \
	php5-soap \
	php5-gmp \
	php5-pdo_odbc \
	php5-dom \
	php5-pdo \
	php5-zip \
	php5-mysqli \
	php5-sqlite3 \
	php5-pdo_pgsql \
	php5-bcmath \
	php5-gd \
	php5-odbc \
	php5-pdo_mysql \
	php5-pdo_sqlite \
	php5-gettext \
	php5-xml \
	php5-xmlreader \
	php5-xmlwriter \
	php5-tokenizer \
	php5-xmlrpc \
	php5-bz2 \
	php5-pdo_dblib \
	php5-curl \
	php5-ctype \
	php5-session \
	php5-redis \
	php5-exif \
	php5-intl \
	php5-fileinfo \
	php5-ldap \
	php5-apcu

# Problems installing in above stack
RUN apk add php5-simplexml

RUN cp /usr/bin/php5 /usr/bin/php \
    && rm -f /var/cache/apk/*

# Add apache to run and configure
RUN sed -i "s/#LoadModule\ rewrite_module/LoadModule\ rewrite_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_module/LoadModule\ session_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_cookie_module/LoadModule\ session_cookie_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_crypto_module/LoadModule\ session_crypto_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ deflate_module/LoadModule\ deflate_module/" /etc/apache2/httpd.conf \
    && sed -i "s#^DocumentRoot \".*#DocumentRoot \"/app/public\"#g" /etc/apache2/httpd.conf \
    && sed -i "s#/var/www/localhost/htdocs#/app/public#" /etc/apache2/httpd.conf \
    && printf "\n<Directory \"/app/public\">\n\tAllowOverride All\n</Directory>\n" >> /etc/apache2/httpd.conf

RUN mkdir /app && mkdir /app/public && chown -R apache:apache /app && chmod -R 755 /app && mkdir bootstrap
ADD start.sh /bootstrap/
RUN chmod +x /bootstrap/start.sh

EXPOSE 80
ENTRYPOINT ["/bootstrap/start.sh"]