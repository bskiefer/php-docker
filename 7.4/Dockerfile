FROM php:7.4-fpm
ENV DEBIAN_FRONTEND noninteractive

RUN apt-get update && apt-get install -y libmcrypt-dev default-libmysqlclient-dev libsodium-dev libzip-dev zip ssh-client ca-certificates zlib1g-dev openssl \
    && pecl install mcrypt-1.0.3 \
    && docker-php-ext-enable mcrypt \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install bcmath \
    && docker-php-ext-configure zip \
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

# PhpLoc
RUN apt-get install wget && wget https://phar.phpunit.de/phploc.phar \
    && mv phploc.phar /bin/phploc \
    && chmod +x /bin/phploc

# PhpStan
RUN wget https://github.com/phpstan/phpstan/raw/master/phpstan.phar \
    && mv phpstan.phar /bin/phpstan \
    && chmod +x /bin/phpstan

# PhpMetrics
RUN wget https://github.com/phpmetrics/PhpMetrics/raw/master/releases/phpmetrics.phar \
    && mv phpmetrics.phar /bin/phpmetrics \
    && chmod +x /bin/phpmetrics

# PhpCS
RUN wget https://squizlabs.github.io/PHP_CodeSniffer/phpcs.phar \
    && mv phpcs.phar /bin/phpcs \
    && chmod +x /bin/phpcs

# Psalm
RUN wget https://github.com/psalm/phar/raw/master/psalm.phar \
    && mv psalm.phar /bin/psalm \
    && chmod +x /bin/psalm

# PhpCpd
RUN wget https://phar.phpunit.de/phpcpd.phar \
    && mv phpcpd.phar /bin/phpcpd \
    && chmod +x /bin/phpcpd

# PhpMd
RUN wget https://phpmd.org/static/latest/phpmd.phar \
    && mv phpmd.phar /bin/phpmd \
    && chmod +x /bin/phpmd

# Rector
RUN wget https://github.com/rectorphp/rector-prefixed/raw/master/rector.phar \
    && mv rector.phar /bin/rector \
    && chmod +x /bin/rector

# Composer Security Checker
RUN wget https://get.sensiolabs.org/security-checker.phar \
    && mv security-checker.phar /bin/security-checker \
    && chmod +x /bin/security-checker

# GH Commenter
RUN wget https://github.com/cloudposse/github-commenter/releases/download/0.5.0/github-commenter_linux_amd64 \
    && mv github-commenter_linux_amd64 /bin/gh-commenter \
    && chmod +x /bin/gh-commenter


CMD ["/bin/bash"]
COPY docker-php-entrypoint /usr/local/bin/
ENTRYPOINT ["docker-php-entrypoint"]