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
    apache2 \
    libapache2-mod-php5.6 \
    openssl && \
    apt-get clean && \
    chmod 755 /*.sh && \
    mkdir -p /etc/supervisor/conf.d && \
    echo "ServerName localhost" >> /etc/apache2/apache2.conf && \
    phpenmod mcrypt && \
    phpenmod curl && \
    a2enmod rewrite && \
    a2enmod headers && \
    a2enmod deflate && \
    a2enmod env && \
    a2enmod expires && \
    chmod +x /install-composer.sh && \
    /install-composer.sh && \
    mv composer.phar /usr/bin/composer

RUN apt-get update && apt-get install -y php5-mcrypt default-libmysqlclient-dev libsodium-dev libzip-dev zip ssh-client ca-certificates zlib1g-dev openssl \
    && pecl install mcrypt-1.0.1 \
    && docker-php-ext-enable mcrypt \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install sodium \
    && docker-php-ext-install bcmath \
    && docker-php-ext-configure zip --with-libzip \
    && docker-php-ext-install zip

RUN pecl install redis \
  && docker-php-ext-enable redis

RUN apt-get update && \
    apt-get install libldap2-dev -y && \
    rm -rf /var/lib/apt/lists/* && \
    docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap

# Install Composer
RUN apt-get update && \
    apt-get install -y --no-install-recommends git zip

RUN curl --silent --show-error https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

ENV PATH /root/.composer/vendor/bin:$PATH

RUN composer global require hirak/prestissimo