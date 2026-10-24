FROM phpmyadmin/phpmyadmin:5.2.3 AS pma

FROM php:8.3.36-apache-bookworm

RUN set -ex; \
    apt-get update; \
    apt-get install -y --no-install-recommends \
        libbz2-dev \
        libfreetype6-dev \
        libjpeg-dev \
        libpng-dev \
        libwebp-dev \
        libxpm-dev \
        libzip-dev \
    ; \
    docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp --with-xpm; \
    docker-php-ext-install -j "$(nproc)" \
        bz2 \
        gd \
        mysqli \
        opcache \
        zip \
        bcmath \
    ; \
    a2enmod remoteip; \
    apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false; \
    rm -rf /var/lib/apt/lists/*

ENV PMA_SSL_DIR=/etc/phpmyadmin/ssl \
    MAX_EXECUTION_TIME=600 \
    MEMORY_LIMIT=512M \
    UPLOAD_LIMIT=256M \
    TZ=America/Bogota \
    SESSION_SAVE_PATH=/sessions

RUN set -ex; \
    mkdir -p "$SESSION_SAVE_PATH" "$PMA_SSL_DIR"; \
    chmod 1777 "$SESSION_SAVE_PATH"; \
    chmod 755 "$PMA_SSL_DIR"; \
    { \
        echo 'allow_url_fopen=Off'; \
        echo 'max_execution_time=600'; \
        echo 'max_input_vars=10000'; \
        echo 'memory_limit=512M'; \
        echo 'post_max_size=256M'; \
        echo 'upload_max_filesize=256M'; \
        echo 'date.timezone=America/Bogota'; \
        echo 'session.save_path=/sessions'; \
    } > "$PHP_INI_DIR/conf.d/phpmyadmin-misc.ini"

COPY --from=pma --chown=www-data:www-data /var/www/html /var/www/html
COPY --from=pma --chown=www-data:www-data /etc/phpmyadmin /etc/phpmyadmin
COPY --from=pma /usr/local/etc/php/conf.d/opcache-recommended.ini /usr/local/etc/php/conf.d/
COPY --from=pma /usr/local/etc/php/conf.d/session-strict.ini /usr/local/etc/php/conf.d/
COPY --from=pma /docker-entrypoint.sh /docker-entrypoint.sh

RUN chown www-data:www-data "$PMA_SSL_DIR" "$SESSION_SAVE_PATH" \
    && chmod +x /docker-entrypoint.sh

ENTRYPOINT ["/docker-entrypoint.sh"]
CMD ["apache2-foreground"]
