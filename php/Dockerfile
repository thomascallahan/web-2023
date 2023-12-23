FROM php:7.4.8-fpm-alpine

CMD ["php-fpm"]

# ADD https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions /usr/local/bin/
# RUN chmod +x /usr/local/bin/install-php-extensions && \
#     install-php-extensions gd

RUN docker-php-ext-install mysqli && docker-php-ext-enable mysqli

# add GD extension
RUN apk add --no-cache \
      freetype \
      libjpeg-turbo \
      libpng \
      freetype-dev \
      libjpeg-turbo-dev \
      libpng-dev \
    && docker-php-ext-configure gd \
      --with-freetype=/usr/include/ \
      # --with-png=/usr/include/ \ # No longer necessary as of 7.4; https://github.com/docker-library/php/pull/910#issuecomment-559383597
      --with-jpeg=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd \
    && docker-php-ext-enable gd \
    && apk del --no-cache \
      freetype-dev \
      libjpeg-turbo-dev \
      libpng-dev \
    && rm -rf /tmp/*

# add ZIP extension
RUN apk add --no-cache \
        libzip-dev \
        zip \
  && docker-php-ext-install zip

# add EXIF extension
RUN docker-php-ext-install exif

# add imagick extension
RUN apk add  --no-cache \
      imagemagick \
      libgomp \
      imagemagick-dev
RUN apk add --update --no-cache autoconf g++ imagemagick-dev libtool make pcre-dev \
    && pecl install imagick \
    && docker-php-ext-enable imagick \
    && apk del autoconf g++ libtool make pcre-dev

# add intl extension
RUN apk add  --no-cache \
      icu-dev \
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-enable intl

# add composer
COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer

RUN composer require phpmailer/phpmailer

EXPOSE 9000
