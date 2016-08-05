FROM nginx:latest

COPY config/nginx.conf /etc/nginx/nginx.conf
COPY config/laravel.conf /etc/nginx/sites-available/
RUN rm -fr /etc/nginx/conf.d/*

RUN echo "upstream php-upstream { server php-fpm:9000; }" > /etc/nginx/conf.d/upstream.conf

RUN usermod -u 1000 www-data

CMD ["nginx"]

EXPOSE 80 443
