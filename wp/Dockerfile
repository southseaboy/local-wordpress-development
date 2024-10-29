FROM wordpress:6.6.2-fpm-alpine

## Installing WP CLI
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
RUN chmod +x wp-cli.phar
RUN mv wp-cli.phar /usr/local/bin/wp

## Bash
RUN apk add --no-cache bash

