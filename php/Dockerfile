FROM php:8.1-fpm-alpine

RUN docker-php-ext-install pdo pdo_mysql

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
