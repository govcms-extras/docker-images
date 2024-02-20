# Stage 1: Build Stage
FROM php:8.2-cli-alpine

RUN apk --update --no-cache add \
    sudo \
    curl \
    freetype-dev \
    git \
    libjpeg-turbo-dev \
    libpng-dev \
    libwebp-dev \
    libzip-dev \
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
RUN echo "govcms ALL=(ALL) NOPASSWD: /sbin/apk" | tee /etc/sudoers.d/govcms

# Set environment variables
ENV PATH=/app/bin:/app/vendor/bin:/app/tests/vendor/bin:${PATH}
ENV COMPOSER_MEMORY_LIMIT=-1

USER govcms

# Set working directory
WORKDIR /app