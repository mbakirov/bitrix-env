FROM php:5.6-cli
MAINTAINER Amelin Anton <antonamru@gmail.com>

RUN apt-get update && DEBIAN_FRONTEND=noninteractive apt-get --yes install \
        git \
        openssl \
        curl \
        wget \
        libicu-dev \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libicu-dev

RUN docker-php-ext-install zip mcrypt intl mbstring pdo_mysql mysql mysqli \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd

RUN curl -sL https://deb.nodesource.com/setup_0.12 | bash -

#RUN DEBIAN_FRONTEND=noninteractive apt-get --yes install \
#        nodejs \
#        build-essential \
#        default-jre

#RUN curl -sL https://www.npmjs.org/install.sh | bash -

RUN apt-get autoremove -y \
        && rm -r /var/lib/apt/lists/*

# RUN npm install -g uglifyjs yuicompressor

# Install composer && global asset plugin
ENV COMPOSER_HOME /root/.composer
ENV PATH /root/.composer/vendor/bin:$PATH
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer && \
    /usr/local/bin/composer global require "fxp/composer-asset-plugin"