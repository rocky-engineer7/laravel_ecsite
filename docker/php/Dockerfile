FROM php:8.3-fpm

# 必要な依存関係をインストール
RUN apt-get update && apt-get install -y \
    git \
    zip \
    unzip \
    vim \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libmcrypt-dev \
    libpng-dev \
    libfontconfig1 \
    libxrender1

# GD ライブラリの設定とインストール
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install gd bcmath pdo_mysql mysqli exif

# Composer のインストール
RUN cd /usr/bin && curl -s https://getcomposer.org/installer | php && ln -s /usr/bin/composer.phar /usr/bin/composer

# Node.js のインストール
ENV NODE_VERSION=20.15.0
RUN curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.5/install.sh | bash
ENV NVM_DIR=/root/.nvm
RUN . "$NVM_DIR/nvm.sh" && nvm install ${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm use v${NODE_VERSION}
RUN . "$NVM_DIR/nvm.sh" && nvm alias default v${NODE_VERSION}
ENV PATH="/root/.nvm/versions/node/v${NODE_VERSION}/bin/:${PATH}"

# 作業ディレクトリの設定
WORKDIR /var/www/html

# 必要な権限設定
RUN mkdir -p storage bootstrap/cache && \
    chown -R www-data:www-data storage bootstrap/cache && \
    chmod -R 775 storage bootstrap/cache
