FROM debian:stretch-slim

RUN apt-get update && apt-get install curl -y
RUN curl -L -O https://github.com/phpbrew/phpbrew/releases/latest/download/phpbrew.phar && chmod +x phpbrew.phar && mv phpbrew.phar /usr/local/bin/phpbrew

RUN apt install ca-certificates apt-transport-https wget gnupg2 -y
RUN wget -q https://packages.sury.org/php/apt.gpg -O- | apt-key add -
RUN echo "deb https://packages.sury.org/php/ stretch main" | tee /etc/apt/sources.list.d/php.list
RUN apt-get update && apt-get install php7.4 -y
RUN apt-get install php7.4-bz2 -y


ENV PHPIZE_DEPS \
		autoconf \
		dpkg-dev \
		file \
		g++ \
		gcc \
		libc-dev \
		make \
		pkg-config \
		re2c

# persistent / runtime deps
RUN set -eux; \
	apt-get update; \
	apt-get install -y --no-install-recommends \
		$PHPIZE_DEPS \
		ca-certificates \
		curl \
		xz-utils \
	; \
	rm -rf /var/lib/apt/lists/*


RUN apt-get update && apt-get install -y libmcrypt-dev default-libmysqlclient-dev libsodium-dev libzip-dev zip ssh-client ca-certificates zlib1g-dev openssl libxml2-dev sudo curl libbz2-dev libcurl4-openssl-dev pkg-config libssl-dev libreadline-dev libxslt-dev

RUN curl -L -O https://github.com/phpbrew/phpbrew/releases/latest/download/phpbrew.phar && chmod +x phpbrew.phar && mv phpbrew.phar /usr/local/bin/phpbrew

RUN phpbrew init
RUN echo "[[ -e ~/.phpbrew/bashrc ]] && source ~/.phpbrew/bashrc" >> ~/.bashrc
RUN echo 'export PHPBREW_ROOT="/root/.phpbrew"' >> ~/.bashrc
RUN echo 'export PHPBREW_HOME=/root/.phpbrew' >> ~/.bashrc
RUN phpbrew install --stdout 7.0 as 7.0
RUN phpbrew install --stdout 7.2 as 7.2
RUN phpbrew install --stdout 7.3 as 7.3
RUN phpbrew install --stdout 7.4 as 7.4

RUN echo 'alias php70=/root/.phpbrew/php/7.0/bin/php' >> ~/.bashrc
RUN echo 'alias php72=/root/.phpbrew/php/7.2/bin/php' >> ~/.bashrc
RUN echo 'alias php73=/root/.phpbrew/php/7.3/bin/php' >> ~/.bashrc
RUN echo 'alias php74=/root/.phpbrew/php/7.4/bin/php' >> ~/.bashrc

RUN apt-get update && apt-get install -y libmcrypt-dev default-libmysqlclient-dev libsodium-dev libzip-dev zip ssh-client ca-certificates zlib1g-dev openssl php7.4-cli php7.4-common php7.4-curl php7.4-mbstring php7.4-mysql php7.4-xml
RUN curl --silent --show-error https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH /root/.composer/vendor/bin:$PATH
RUN composer global require hirak/prestissimo
RUN apt-get install git -y && git --version
WORKDIR /root

CMD ["/bin/bash"]
COPY docker-php-entrypoint /usr/local/bin/
ENTRYPOINT ["docker-php-entrypoint"]