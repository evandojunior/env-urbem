FROM ubuntu:latest

RUN apt-get update && apt-get install -y software-properties-common language-pack-en-base \
    && LC_ALL=en_US.UTF-8 add-apt-repository -y ppa:ondrej/php \
    && apt-get update \
	&& apt-get install -y php5.6-fpm php5.6-cli php5.6-curl \
	php5.6-json \
	php5.6-mcrypt \
    php5.6-bcmath \
	php-memcache \
	php-memcached \
	php-mysqlnd-ms \
	php5.6-mysql \
	php-redis \
	php5.6-sqlite3 \
	php-stomp \
	php5.6-intl \
	php5.6-pgsql \
	php-xdebug \
    php5.6-gd \
	php5.6-dev \
    php5.6-xml \
    php5.6-mbstring \
    php5.6-zip \
	php-pear \
    php5.6-soap \
	wkhtmltopdf \
	xvfb \
	sendmail supervisor \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN apt-get update && apt-get install -y supervisor
COPY ./conf/php/supervisord.conf /etc/supervisor/conf.d/supervisord.conf

RUN apt-get update && apt-get install -y nginx git zip unzip postgresql-contrib mysql-client

RUN sed -i -e 's/listen = \/run\/php\/php5.6-fpm.sock/listen = 0.0.0.0:9000/g' /etc/php/5.6/fpm/pool.d/www.conf \
    && sed -i -e 's/;daemonize = yes/daemonize = no/g' /etc/php/5.6/fpm/php-fpm.conf

# Creates a launcher for wkhtmltopdf under xvfb
RUN echo 'xvfb-run -- /usr/bin/wkhtmltopdf "${@:-}"' > /usr/local/bin/wkhtmltopdf && \
	chmod +x /usr/local/bin/wkhtmltopdf

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Ruby
RUN apt-add-repository -y ppa:brightbox/ruby-ng && apt-get update && apt-get install -y ruby2.3 ruby2.3-dev
RUN gem install bunny

COPY ./conf/php/cli-php.ini /etc/php/5.6/cli/php.ini
#COPY ./conf/php/php-fpm.conf /etc/php/5.6/fpm/php-fpm.conf
COPY ./conf/php/php.ini /etc/php/5.6/fpm/php.ini
COPY ./conf/php/www.conf /etc/php/5.6/fpm/pool.d/www.conf
ADD ./conf/nginx/fastcgi_params /etc/nginx/fastcgi_params

RUN rm /etc/nginx/sites-available/default && rm /etc/nginx/sites-enabled/default

RUN touch ~/.pgpass
RUN echo "postgrehost:5432:urbem:docker:docker" >> ~/.pgpass
RUN echo "postgrehost:5432:urbem-transparencia:docker:docker" >> ~/.pgpass
RUN echo "postgrehost:5432:etl:docker:docker" >> ~/.pgpass
RUN chmod 600 ~/.pgpass

COPY ./entrypointRedeSimples.sh /sbin/entrypointRedeSimples.sh
RUN chmod 755 /sbin/entrypointRedeSimples.sh

COPY ./importRedeSimples.sh /sbin/importRedeSimples.sh
RUN chmod 755 /sbin/importRedeSimples.sh

COPY ./importTransparencia.sh /sbin/importTransparencia.sh
RUN chmod 755 /sbin/importTransparencia.sh

COPY ./importUrbem.sh /sbin/importUrbem.sh
RUN chmod 755 /sbin/importUrbem.sh

COPY ./importETL.sh /sbin/importETL.sh
RUN chmod 755 /sbin/importETL.sh

# removing deb files
RUN apt-get clean

EXPOSE 80

CMD ["/usr/bin/supervisord"]