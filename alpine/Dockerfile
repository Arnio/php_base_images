FROM alpine:3.10
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone 
RUN apk update && \
    apk upgrade && \
    apk add openssl curl ca-certificates 
RUN set -eux; \
	addgroup -g 82 -S nginx; \
	adduser -u 82 -D -S -G nginx nginx
RUN printf "%s%s%s\n" \
"http://nginx.org/packages/alpine/v" \
`egrep -o '^[0-9]+\.[0-9]+' /etc/alpine-release` \
"/main" \
| tee -a /etc/apk/repositories 
RUN curl -o /tmp/nginx_signing.rsa.pub https://nginx.org/keys/nginx_signing.rsa.pub && mv /tmp/nginx_signing.rsa.pub /etc/apk/keys/
RUN apk add --no-cache composer \
            bash \
            nginx \
            git \
            unzip \
            php7 \
            php7-fpm \
            php7-pdo_mysql \
            php7-session \
            php7-simplexml \
            php7-xmlwriter \
            php7-opcache \
            php7-tokenizer \
            php7-common \
            php7-mbstring \
            php7-xml \
            php7-soap \
            php7-dom \
            php7-gd \
            php7-xdebug \
            php7-curl && \
    rm -rf /var/cache/apk/* 

RUN mkdir -p /var/www/html /var/run/php/ && \
    sed -i -e 's/;cgi.fix_pathinfo=1/cgi.fix_pathinfo=0/g' /etc/php7/php.ini 

RUN curl -sS https://getcomposer.org/installer -o composer-setup.php && \
    php composer-setup.php --install-dir=/usr/local/bin --filename=composer 

COPY ./nginx.conf /etc/nginx/nginx.conf
COPY ./default /etc/nginx/conf.d/default.conf
COPY ./www.conf /etc/php7/php-fpm.d/www.conf
COPY ./start.sh /usr/local/bin/
COPY ./info.php /var/www/html/index.php
RUN chown -R nginx:nginx /var/www/html && \
    chmod u+x /usr/local/bin/start.sh
EXPOSE 80
ENTRYPOINT [ "start.sh" ]
