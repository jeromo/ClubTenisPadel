FROM ubuntu:xenial

RUN apt update -yqq                                                                                            && \
    apt install -y apache2                                                                                        \
                   openssh-server                                                                                 \
                   php                                                                                            \
                   php-pear                                                                                       \
                   php-xdebug                                                                                     \
                   libapache2-mod-php                                                                             \
                   php-mcrypt                                                                                     \
                   php-mysql                                                                                      \
                   php-mbstring                                                                                   \
                   php-intl                                                                                       \
                   php-sqlite3                                                                                    \
                   php-curl                                                                                       \
                   php-gd                                                                                         \
                   supervisor                                                                                     \
                   git                                                                                            \
                   zip                                                                                            \
                   unzip                                                                                          \
                   wget                                                                                           \
                   apt-transport-https                                                                            \
                   ca-certificates                                                                                \
                   openssl                                                                                        \
                   node.js                                                                                        \
                   npm                                                                                         && \ 
    npm install -g bower                                                                                       && \
    wget https://phar.phpunit.de/phpunit.phar                                                                  && \
    chmod +x phpunit.phar                                                                                      && \
    mv phpunit.phar /usr/bin/phpunit                                                                           && \
    wget https://sonarsource.bintray.com/Distribution/sonar-scanner-cli/sonar-scanner-cli-3.0.1.733-linux.zip  && \
    useradd -u 1000 -m jrojo                                                                                   && \
    useradd  -m jenkins                                                                                        && \
    echo "jenkins:jenkins" | chpasswd                                                                          && \
    cd /home/jenkins                                                                                           && \
    unzip /sonar-scanner-cli-3.0.1.733-linux.zip                                                               && \
    chmod +x sonar-scanner-3.0.1.733-linux/bin/sonar-scanner                                                   && \
    rm /sonar-scanner-cli-3.0.1.733-linux.zip                                                                  && \
    apt clean


ENV PATH ${PATH}:/home/jenkins/sonar-scanner-3.0.1.733-linux/bin
ADD ./start.sh /start.sh
RUN mkdir -p /var/lock/apache2 /var/run/apache2 /var/run/sshd /var/log/supervisor && \
    mkdir -p /etc/supervisor/conf.d                                               && \
    echo "date.timezone=Europe/Madrid" >> /etc/php/7.0/apache2/php.ini            && \
    echo "date.timezone=Europe/Madrid" >> /etc/php/7.0/cli/php.ini                && \
    mkdir /data
ADD ./supervisord.conf /etc/supervisor/conf.d/supervisord.conf
ADD apache-default.conf /etc/apache2/sites-available/000-default.conf
ADD club.dev.conf /etc/apache2/sites-available/club.dev.conf
ADD prueba.dev.conf /etc/apache2/sites-available/prueba.dev.conf
ADD ClubTenisPadelServer_parameters.yml /data
RUN echo "ServerName localhost" >> /etc/apache2/apache2.conf && \
    a2enmod rewrite                                          && \
    a2enmod headers                                          && \
    a2enmod deflate                                          && \
    a2enmod env                                              && \
    a2enmod expires                                          && \
    a2ensite club.dev                                        && \
    a2ensite prueba.dev                                      && \
    ln -s /usr/bin/nodejs /usr/bin/node 

#VOLUME ["/var/www/html", "/var/log/apache2"]

EXPOSE 22 80
CMD ["/bin/bash", "/start.sh"]
