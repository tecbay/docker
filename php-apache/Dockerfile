FROM php:7.2-apache

RUN apt-get update

# Enable apache rewrite mode
RUN a2enmod rewrite 

#install some basic tools
RUN apt-get install -y \
        git \
        vim 


#setup composer
RUN curl -sS https://getcomposer.org/installer | php \
        && mv composer.phar /usr/local/bin/ \
        && ln -s /usr/local/bin/composer.phar /usr/local/bin/composer


#install some base extensions
RUN apt-get install -y \
        libzip-dev \
        zip \
  && docker-php-ext-configure zip --with-libzip \
  && docker-php-ext-install zip pdo_mysql mysqli

# install google gRPC
RUN pecl install grpc


# install Supervisor and corn
RUN apt-get install -y\ 
        supervisor\
        cron\
        openssh-server

        # Copy supervisor file in the supervisor directory

RUN mkdir -p /var/lock/apache2 /var/run/apache2 /var/run/sshd /var/log/supervisor

COPY laravel-queue.conf /etc/supervisor/conf.d/laravel-queue.conf
                
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf



        # Setup corn job  +++++++++ >>>>    
COPY crontab /etc/cron.d/laravel-cron

# Give execution rights on the cron job
RUN chmod 0755 /etc/cron.d/laravel-cron

# Apply cron job
RUN crontab /etc/cron.d/laravel-cron

# Cean up
RUN apt-get clean

WORKDIR /var/www/html