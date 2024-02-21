# Stage 1: Build Stage
FROM php:8.2-fpm AS builder

# Set the timezone to Australia/Sydney
RUN ln -sf /usr/share/zoneinfo/Australia/Sydney /etc/localtime

# Install additional packages
RUN apt-get update && \
    apt-get install -y \
    sudo \
    curl \
    libfreetype6-dev \
    git \
    libjpeg-dev \
    libpng-dev \
    libwebp-dev \
    libzip-dev \
    nginx \
    nodejs \
    npm \
    patch \
    rsync \
    sqlite3 \
    libicu-dev \
    tzdata \
    zip \
    libgtk2.0-0 libgtk-3-0 libgbm-dev \
    libnotify-dev \
    libgconf-2-4 libnss3 libxss1 \
    libasound2 libxtst6 xauth xvfb \
    locales \
    mariadb-client \
    openssh-client \
    supervisor \ 
    && \
    rm -rf /var/lib/apt/lists/*

# Clean up the installation files
RUN apt-get autoclean && apt-get autoremove && rm -rf /var/lib/apt/lists/*

# Config PHP extension
RUN docker-php-ext-configure gd --with-freetype --with-jpeg --with-webp

# Install PHP extensions
RUN docker-php-ext-install -j "$(nproc)" gd intl opcache pdo_mysql zip

# Copy Composer from the Composer Docker image
COPY --from=composer:lts /usr/bin/composer /usr/local/bin/composer

# Create the govcms group and user
RUN groupadd -g 1000 govcms && useradd -u 1000 -g govcms -m govcms

# Create a sudoers file for govcms
RUN echo "govcms ALL=(ALL) NOPASSWD: /usr/bin/apt-get" | tee /etc/sudoers.d/govcms

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

# Copy supervisord.conf from the conf directory
COPY conf/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Copy the scripts to a separate directory
COPY --chown=govcms:govcms scripts /scripts

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

# Expose port 80
EXPOSE 80

USER govcms

# Set up working directory
WORKDIR /app

# Set entrypoint
ENTRYPOINT ["/scripts/setup-govcms"]

# Start Supervisor with the configured processes
CMD ["supervisord", "-c", "/etc/supervisor/conf.d/supervisord.conf"]