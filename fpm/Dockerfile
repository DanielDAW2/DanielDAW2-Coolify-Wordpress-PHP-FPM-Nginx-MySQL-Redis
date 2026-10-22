FROM wordpress:7.1.3-php8.3-fpm-alpine

# PHP runtime patched for CVE-2026-109618 / CVE-2026-109619 ("php2shell"),
# fixed in PHP 8.3.36 (also 8.5.12 / 8.4.27 / 8.2.35). The previous pin
# (wordpress:6.7-php8.3-fpm-alpine) had PHP < 8.3.36 and is no longer
# rebuilt with new PHP runtime releases.

# Security hardening against CVE-2026-109618 / CVE-2026-109619 (php2shell):
# - memory_limit set via php_admin_value cannot be overridden by WP plugins/ini_set
#   (removes the reachable memory-limit failure path; the container memory limit
#   in docker-compose.yml is the real containment boundary)
# - session.upload_progress.enabled=0 in an FPM pool-level conf (not .user.ini,
#   which the advisory says must not be relied on) blocks the confirmed PHP-FPM
#   attack path of CVE-2026-109619, which is enabled by default
RUN printf '[www]\nphp_admin_value[memory_limit] = 256M\nphp_admin_value[session.upload_progress.enabled] = 0\n' \
	> /usr/local/etc/php-fpm.d/z-oom-hardening.conf

# Override default PHP-FPM pool config with our tuned version
COPY fpm/www.conf /usr/local/etc/php-fpm.d/www.conf

# Add custom PHP runtime settings (memory, OPcache, upload limits)
COPY php/wp-custom.ini /usr/local/etc/php/conf.d/wp-custom.ini
