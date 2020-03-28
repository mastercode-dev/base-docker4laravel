FROM php:7.4-fpm
LABEL maintainer="Vanderlei Sbaraini Amancio <hello@vanderleis.me>"

# Set Environment Variables
ENV DEBIAN_FRONTEND noninteractive

### Add helper install script

ADD https://raw.githubusercontent.com/mlocati/docker-php-extension-installer/master/install-php-extensions /usr/local/bin/
RUN chmod uga+x /usr/local/bin/install-php-extensions && sync

### Install required packages

USER root

RUN set -eux; \
    apt-get update && \
    apt-get install -y gnupg2 && \
    apt-get install -y --no-install-recommends \
    g++ \
    curl \
    software-properties-common \
    python3 \
    libmemcached-dev \
    libz-dev \
    libpq-dev \
    libjpeg-dev \
    libpng-dev \
    libfreetype6-dev \
    libssl-dev \
    libmcrypt-dev \
    zlib1g-dev \
    libicu-dev \
    libcurl4-openssl-dev \
    libmagickwand-dev \
    python3-pip \
    libonig-dev; \
    rm -rf /var/lib/apt/lists/*

### Install general PHP extensions

RUN install-php-extensions bcmath intl mcrypt pcntl pdo_pgsql pgsql redis zip

# Install graphical extensions

RUN install-php-extensions exif gd imagick

### Install image optimizers

#RUN apt-get install -y jpegoptim optipng pngquant gifsicle

# Install PHP Redis Extensions
RUN pecl install -o -f redis && \
    rm -rf /tmp/pear && \
    docker-php-ext-enable redis

# Install WKHTMLTOPDF
RUN apt update && \
    apt install -y \
    libxrender1 \
    libfontconfig1 \
    libx11-dev \
    libjpeg62 \
    libxtst6 \
    wget \
    && wget https://github.com/h4cc/wkhtmltopdf-amd64/blob/master/bin/wkhtmltopdf-amd64?raw=true -O /usr/local/bin/wkhtmltopdf \
    && chmod +x /usr/local/bin/wkhtmltopdf

# Install Ansible

USER root

RUN apt-get update && \
    apt-get -y install ansible && \
    apt-get -y install python3-pip && \
    pip3 install docker

# Install Supervisor

USER root

RUN apt-get install -y supervisor

COPY supervisord.conf /etc/supervisord.conf
COPY supervisord.d /etc/supervisord.d

# Configure locales

RUN apt-get install -y locales && \
    echo '' >> /usr/share/locale/locale.alias && \
    temp="${ADDITIONAL_LOCALES%\"}" && \
    temp="${temp#\"}" && \
    for i in ${temp}; do sed -i "/$i/s/^#//g" /etc/locale.gen; done && \
    locale-gen

ARG LOCALE=POSIX
ENV LC_ALL=${LOCALE}

# Install Composer

USER root

RUN curl -sS https://getcomposer.org/installer | php && \
    mv composer.phar /usr/local/bin/composer

# Install nodejs

USER root

RUN apt remove -y cmdtest && \
    curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - && \
    echo "deb https://dl.yarnpkg.com/debian/ stable main" | tee /etc/apt/sources.list.d/yarn.list && \
    curl -sL https://deb.nodesource.com/setup_13.x | bash - && \
    apt install -y nodejs yarn

# Install git
RUN apt install -y git

# Run command

CMD ["/usr/bin/supervisord"]

WORKDIR /app
EXPOSE 9000
