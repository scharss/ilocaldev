# Usar una imagen base de PHP con Apache (elige la versión de PHP que necesites)
FROM php:8.2-apache

# Variables de entorno (opcional, pero útil)
ENV COMPOSER_ALLOW_SUPERUSER=1
ENV COMPOSER_HOME="/tmp"

# Actualizar lista de paquetes e instalar dependencias del sistema y herramientas comunes
RUN apt-get update && apt-get install -y \
    git \
    curl \
    libzip-dev \
    zip \
    unzip \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libonig-dev \
    libxml2-dev \
    libicu-dev \
    # Dependencias para extensiones específicas (ej. imagick)
    # libmagickwand-dev --no-install-recommends \
    && rm -rf /var/lib/apt/lists/*

# Instalar extensiones PHP
# - pdo_mysql y mysqli: para conexión a MySQL
# - gd: para manipulación de imágenes (necesaria para códigos QR)
# - zip: para trabajar con archivos .zip (necesaria para librerías de Excel como PhpSpreadsheet)
# - intl: para internacionalización (requerida por algunas librerías)
# - mbstring: para manipulación de strings multibyte
# - exif: para metadatos de imágenes
# - opcache: para mejorar el rendimiento de PHP
# - bcmath: para matemáticas de precisión arbitraria
# - soap: para servicios web SOAP
# - xml: para trabajar con XML

RUN docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) \
    pdo_mysql \
    mysqli \
    gd \
    zip \
    intl \
    mbstring \
    exif \
    opcache \
    bcmath \
    soap \
    xml

# Instalar PECL extensions (ejemplo: imagick, xdebug - descomentar si es necesario)
# RUN pecl install imagick && docker-php-ext-enable imagick
# RUN pecl install xdebug && docker-php-ext-enable xdebug

# Instalar Composer (gestor de dependencias para PHP)
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Habilitar mod_rewrite de Apache (para URLs amigables, ej. en frameworks)
RUN a2enmod rewrite

# Establecer el directorio de trabajo
WORKDIR /var/www/html

# Opcional: Cambiar el propietario del directorio web al usuario de Apache para evitar problemas de permisos
# Esto es útil si vas a instalar dependencias con Composer desde dentro del contenedor o si la app escribe archivos.
# RUN chown -R www-data:www-data /var/www/html
# USER www-data # Ejecutar como usuario www-data

# Exponer el puerto (ya lo hace la imagen base de Apache)
# EXPOSE 80