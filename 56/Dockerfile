FROM alpine:3.8
MAINTAINER Marc Ende <me@e-beyond.de>

# Add repos
#RUN echo "http://dl-cdn.alpinelinux.org/alpine/v3.8/testing" >> /etc/apk/repositories

# Add basics first
RUN apk update && apk upgrade && apk add \
	bash apache2 php5-apache2 curl ca-certificates openssl openssh git php5 php5-phar php5-json php5-iconv php5-openssl tzdata

# Setup apache and php
RUN apk add \
	php5-ftp \
	php5-mcrypt \
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
	php5-xmlrpc \
	php5-bz2 \
	php5-pdo_dblib \
	php5-curl \
	php5-ctype \
	php5-exif \
	php5-intl \
	php5-ldap \
	php5-apcu \
    php5-curl \
    php5-imap 
	
# php5-bcmath
# php5-bz2
# php5-calendar
# php5-cgi
# php5-cli
# php5-common
# php5-ctype
# php5-dba
# php5-dbg
# php5-dev
# php5-doc
# php5-dom
# php5-embed
# php5-enchant
# php5-exif
# php5-fpm
# php5-gd
# php5-gettext
# php5-gmp
# php5-intl
# php5-ldap
# php5-mcrypt
# php5-mysql
# php5-mysqli
# php5-opcache
# php5-openssl
# php5-pcntl
# php5-pdo
# php5-pdo_dblib
# php5-pdo_mysql
# php5-pdo_pgsql
# php5-pdo_sqlite
# php5-pear
# php5-pgsql
# php5-phpdbg
# php5-posix
# php5-pspell
# php5-shmop
# php5-snmp
# php5-soap
# php5-sockets
# php5-sqlite3
# php5-suhosin
# php5-sysvmsg
# php5-sysvsem
# php5-sysvshm
# php5-wddx
# php5-xml
# php5-xmlreader
# php5-xmlrpc
# php5-xsl
# php5-zip
	
	

# Problems installing in above stack
#RUN apk add php5-simplexml

RUN cp /usr/bin/php5 /usr/bin/php \
    && rm -f /var/cache/apk/*
    
# Add Composer
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer

# Add apache to run and configure
RUN sed -i "s/#LoadModule\ rewrite_module/LoadModule\ rewrite_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_module/LoadModule\ session_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_cookie_module/LoadModule\ session_cookie_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ session_crypto_module/LoadModule\ session_crypto_module/" /etc/apache2/httpd.conf \
    && sed -i "s/#LoadModule\ deflate_module/LoadModule\ deflate_module/" /etc/apache2/httpd.conf \
    && sed -i "s#^DocumentRoot \".*#DocumentRoot \"/app/public\"#g" /etc/apache2/httpd.conf \
    && sed -i "s#/var/www/localhost/htdocs#/app/public#" /etc/apache2/httpd.conf \
    && sed -i "s#^ErrorLog .*#ErrorLog /dev/stderr#g" /etc/apache2/httpd.conf \
    && sed -i "s#CustomLog .*#CustomLog /dev/stdout combined#g" /etc/apache2/httpd.conf \
    && sed -i '/^xfs.*$/d' /etc/group \
    && sed -i '/^xfs.*$/d' /etc/passwd \
    && sed -i 's/^apache:x:101/apache:x:33/' /etc/group \
    && sed -i 's/^apache:x:100:101/apache:x:33:33/' /etc/passwd \
    && printf "\n<Directory \"/app/public\">\n\tAllowOverride All\n</Directory>\n" >> /etc/apache2/httpd.conf

RUN mkdir /app && mkdir /app/public && chown -R apache:apache /app && chmod -R 755 /app && mkdir bootstrap
ADD start.sh /bootstrap/
RUN chmod +x /bootstrap/start.sh
RUN mkdir -p /run/apache2 && chown -R apache:apache /run/apache2
EXPOSE 80
ENTRYPOINT ["/bootstrap/start.sh"]