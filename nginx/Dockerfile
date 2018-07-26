FROM nginx:1.15-alpine

MAINTAINER Igor Pinchuk <i.pinchuk.work@gmail.com>

RUN addgroup -g 1000 -S alpine-www-data \
    && adduser -u 1000 -D -S -G alpine-www-data alpine-www-data

RUN rm /etc/nginx/conf.d/default.conf

RUN mkdir etc/nginx/sites-available
RUN mkdir etc/nginx/sites-enabled

ADD nginx.conf /etc/nginx/

ADD bitrix.conf /etc/nginx/sites-available
RUN ln -s /etc/nginx/sites-available/bitrix.conf /etc/nginx/sites-enabled/bitrix.conf

RUN echo "upstream php-fpm-upstream { server php-fpm:9000; }" > /etc/nginx/conf.d/upstream.conf

CMD ["nginx"]

#docker-compose exec nginx sh
WORKDIR "/var/www"

EXPOSE 80
EXPOSE 443