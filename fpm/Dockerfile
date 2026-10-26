FROM wordpress:7.1.3-php8.3-fpm-alpine

# Override default PHP-FPM pool config with our tuned version
COPY fpm/www.conf /usr/local/etc/php-fpm.d/www.conf

# Add custom PHP runtime settings (memory, OPcache, upload limits)
COPY php/wp-custom.ini /usr/local/etc/php/conf.d/wp-custom.ini
