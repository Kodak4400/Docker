FROM php:7.1-apache

RUN \
  { apt-get update; \
    apt-get install -y zlib1g-dev libzip-dev; \
    docker-php-ext-install zip; \
    docker-php-ext-install mysqli; \
    docker-php-ext-configure zip; \
    docker-php-ext-configure mysqli ; \
    ln -s /etc/apache2/mods-available/rewrite.load /etc/apache2/mods-enabled/rewrite.load; \
  }

RUN \
    if [ ! -f /usr/local/lib/php/extensions/no-debug-non-zts-20160303/xdebug.so ]; then \
        pecl install xdebug; \
    fi; \
    { \
        echo 'zend_extension=/usr/local/lib/php/extensions/no-debug-non-zts-20160303/xdebug.so'; \
        echo '[xdebug]'; \
        echo 'xdebug.remote_enable=1'; \
        echo 'xdebug.remote_autostart=1'; \
        echo 'xdebug.remote_host=host.docker.internal'; \
        echo 'xdebug.remote_port=9000'; \
    } > /usr/local/etc/php/conf.d/php-xdebug.ini ;

# wp-cliインストール
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
    && chmod +x wp-cli.phar \
    && mv wp-cli.phar /usr/local/bin/wp