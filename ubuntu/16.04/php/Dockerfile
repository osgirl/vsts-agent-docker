FROM microsoft/vsts-agent:ubuntu-16.04

# Base line tools
RUN apt-get update && \
    apt-get install -y make zip g++ locales python-software-properties ssh xsltproc && \
    locale-gen en_US.UTF-8 && export LANG=en_US.UTF-8 && \
    add-apt-repository ppa:ondrej/php

# Docker override from the base, so we get a newer version of docker
# Ideally, we take this out and revert back to the base docker images from
# Microsoft
ENV DOCKER_CHANNEL stable
ENV DOCKER_VERSION 18.03.0-ce

RUN set -ex \
 && curl -fL "https://download.docker.com/linux/static/${DOCKER_CHANNEL}/`uname -m`/docker-${DOCKER_VERSION}.tgz" -o docker.tgz \
 && tar --extract --file docker.tgz --strip-components 1 --directory /usr/local/bin \
 && rm docker.tgz \
 && docker -v

ENV DOCKER_COMPOSE_VERSION 1.18.0

RUN set -x \
 && curl -fSL "https://github.com/docker/compose/releases/download/$DOCKER_COMPOSE_VERSION/docker-compose-`uname -s`-`uname -m`" -o /usr/local/bin/docker-compose \
 && chmod +x /usr/local/bin/docker-compose \
 && docker-compose -v


# PHP
ENV PHP_VERSION "7.2"
RUN apt-get update && \
    apt-get install -y php${PHP_VERSION} \
        php${PHP_VERSION}-curl php${PHP_VERSION}-xdebug php${PHP_VERSION}-gd \
        php${PHP_VERSION}-ldap php${PHP_VERSION}-bcmath php${PHP_VERSION}-mbstring \
        php${PHP_VERSION}-soap php${PHP_VERSION}-imap php${PHP_VERSION}-xml php${PHP_VERSION}-zip

COPY override-php.ini /etc/php/${PHP_VERSION}/cli/conf.d/

# Composer
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php --install-dir=/usr/local/bin && \
    php -r "unlink('composer-setup.php');"

# Node
RUN curl -sL https://deb.nodesource.com/setup_9.x | bash - && apt-get install nodejs

# Npm
RUN npm install -g grunt-cli
