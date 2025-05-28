FROM nginx:1.22

RUN groupadd -g 1000 app \
 && useradd -g 1000 -u 1000 -d /var/www -s /bin/bash app
RUN touch /var/run/nginx.pid
RUN mkdir /sock

COPY ./nginx.conf /etc/nginx/
COPY ./default.conf /etc/nginx/conf.d/

RUN mkdir -p /var/www/html
RUN chown -R app:app /etc/nginx /var/www /var/cache/nginx /var/run/nginx.pid /sock

# Node.js
# RUN curl -sL https://deb.nodesource.com/setup_14.x -o nodesource_setup.sh
# RUN bash nodesource_setup.sh
# RUN apt-get install nodejs -y
# RUN npm install npm@6 -g
# RUN command -v node
# RUN command -v npm

EXPOSE 8000

USER app:app

VOLUME /var/www

WORKDIR /var/www/html