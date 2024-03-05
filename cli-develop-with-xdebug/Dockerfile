ARG PHP_VERSION="7.4.33"

FROM php:${PHP_VERSION}-cli-alpine

ARG USER_ID=1000

ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
RUN chmod uga+x /usr/local/bin/install-php-extensions && \
    install-php-extensions xdebug && \
    runDeps="$( \
    		scanelf --needed --nobanner --format '%n#p' --recursive /usr/local/lib/php/extensions \
    			| tr ',' '\n' | sort -u | awk 'system("[ -e /usr/local/lib/" $1 " ]") == 0 { next } { print "so:" $1 }'  \
        )"; \
      # Save Runtime Deps in a virtual deps
    	apk add --no-network --virtual .php-extensions-rundeps $runDeps; \
      # Uninstall Everything we Installed (minus the runtime Deps)
     #	apk del --no-network .build-deps && \
    docker-php-ext-enable xdebug

COPY --from=composer /usr/bin/composer /usr/bin/composer

#RUN composer config platform.php ${PHP_VERSION}

# ------------------------------------------------- Permissions --------------------------------------------------------

# - Clean bundled config/users & recreate them with UID 1000 for docker compatability in dev container.
# - Create composer directories (since we run as non-root later)
RUN deluser --remove-home www-data && adduser -u$USER_ID -D www-data && rm -rf /var/www /usr/local/etc/php-fpm.d/* && \
    mkdir -p /var/www/.composer /app && chown -R www-data:www-data /app /var/www/.composer

#RUN pecl install xdebug-3.2.1 \
#	&& docker-php-ext-enable xdebug