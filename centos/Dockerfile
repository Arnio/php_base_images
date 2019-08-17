FROM centos:7
RUN yum -y install yum-plugin-fastestmirror httpd epel-release && \
    yum -y install http://rpms.remirepo.net/enterprise/remi-release-7.rpm && \
    yum-config-manager --enable remi-php73 

RUN yum install -y nginx \
                    curl \
                    git \
                    unzip \
                    php \
                    php-fpm \
                    php-pdo_mysql \
                    php-session \
                    php-simplexml \
                    php-xmlwriter \
                    php-opcache \
                    php-tokenizer \
                    php-common \
                    php-mbstring \
                    php-xml \
                    php-soap \
                    php-dom \
                    php-gd \
                    php-xdebug \
                    php-curl && \
    yum  -y clean all && \
    rm -rf /var/cache/yum && \
    rm -rf /var/tmp/yum-*

RUN sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' /etc/php.ini && \
    mkdir -p /var/www/html /var/run/php/ /var/run/php-fpm/ && \
    curl -sS https://getcomposer.org/installer -o composer-setup.php && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer 

COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./default /etc/nginx/conf.d/default.conf
COPY ./www.conf /etc/php-fpm.d/www.conf
COPY ./start.sh /usr/local/bin/
COPY ./info.php /var/www/html/index.php

RUN chmod u+x /usr/local/bin/start.sh
EXPOSE 80 
ENTRYPOINT [ "start.sh" ]