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
RUN apt install -y php7.3 php7.3-cli php7.3-common \
    php7.3-mbstring php7.3-xml php7.3-mysqlnd php7.3-pdo-sqlite \
    php7.3-gd php7.3-curl php7.3-zip php7.3-soap php7.3-intl \
    php7.3-fpm php7.3-apcu

# PHP fpm
RUN echo "listen = 0.0.0.0:9000" >> /etc/php/7.3/fpm/pool.d/www.conf
RUN service php7.3-fpm start

# PHP composer
RUN curl -sS https://getcomposer.org/installer | php --  --install-dir=/usr/bin --filename=composer

WORKDIR /var/www/html/

RUN apt install -y nodejs npm

ADD bash_aliases.sh /usr/sbin/entrypoint.sh
RUN cat /usr/sbin/entrypoint.sh >> ~/.bashrc

CMD ["php-fpm7.3", "-F"]
