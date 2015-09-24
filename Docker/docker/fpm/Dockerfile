FROM php:5.6-fpm
MAINTAINER Amelin Anton <antonamru@gmail.com>

# Install modules
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libicu-dev \
                --no-install-recommends \
    && docker-php-ext-install mcrypt intl mbstring pdo_mysql mysql mysqli opcache \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

RUN pecl install -o -f xdebug \
    && rm -rf /tmp/pear \
    && echo "zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20131226/xdebug.so" > /usr/local/etc/php/conf.d/xdebug.ini \
    && echo "mbstring.func_overload=2 \nmbstring.internal_encoding=UTF-8 \ndate.timezone = Europe/Moscow \nmax_execution_time = 900 \npost_max_size = 1024M \nmemory_limit = 512M \nupload_max_filesize = 1024M \nmax_input_vars = 10000 \nexpose_php = Off \nrealpath_cache_size=8192K" >  /usr/local/etc/php/conf.d/bitrix.ini \
    && echo "zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20131226/opcache.so \nopcache.enable=1 \nopcache.memory_consumption=124M \nopcache.interned_strings_buffer=8 \nopcache.max_accelerated_files=4000 \nopcache.max_wasted_percentage=5 \nopcache.validate_timestamps=1 \nopcache.revalidate_freq=0 \nopcache.fast_shutdown=1 \nopcache.blacklist_filename=/etc/php.d/opcache*.blacklist" > /usr/local/etc/php/conf.d/opcache.ini \
    && pecl install apcu-beta && echo extension=apcu.so > /usr/local/etc/php/conf.d/apcu.ini 

RUN apt-get purge -y g++ \
    && apt-get autoremove -y \
    && rm -r /var/lib/apt/lists/* \
    && rm -rf /tmp/*

RUN sed -i '/error_log /c \
	error_log = /var/log/php5-fpm/php5.6-fpm.log' /usr/local/etc/php-fpm.conf

RUN mkdir -p /var/log/php5-fpm && \
    chown -R www-data:www-data /var/log/php5-fpm

RUN usermod -u 1000 www-data

EXPOSE 9000
CMD ["php-fpm"]
