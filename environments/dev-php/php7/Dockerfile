FROM ubuntu:xenial

RUN apt update -yqq                    && \
    apt install -y apache2                \
                   openssh-server         \
                   php                    \
                   php-pear               \
                   php-xdebug             \
                   libapache2-mod-php     \
                   php-mcrypt             \
                   php-mysql              \
                   php-mbstring           \
                   php-intl               \
                   php-sqlite3            \
                   php-curl               \
                   php-gd                 \
                   supervisor             \
                   git                    \
                   zip                    \
                   unzip                  \
                   wget                   \
                   openssl             && \
    apt clean
RUN wget https://phar.phpunit.de/phpunit.phar && \
  chmod +x phpunit.phar                       && \
  mv phpunit.phar /usr/bin/phpunit

ADD ./start.sh /start.sh
RUN mkdir -p /var/lock/apache2 /var/run/apache2 /var/run/sshd /var/log/supervisor && \
    mkdir -p /etc/supervisor/conf.d                                               && \
    echo "date.timezone=Europe/Madrid" >> /etc/php/7.0/apache2/php.ini            && \
    echo "date.timezone=Europe/Madrid" >> /etc/php/7.0/cli/php.ini
ADD ./supervisord.conf /etc/supervisor/conf.d/supervisord.conf
ADD apache-default.conf /etc/apache2/sites-available/000-default.conf
ADD club.dev.conf /etc/apache2/sites-available/club.dev.conf

RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf && \
    a2enmod rewrite                                          && \
    a2enmod headers                                          && \
    a2enmod deflate                                          && \
    a2enmod env                                              && \
    a2enmod expires                                          && \
    a2ensite club.dev                                        && \
    useradd -u 1000 -m jrojo                                 && \
    useradd jenkins                                          && \
    echo "jenkins:jenkins" | chpasswd

#VOLUME ["/var/www/html", "/var/log/apache2"]

EXPOSE 22 80
CMD ["/bin/bash", "/start.sh"]
