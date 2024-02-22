# Stage 1: Build Stage
FROM php:8.2-fpm-alpine AS builder

RUN apk --update --no-cache add \
    sudo \
    curl \
    freetype-dev \
    git \
    libjpeg-turbo-dev \
    libpng-dev \
    libwebp-dev \
    libzip-dev \
    nginx \
    nodejs \
    npm \
    patch \
    rsync \
    sqlite-dev \
    icu-dev \
    tzdata \
    zip \
    && rm -rf /var/cache/apk/*

# Config PHP extension
RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp

# Install PHP extensions
RUN docker-php-ext-install gd intl opcache pdo_mysql pdo_sqlite zip

# Set the timezone to Australia/Sydney
RUN ln -sf /usr/share/zoneinfo/Australia/Sydney /etc/localtime

# Copy Composer from the Composer Docker image
COPY --from=composer:lts /usr/bin/composer /usr/local/bin/composer

# Create the govcms group and user
RUN addgroup -g 1000 govcms && adduser -u 1000 -D -S -G govcms -s /bin/sh govcms

# Create a sudoers file for govcms
RUN echo "govcms ALL=(ALL) NOPASSWD: /sbin/apk, /bin/chown" | tee /etc/sudoers.d/govcms

# Set ownership and permissions for nginx
RUN chown -R govcms.govcms /run \
    && chown -R govcms.govcms /var/lib/nginx \
    && chown -R govcms.govcms /var/log/nginx

# Configure PHP-FPM
COPY conf/www.conf /usr/local/etc/php-fpm.d/www.conf
# Delete the www.conf.default file
RUN rm /usr/local/etc/php-fpm.d/www.conf.default

# Configure Nginx
COPY conf/nginx.conf /etc/nginx/nginx.conf
COPY conf/site.conf /etc/nginx/conf.d/default.conf

# Expose port 80
EXPOSE 80

USER govcms

# Set working directory
WORKDIR /app

# Use the official PHP FPM Alpine image
FROM builder

# Set environment variables
ENV PATH=/app/bin:/app/vendor/bin:/app/tests/vendor/bin:${PATH}
ENV COMPOSER_MEMORY_LIMIT=-1
ENV SIMPLETEST_BASE_URL='http://localhost'
ENV SIMPLETEST_DB='sqlite://localhost/sites/default/files/.ht.sqlite'
ENV CI=1
ENV CYPRESS_BASE_URL=http://localhost

# Set environment variable for govcms version
ENV GOVCMS_VERSION=dev-3.x-develop
ENV GOVCMS_TESTS_VERSION=3.x-dev

# Set up working directory
WORKDIR /app

# Run Composer to create project and tests
RUN composer create-project govcms/govcms:${GOVCMS_VERSION} . --prefer-dist --remove-vcs --no-install && \
    composer create-project govcms/tests:${GOVCMS_TESTS_VERSION} tests --prefer-dist --remove-vcs --no-install

# Start PHP-FPM and Nginx
CMD ["sh", "-c", "php-fpm & nginx -g 'daemon off;'"]