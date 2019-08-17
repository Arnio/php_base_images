FROM ubuntu:latest

RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone && \
    apt list --upgradable && apt-get -y update && apt-get -y remove --purge apache2 && \
    apt-get -y install software-properties-common && \
    add-apt-repository -y ppa:ondrej/php 
RUN	useradd -u 82 -MrU nginx && usermod -L nginx
RUN apt-get -y install nginx \
                       curl \
                       git \
                       unzip \
                       php7.3 && \
    apt-get -y install php7.3-fpm \
                       php7.3-simplexml \
                       php7.3-xmlwriter \
                       php7.3-opcache \
                       php7.3-tokenizer \
                       php7.3-common \
                       php7.3-mbstring \
                       php7.3-xml \
                       php7.3-soap \
                       php7.3-intl \
                       php7.3-dom \
                       php7.3-gd \
                       php7.3-xdebug \
                       php7.3-mysql \
                       php7.3-curl && \
    apt-get -y clean && rm -rf /var/lib/apt/lists/*
RUN sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' /etc/php/7.3/fpm/php.ini && \
    curl -sS https://getcomposer.org/installer -o composer-setup.php && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer 


COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./default /etc/nginx/conf.d/default.conf
COPY ./www.conf /etc/php/7.3/fpm/pool.d/www.conf

COPY ./info.php /var/www/html/index.php

COPY ./start.sh /usr/local/bin/

RUN chmod u+x /usr/local/bin/start.sh
EXPOSE 80 
ENTRYPOINT [ "start.sh" ]
