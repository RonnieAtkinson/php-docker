FROM php:8.1-fpm-alpine as app

# Useful PHP extension installer image, copy binary into your container
COPY --from=mlocati/php-extension-installer /usr/bin/install-php-extensions /usr/local/bin/

# Install php extensions
# exit on errors, exit on unset variables, print every command as it is executed
RUN set -eux; \
    install-php-extensions pdo pdo_mysql;

# Allow super user - set this if you use Composer as a
# super user at all times like in docker containers.
ENV COMPOSER_ALLOW_SUPERUSER=1

# Obtain composer using multi-stage build
# https://docs.docker.com/build/building/multi-stage
#
# The /usr/bin directory is used to contain user 
# command binaries which perform important tasks.
#
# This composer binary is what enables use to run
# composer commands. 
COPY --from=composer:2.4 /usr/bin/composer /usr/bin/composer

# Here we are copying only composer.json and composer.lock
# right before doing the composer install.
#
# This is enough to take advantage of docker cache and 
# composer install will be executed only when composer.lock
# or composer.lock have changed.
COPY ./app/composer.* ./

# Install
RUN composer install --prefer-dist --no-dev --no-scripts --no-progress --no-interaction

# Copy application files to the working directory.
COPY ./app .

# Run composer dump-autoload --optimize
RUN composer dump-autoload --optimize

# Dev image
# This stage is meant to be target-built into a separate image
# https://docs.docker.com/develop/develop-images/multistage-build/#stop-at-a-specific-build-stage
# https://docs.docker.com/compose/compose-file/#target
FROM app as app_dev

# Xdebug has different modes / functionalities. We can default to 'off' and set to 'debug'
# when we run docker compose up if we need it, see readme.
ENV XDEBUG_MODE=off

# Copy xdebug config file into container
#COPY ./php/conf.d/xdebug.ini $PHP_INI_DIR/conf.d/xdebug.ini
COPY ./php/conf.d/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini

# Install xdebug
RUN set -eux; \
	install-php-extensions xdebug