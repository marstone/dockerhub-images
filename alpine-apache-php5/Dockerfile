FROM alpine:3.5

MAINTAINER marstone <i@marstone.net>

# disable interactive functions
# ENV DEBIAN_FRONTEND noninteractive

# Install basics first
RUN apk add --update --no-cache \
    bash curl ca-certificates openssl openssh git tzdata openntpd nano \
    apache2 apache2-utils apache2-webdav \
    mod_dav_svn subversion \
	python \
	php5-apache2 php5 \
    php5-phar \
    php5-json \
    php5-iconv \
    php5-openssl \
    php5-ftp \
    php5-xdebug \
    php5-mcrypt \
    php5-soap \
    php5-gmp \
    php5-pdo_odbc \
    php5-dom \
    php5-pdo \
    php5-zip \
    php5-mysql \
    php5-mysqli \
    php5-sqlite3 \
    php5-pdo_pgsql \
    php5-bcmath \
    php5-gd \
    php5-odbc \
    php5-pdo_mysql \
    php5-pdo_sqlite \
    php5-pear \
    php5-gettext \
    php5-xmlreader \
    php5-xmlrpc \
    php5-bz2 \
    php5-pdo_dblib \
    php5-curl \
    php5-ctype \
    php5-exif \
    php5-zlib

# Install pear File_Passwd & patch it
RUN pear channel-update pear.php.net \
    && pear upgrade-all \
    && pear config-set auto_discover 1 \
    && pear install File_Passwd --alldeps

RUN sed -i "/define('FILE_PASSWD_PLAIN'/a define('PEAR_ZE2', false);" /usr/share/pear/File/Passwd.php 


# Install composer
RUN curl -sS https://getcomposer.org/installer | php && mv composer.phar /usr/local/bin/composer

# Add apache to run and configure
RUN mkdir -p /run/apache2 \
 && sed -i "s/#LoadModule\ rewrite_module/LoadModule\ rewrite_module/" /etc/apache2/httpd.conf \
 && sed -i "s/#LoadModule\ session_module/LoadModule\ session_module/" /etc/apache2/httpd.conf \
 && sed -i "s/#LoadModule\ session_cookie_module/LoadModule\ session_cookie_module/" /etc/apache2/httpd.conf \
 && sed -i "s/#LoadModule\ session_crypto_module/LoadModule\ session_crypto_module/" /etc/apache2/httpd.conf \
 && sed -i "s/#LoadModule\ deflate_module/LoadModule\ deflate_module/" /etc/apache2/httpd.conf \
 && sed -i "s#^DocumentRoot \".*#DocumentRoot \"/app/public\"#g" /etc/apache2/httpd.conf \
 && sed -i "s#/var/www/localhost/htdocs#/app/public#" /etc/apache2/httpd.conf \
 && printf "\n<Directory \"/app/public\">\n\tAllowOverride All\n</Directory>\n" >> /etc/apache2/httpd.conf

ADD ["entrypoint.sh", "/"]

RUN mkdir -p /app/public \
 && chown -R apache:apache /app \
 && chmod -R 755 /app \
 && chmod +x /entrypoint.sh

EXPOSE 80
ENTRYPOINT ["/entrypoint.sh"]


