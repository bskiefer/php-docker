FROM ubuntu:xenial
MAINTAINER Leonel Baer <leonel@lysender.com>

# Install packages
ENV DEBIAN_FRONTEND noninteractive
ENV LANG C.UTF-8

# Combined RUN commands to reduce layer size
RUN apt-get update && \
    apt-get install -y software-properties-common && \
    add-apt-repository ppa:ondrej/php && \
    apt-get update && \
    apt-get install -y \
    wget \
    supervisor \
    php5.6 \
    php5.6-cgi \
    php5.6-cli \
    php5.6-common \
    php5.6-fpm \
    php5.6-mysql \
    php5.6-mbstring \
    php5.6-curl \
    php5.6-dev \
    php5.6-gd \
    php5.6-xml \
    php5.6-mcrypt \
    php5.6-xmlrpc \
    php5.6-zip \
    libxrender1  \
    libxext6 \
    libfontconfig1 \
    wkhtmltopdf \
    xvfb \
    git \
    curl \
    openssl && \
    apt-get clean && \
    mkdir -p /etc/supervisor/conf.d && \
    phpenmod mcrypt && \
    phpenmod curl

RUN apt-get clean

RUN apt-get update && apt-get install -y libsodium-dev libzip-dev zip ssh-client ca-certificates zlib1g-dev openssl

RUN apt-get install php5.6-redis -y

RUN apt-get update && \
    apt-get install libldap2-dev -y

# Install Composer
RUN apt-get update && \
    apt-get install -y --no-install-recommends git zip

RUN curl --silent --show-error https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ENV PATH /root/.composer/vendor/bin:$PATH