# Yarn commands

-   `yarn composer.install` Install dependencies locally.
-   `yarn composer.update` Updates dependencies.
-   `yarn composer.require [package name]` Installs a package (and adds it to composer.json).
-   `yarn composer.dump-autoload` Generate autoloading files.

# Examples

## Setting the debug env variable

XDEBUG_MODE=debug docker compose -f docker-compose.dev.yaml up -d

## Using a local env file

docker compose -f docker-compose.dev.yaml --env-file .env.local up --build -d

## Accessing shell inside docker container

docker exec -it php-docker-app-1 sh
