FROM ubuntu:latest

MAINTAINER Amin M

ENV DEBIAN_FRONTEND noninteractive
# clean up
RUN apt clean && apt update && apt upgrade -y

# installing required stuff
RUN apt-get update \
    && apt-get install -y unzip libaio-dev libmcrypt-dev git libzip-dev\
		libmcrypt-dev \
		libpng-dev \
    && apt-get clean -y

# php required settings
RUN apt install -y locales
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8

# house keeping
RUN apt install  -y \
	curl zip unzip git software-properties-common

# PHP repos
RUN add-apt-repository ppa:ondrej/php -y
RUN apt update

# PHP install
RUN apt install -y php7.4 php7.4-cli php7.4-common \
    php7.4-mbstring php7.4-xml php7.4-mysqlnd php7.4-pdo-sqlite \
    php7.4-gd php7.4-curl php7.4-zip php7.4-soap php7.4-intl \
    php7.4-fpm php7.4-apcu php7.4-pgsql

# PHP fpm
RUN echo "listen = 0.0.0.0:9000" >> /etc/php/7.4/fpm/pool.d/www.conf
RUN service php7.4-fpm start

# PHP composer
RUN curl -sS https://getcomposer.org/installer | php --  --install-dir=/usr/bin --filename=composer

WORKDIR /var/www/html/

# Node js
RUN curl -sL https://deb.nodesource.com/setup_10.x -o setup_12.x
RUN bash setup_12.x
RUN apt-get install -y nodejs
RUN apt-get install build-essential -y

ADD bash_aliases.sh /usr/sbin/entrypoint.sh
RUN cat /usr/sbin/entrypoint.sh >> ~/.bashrc

CMD ["php-fpm7.4", "-F"]
